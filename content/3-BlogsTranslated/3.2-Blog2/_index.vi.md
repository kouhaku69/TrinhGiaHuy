---
title: "Blog 2"
date: "2026-07-31"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Tự động bật/tắt EC2 bằng Amazon EventBridge Scheduler mà không cần viết Lambda

**Biên soạn từ AWS Compute Blog, AWS DevOps Blog và AWS Documentation** | Chủ đề: Amazon EventBridge Scheduler, EC2, Automation, Cost Optimization, IAM

---

Một trong những cách đơn giản nhất để lãng phí tiền trên AWS là để môi trường development chạy 24/7 dù team chỉ làm việc khoảng 8–10 giờ mỗi ngày.

Giả sử một EC2 instance chỉ cần hoạt động:

```text
Thứ Hai - Thứ Sáu
08:00 - 18:00
```

Nếu chúng ta nhớ tắt thủ công mỗi ngày thì không có vấn đề. Nhưng thực tế, “nhớ tắt server” không phải là một control đáng tin cậy.

Amazon EventBridge Scheduler giải quyết bài toán này bằng cách cho phép tạo lịch một lần hoặc định kỳ và gọi trực tiếp API của nhiều AWS service. Với universal target, chúng ta có thể gọi `EC2 StartInstances` và `StopInstances` mà không nhất thiết phải viết một Lambda function chỉ để chạy vài dòng SDK.

Nguồn chính:

