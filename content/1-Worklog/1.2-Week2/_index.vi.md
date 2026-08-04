---
title: "Worklog Tuần 2"
date: "2026-06-01"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục Tiêu Tuần 2:

* Nắm vững AWS IAM: User, Group, Role, Policy và cách cấp quyền an toàn cho ứng dụng.
* Làm quen các dịch vụ compute: EC2, Lightsail, Lightsail Container, EC2 Auto Scaling.
* Sử dụng công cụ vận hành (Cloud9, AWS CLI) và bắt đầu với tầng dữ liệu: RDS, DynamoDB, ElastiCache.
* Giám sát hệ thống với CloudWatch và mở rộng mạng lai với Route 53 Resolver.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: AWS Identity and Access Management (IAM)** <br>&emsp;+ Phân biệt IAM Group, IAM User, IAM Policy, IAM Role <br>&emsp;+ Tạo Admin Group và Admin User, đăng nhập thử <br>&emsp;+ Tạo Admin Role và Operator User, cấu hình switch role <br>&emsp;+ Dọn dẹp tài nguyên <br>- **Lab: Làm quen Amazon EC2** <br>&emsp;+ Chuẩn bị VPC và Security Group riêng cho Linux, Windows <br>&emsp;+ Khởi chạy Windows Server 2025 và Amazon Linux instance, kết nối tới từng loại <br>&emsp;+ Đổi loại instance, tạo EBS Snapshot, tạo Custom AMI và khởi chạy từ AMI đó <br>&emsp;+ Tìm hiểu cách khôi phục quyền truy cập khi mất key pair <br>- **Lab: Cấp quyền ứng dụng bằng IAM Role trên EC2** <br>&emsp;+ Chuẩn bị EC2 instance và S3 bucket <br>&emsp;+ Thử dùng Access Key để truy cập S3, sau đó chuyển sang IAM Role gắn trên EC2 <br>&emsp;+ So sánh mức độ an toàn giữa hai cách, dọn dẹp tài nguyên | 01/06/2026 | 01/06/2026 | <https://000002.awsstudygroup.com/>, <https://000004.awsstudygroup.com/>, <https://000048.awsstudygroup.com/> |
| 2   | - **Lab: AWS Cloud9** <br>&emsp;+ Tạo Cloud9 instance <br>&emsp;+ Thao tác cơ bản: dùng command line, chỉnh sửa file text, quay lại Dashboard <br>&emsp;+ Dùng AWS CLI ngay trong Cloud9, dọn dẹp tài nguyên <br>- **Lab: Amazon S3 Static Website Hosting** <br>&emsp;+ Tìm hiểu khái niệm bucket, object, region <br>&emsp;+ Tạo bucket, tải source code mẫu lên, bật static website hosting <br>&emsp;+ Cấu hình public access block và quyền public cho object, kiểm tra website <br>- **Lab: Amazon RDS** <br>&emsp;+ Chuẩn bị VPC, Security Group cho EC2 và RDS, DB Subnet Group <br>&emsp;+ Tạo EC2 instance và RDS database instance, triển khai ứng dụng kết nối tới RDS <br>&emsp;+ Thực hành Backup và Restore, dọn dẹp tài nguyên | 02/06/2026 | 02/06/2026 | <https://000049.awsstudygroup.com/>, <https://000057.awsstudygroup.com/>, <https://000005.awsstudygroup.com/> |
| 3   | - **Lab: Amazon Lightsail — Cost Optimization** <br>&emsp;+ Triển khai database và 3 ứng dụng mã nguồn mở: WordPress, PrestaShop, Akaunting <br>&emsp;+ Cấu hình mạng và bảo mật cho từng ứng dụng <br>&emsp;+ Tạo Snapshot, nâng cấp instance lớn hơn, thiết lập Alarm giám sát <br>- **Lab: Amazon Lightsail Container** <br>&emsp;+ Tạo Container Service, thử deploy một public image <br>&emsp;+ Build và push image riêng bằng Docker rồi deploy, dọn dẹp tài nguyên <br>- **Lab: EC2 Auto Scaling & Load Balancer** <br>&emsp;+ Chuẩn bị hạ tầng mạng, EC2, RDS và web server nền <br>&emsp;+ Tạo Launch Template và Application Load Balancer <br>&emsp;+ Tạo Auto Scaling Group, kiểm thử scaling thủ công, theo lịch và dynamic scaling | 03/06/2026 | 03/06/2026 | <https://000045.awsstudygroup.com/>, <https://000046.awsstudygroup.com/>, <https://000006.awsstudygroup.com/> |
| 4   | - **Lab: AWS CloudWatch** <br>&emsp;+ Xem và phân tích CloudWatch Metrics (search expression, math expression, dynamic label) <br>&emsp;+ Làm việc với CloudWatch Logs, Logs Insights và Metric Filter <br>&emsp;+ Tạo CloudWatch Alarm và Dashboard giám sát <br>- **Lab: Hybrid DNS với Route 53 Resolver** <br>&emsp;+ Chuẩn bị Key Pair, CloudFormation Template, Security Group <br>&emsp;+ Kết nối RDGW, triển khai Microsoft AD <br>&emsp;+ Tạo Route 53 Outbound/Inbound Endpoint và Resolver Rules, kiểm tra kết quả <br>- **Lab: Làm quen AWS CLI** <br>&emsp;+ Cài đặt và cấu hình AWS CLI <br>&emsp;+ Thao tác với S3, SNS, IAM, VPC bằng CLI <br>&emsp;+ Tạo EC2 instance bằng CLI, khắc phục lỗi thường gặp | 04/06/2026 | 04/06/2026 | <https://000008.awsstudygroup.com/>, <https://000010.awsstudygroup.com/>, <https://000011.awsstudygroup.com/> |
| 5   | - **Lab: Amazon DynamoDB** <br>&emsp;+ Tìm hiểu Core Component, Primary Key, Secondary Index, Read Consistency, Capacity Mode <br>&emsp;+ Thực hành tạo bảng, ghi/đọc/cập nhật/truy vấn dữ liệu qua Console và CloudShell <br>&emsp;+ Thực hành với AWS SDK (Python): CRUD, load sample data, query/scan <br>- **Lab: Amazon ElastiCache (Redis)** <br>&emsp;+ Tạo Subnet Group và cluster Redis (cluster mode disabled/enabled) <br>&emsp;+ Kết nối tới cluster node, cấp quyền truy cập <br>&emsp;+ Dùng AWS SDK để set/get string, hash, publish/subscribe và đọc/ghi stream | 05/06/2026 | 05/06/2026 | <https://000060.awsstudygroup.com/>, <https://000061.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 2**

