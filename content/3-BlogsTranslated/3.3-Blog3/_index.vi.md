---
title: "Blog 3"
date: "2026-07-31"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Amazon S3 Lifecycle: Tự động giảm chi phí lưu trữ mà không cần quản lý thủ công

**Biên soạn từ AWS Documentation và AWS Storage Blog**  
Chủ đề: Amazon S3, Cost Optimization, Storage

---

Khi sử dụng Amazon S3, một vấn đề rất dễ gặp là dữ liệu ngày càng nhiều nhưng không phải dữ liệu nào cũng cần được truy cập thường xuyên.

Ví dụ, một hệ thống lưu log mỗi ngày:

- Log trong 30 ngày gần nhất thường xuyên được xem.
- Log cũ hơn ít khi được sử dụng.
- Log sau một năm có thể không còn cần thiết.

Nếu tất cả dữ liệu đều nằm mãi trong cùng một storage class, chi phí lưu trữ có thể tăng dần theo thời gian.

**Amazon S3 Lifecycle** giúp tự động xử lý vấn đề này.

---

## S3 Lifecycle là gì?

S3 Lifecycle cho phép chúng ta tạo các **rule** để Amazon S3 tự động quản lý object theo tuổi của dữ liệu.

Hai hành động quan trọng nhất là:

### 1. Transition

Tự động chuyển object sang storage class khác khi dữ liệu ít được sử dụng hơn.

Ví dụ:

```text
Ngày 0
S3 Standard

      ↓ sau 30 ngày

S3 Standard-IA

      ↓ sau 90 ngày

S3 Glacier Flexible Retrieval
```

### 2. Expiration

Tự động xóa object khi dữ liệu không còn cần thiết.

Ví dụ:

```text
Sau 365 ngày
→ Xóa log
```

Như vậy, chúng ta không cần mỗi tháng tự kiểm tra và di chuyển từng file.

---

## Ví dụ thực tế

Giả sử một website lưu log vào:

```text
s3://my-website-logs/
```

Mỗi ngày hệ thống tạo thêm hàng nghìn file.

Ta có thể đặt lifecycle như sau:

```text
0–30 ngày
→ S3 Standard

30–90 ngày
→ S3 Standard-IA

Sau 90 ngày
→ S3 Glacier Flexible Retrieval

Sau 365 ngày
→ Expire
```

Ý tưởng rất đơn giản:

> Dữ liệu càng ít được sử dụng thì chuyển sang lớp lưu trữ phù hợp hơn.

---

## Cách tạo S3 Lifecycle Rule

### Bước 1: Mở bucket

Trong AWS Console:

```text
Amazon S3
→ Buckets
→ Chọn bucket
```

### Bước 2: Mở Lifecycle rules

Chọn:

```text
Management
→ Lifecycle rules
→ Create lifecycle rule
```

### Bước 3: Đặt tên rule

Ví dụ:

```text
archive-old-logs
```

Ta có thể áp dụng rule cho:

- toàn bộ bucket;
- một prefix;
- các object có tag cụ thể.

Ví dụ chỉ áp dụng cho:

```text
logs/
```

### Bước 4: Chọn hành động

Có thể chọn:

```text
Transition current versions of objects
Expire current versions of objects
```

Ví dụ:

```text
30 days → S3 Standard-IA
90 days → S3 Glacier Flexible Retrieval
365 days → Expire
```

Sau đó lưu rule.

Amazon S3 sẽ tự động xử lý các object đủ điều kiện.

---

## Một lưu ý quan trọng về file nhỏ

Không phải cứ chuyển dữ liệu sang storage class rẻ hơn là chắc chắn tiết kiệm.

AWS có thể tính phí cho các lifecycle transition request.

Ngoài ra, theo cấu hình mặc định hiện nay, object nhỏ hơn **128 KB** không được tự động transition bằng S3 Lifecycle.

Lý do là với rất nhiều file nhỏ, phí transition có thể lớn hơn số tiền lưu trữ tiết kiệm được.

Vì vậy nên kiểm tra:

```text
Kích thước object
+
Tần suất truy cập
+
Thời gian cần lưu
```

trước khi tạo rule.

---

## Khi nào nên dùng?

S3 Lifecycle phù hợp với:

- log cũ;
- file backup;
- ảnh/video ít truy cập sau một thời gian;
- dữ liệu lịch sử;
- dữ liệu cần lưu lâu nhưng hiếm khi đọc.

---

## Điều mình học được

Trước đây mình nghĩ tối ưu S3 chỉ đơn giản là tìm storage class rẻ nhất.

Nhưng thực tế, câu hỏi đúng hơn là:

> “Dữ liệu này sẽ được sử dụng như thế nào theo thời gian?”

Nếu dữ liệu thay đổi từ **truy cập thường xuyên → ít truy cập → lưu trữ dài hạn**, S3 Lifecycle giúp tự động hóa cả quá trình đó.

---

## Tài liệu tham khảo

- [AWS Documentation – Managing the lifecycle of objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
- [AWS Documentation – Transitioning objects using Amazon S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html)
- [AWS Storage Blog – Optimize storage costs with Amazon S3 Lifecycle](https://aws.amazon.com/blogs/storage/optimize-storage-costs-with-new-amazon-s3-lifecycle-filters-and-actions/)

---

## Kết luận

Amazon S3 Lifecycle là một tính năng đơn giản nhưng rất hữu ích để tối ưu chi phí lưu trữ.

Thay vì quản lý dữ liệu thủ công, chúng ta chỉ cần xác định:

```text
Dữ liệu bao nhiêu ngày tuổi?
→ Nên nằm ở storage class nào?
→ Khi nào có thể xóa?
```

Sau đó Amazon S3 sẽ tự động thực hiện phần còn lại.