- [AWS Compute Blog – Introducing Amazon EventBridge Scheduler](https://aws.amazon.com/blogs/compute/introducing-amazon-eventbridge-scheduler/)
- [AWS DevOps Blog – EventBridge Scheduler L2 Construct](https://aws.amazon.com/blogs/devops/announcing-the-general-availability-of-the-amazon-eventbridge-scheduler-l2-construct/)
- [AWS Documentation – Amazon EventBridge Scheduler](https://docs.aws.amazon.com/eventbridge/latest/userguide/using-eventbridge-scheduler.html)
- [AWS What’s New – EventBridge Scheduler adds 619 new SDK API actions](https://aws.amazon.com/about-aws/whats-new/2026/05/amazon-eventbridge-sdk-integrations/)

---

## 1. Tại sao không dùng cron trên EC2?

Một cách quen thuộc là:

```bash
crontab -e
```

Sau đó tạo script gọi AWS CLI.

Vấn đề là server chạy cron cũng phải tồn tại, được patch, có credential và phải đủ ổn định để chính nó không trở thành single point of failure.

Một cách khác là:

```text
EventBridge Rule
-> Lambda
-> EC2 API
```

Cách này hoạt động tốt, nhưng nếu Lambda chỉ tồn tại để gọi đúng một API `StopInstances`, chúng ta đang thêm một lớp compute và code không thật sự cần thiết.

EventBridge Scheduler có thể đơn giản hóa thành:

```text
Schedule
-> EC2 StopInstances API
```

---

## 2. EventBridge Scheduler là gì?

EventBridge Scheduler là một serverless scheduler được AWS quản lý.

Nó hỗ trợ ba dạng biểu thức chính:

```text
at(...)
rate(...)
cron(...)
```

### One-time

Ví dụ chạy đúng một lần:

```text
at(2026-08-01T10:00:00)
```

### Rate

Ví dụ chạy mỗi 15 phút:

```text
rate(15 minutes)
```

### Cron

Ví dụ chạy lúc 18:00 từ thứ Hai đến thứ Sáu:

```text
cron(0 18 ? * MON-FRI *)
```

Một ưu điểm rất thực tế là Scheduler hỗ trợ **time zone**, nên ta có thể đặt:

```text
Asia/Ho_Chi_Minh
```

thay vì tự quy đổi lịch sang UTC.

---

## 3. Kiến trúc lab

Mục tiêu:

```text
08:00 -> Start EC2
18:00 -> Stop EC2
Monday-Friday
Asia/Ho_Chi_Minh
```

Luồng:

```text
EventBridge Scheduler
        |
        | assumes execution role
        v
       IAM Role
        |
        | ec2:StartInstances
        | ec2:StopInstances
        v
     EC2 Instance
```

Chúng ta cần **hai schedule**:

1. `start-dev-ec2`
2. `stop-dev-ec2`

---

## 4. Bước 1: Chuẩn bị EC2

Tạo một EC2 instance dùng cho lab.

Lưu lại Instance ID:

```text
i-0123456789abcdef0
```

Không nên thử trực tiếp trên production instance.

---

## 5. Bước 2: Tạo IAM execution role cho Scheduler

EventBridge Scheduler cần assume một IAM role để gọi API EC2.

### Trust Policy

Role phải tin cậy service principal:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "scheduler.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Permission Policy

Chỉ cấp quyền đúng hai action cần thiết và giới hạn resource vào instance cụ thể:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:<region>:<account-id>:instance/i-0123456789abcdef0"
    }
  ]
}
```

Điểm cần nhớ:

> Scheduler không tự có quyền stop EC2. Nó chỉ làm được những gì execution role cho phép.

Đây chính là tư duy least privilege tương tự khi làm service role ở các dịch vụ AWS khác.

---

## 6. Bước 3: Tạo lịch Start EC2 lúc 08:00

Trong AWS Console:

```text
Amazon EventBridge
-> Scheduler
-> Create schedule
```

Điền:

```text
Name: start-dev-ec2
Schedule type: Recurring
Cron: cron(0 8 ? * MON-FRI *)
Time zone: Asia/Ho_Chi_Minh
Flexible time window: Off
```

Ở phần target, chọn EC2 API tương ứng với:

```text
StartInstances
```

Input:

```json
{
  "InstanceIds": [
    "i-0123456789abcdef0"
  ]
}
```

Chọn execution role vừa tạo.

---

## 7. Bước 4: Tạo lịch Stop EC2 lúc 18:00

Tạo schedule thứ hai:

```text
Name: stop-dev-ec2
Cron: cron(0 18 ? * MON-FRI *)
Time zone: Asia/Ho_Chi_Minh
```

Target:

```text
EC2 StopInstances
```

Input:

```json
{
  "InstanceIds": [
    "i-0123456789abcdef0"
  ]
}
```

Sau đó dùng cùng execution role.

---

## 8. Vì sao cách này có thể tối ưu chi phí?

Nếu một môi trường development chỉ cần chạy 10 giờ/ngày, 5 ngày/tuần, tổng thời gian hoạt động khoảng:

```text
10 x 5 = 50 giờ/tuần
```

Trong khi chạy 24/7:

```text
24 x 7 = 168 giờ/tuần
```

Tỷ lệ thời gian compute hoạt động còn:

```text
50 / 168 ≈ 29.8%
```

Nghĩa là về mặt **thời gian EC2 chạy**, ta loại bỏ hơn 70% số giờ không cần thiết.

Điều này không có nghĩa hóa đơn tổng sẽ chắc chắn giảm đúng 70%, vì:

- EBS volume vẫn có chi phí khi instance stop;
- Elastic IP/Public IPv4 có thể có chi phí;
- snapshot, data transfer và dịch vụ khác vẫn tính riêng;
- workload thực tế có thể cần chạy ngoài giờ.

Nhưng với môi trường dev/test, scheduling là một trong những cách dễ nhất để loại bỏ idle compute.

---

## 9. Không chỉ EC2

Điểm mạnh của EventBridge Scheduler là target không giới hạn ở Lambda.

AWS hỗ trợ templated targets và universal targets cho rất nhiều service/API operation.

Các use case có thể là:

```text
Scheduler -> SNS
Scheduler -> SQS
Scheduler -> Lambda
Scheduler -> Step Functions
Scheduler -> ECS task
Scheduler -> AWS service API
```

Tháng 5/2026, AWS tiếp tục mở rộng Scheduler với thêm hàng trăm SDK API action, giúp nhiều tác vụ có thể được lên lịch trực tiếp mà không phải viết lớp integration code riêng.

---

## 10. Reliability: Đừng quên retry và DLQ

Một scheduler production không nên chỉ nghĩ tới “đúng giờ”.

Cần nghĩ thêm:

```text
Nếu API call thất bại thì sao?
Nếu target tạm unavailable thì sao?
Nếu request retry nhiều lần thì sao?
```

EventBridge Scheduler hỗ trợ:

- retry policy;
- event retention;
- flexible time window;
- dead-letter queue (Amazon SQS);
- encryption.

Với workload quan trọng, nên cấu hình DLQ để không “mất dấu” một schedule thất bại.

Ngoài ra, vì EventBridge Scheduler cung cấp delivery theo mô hình **at-least-once**, target nên được thiết kế để chịu được trường hợp invocation lặp lại.

---

## 11. Một lỗi mình thấy rất dễ mắc: IAM quá rộng

Đừng tạo execution role như:

```json
{
  "Effect": "Allow",
  "Action": "ec2:*",
  "Resource": "*"
}
```

chỉ vì “cho nhanh”.

Với lab một instance, role hoàn toàn có thể giới hạn:

```text
Action:
- ec2:StartInstances
- ec2:StopInstances

Resource:
- đúng ARN của instance
```

Một schedule tự động có quyền rộng là một rủi ro lớn hơn một thao tác thủ công, vì nó có thể chạy lặp lại mà không có người quan sát.

---

## 12. Khi nào nên dùng EventBridge Scheduler?

Phù hợp khi:

- task có lịch rõ ràng;
- cần one-time hoặc recurring schedule;
- muốn gọi AWS service/API mà không duy trì server cron;
- cần timezone;
- muốn retry/DLQ được AWS quản lý;
- có nhiều lịch cần quản lý tập trung.

Ví dụ:

- bật/tắt EC2 dev;
- gửi reminder qua SNS;
- chạy Step Functions mỗi đêm;
- kích hoạt ECS task định kỳ;
- đóng một workflow tạm thời sau ngày hết hạn;
- gửi event một lần trong tương lai.

---

## 13. Khi nào không nên dùng?

Không nên dùng Scheduler như một công cụ autoscaling theo tải.

Nếu EC2 cần scale theo CPU, request count hoặc queue depth, hãy dùng:

- EC2 Auto Scaling;
- Application Auto Scaling;
- ECS Service Auto Scaling;
- các cơ chế scaling chuyên dụng khác.

Scheduler trả lời câu hỏi:

> “Khi nào chạy?”

Auto Scaling trả lời:

> “Cần bao nhiêu capacity?”

Hai bài toán khác nhau.

---

## 14. Dọn dẹp

Sau khi lab:

1. Xóa hai schedules.
2. Xóa IAM execution role nếu không còn sử dụng.
3. Terminate EC2 instance demo.
4. Kiểm tra EBS volume, Elastic IP/Public IPv4 và snapshot.
5. Kiểm tra Billing/Cost Explorer.

Nếu tạo one-time schedule cho use case khác, có thể cấu hình `ActionAfterCompletion=DELETE` để schedule tự xóa sau khi hoàn tất.

---

## Tài liệu tham khảo

- AWS Compute Blog: [Introducing Amazon EventBridge Scheduler](https://aws.amazon.com/blogs/compute/introducing-amazon-eventbridge-scheduler/)
- AWS DevOps Blog: [Announcing the General Availability of the Amazon EventBridge Scheduler L2 Construct](https://aws.amazon.com/blogs/devops/announcing-the-general-availability-of-the-amazon-eventbridge-scheduler-l2-construct/)
- AWS Documentation: [Amazon EventBridge Scheduler](https://docs.aws.amazon.com/eventbridge/latest/userguide/using-eventbridge-scheduler.html)
- AWS Documentation: [Managing targets in EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-targets.html)
- AWS What’s New: [EventBridge Scheduler adds 619 new SDK API actions](https://aws.amazon.com/about-aws/whats-new/2026/05/amazon-eventbridge-sdk-integrations/)

---

## Kết luận

Một automation tốt không nhất thiết phải có Lambda, container hoặc một server chạy cron.

Nếu công việc chỉ là:

```text
Đúng giờ
-> gọi một AWS API
```

thì EventBridge Scheduler có thể là lớp phù hợp nhất.

Bài học mình rút ra là: **trước khi viết thêm code cho automation, hãy kiểm tra xem AWS managed service có thể thực hiện trực tiếp tác vụ đó hay không**. Ít code hơn thường đồng nghĩa với ít thứ phải deploy, monitor và debug hơn.