**1. Quản lý truy cập & bảo mật (IAM)**

* Phân biệt rõ IAM Group, User, Role, Policy; thực hành switch role
* So sánh Access Key và IAM Role khi cấp quyền cho ứng dụng chạy trên EC2

**2. Compute & Scaling**

* Khởi chạy và quản lý EC2 (Windows Server 2025, Amazon Linux), tạo Custom AMI
* Triển khai ứng dụng mã nguồn mở trên Lightsail và Lightsail Container
* Xây dựng Auto Scaling Group kết hợp Load Balancer cho ứng dụng có khả năng mở rộng

**3. Công cụ phát triển & vận hành**

* Làm quen AWS Cloud9 làm IDE trên trình duyệt
* Thao tác thành thạo AWS CLI với S3, SNS, IAM, VPC, EC2

**4. Lưu trữ & cơ sở dữ liệu**

* Host static website trên Amazon S3
* Triển khai Amazon RDS, thực hành backup/restore
* Làm việc với Amazon DynamoDB và Amazon ElastiCache (Redis) qua Console, CLI và SDK

**5. Giám sát & mạng**

* Sử dụng CloudWatch Metrics, Logs, Alarm và Dashboard để giám sát hệ thống
* Triển khai Hybrid DNS với Route 53 Resolver kết nối AWS với Microsoft AD on-premises

### Kết luận Tuần 2

Tuần 2 mở rộng nhanh sang nhiều dịch vụ nền tảng khác nhau — từ bảo mật (IAM) đến compute (EC2, Lightsail, Auto Scaling), công cụ vận hành (Cloud9, CLI) và tầng dữ liệu (RDS, DynamoDB, ElastiCache), kèm giám sát bằng CloudWatch và mạng lai với Route 53. Khối lượng kiến thức khá lớn cho một tuần, nhưng nhờ thực hành trực tiếp trên từng dịch vụ nên dễ hình dung hơn là chỉ đọc lý thuyết. Auto Scaling và Hybrid DNS là hai phần có nhiều bước cấu hình nhất, cần xem lại kỹ trước khi áp dụng vào dự án thực tế.
