---
title: "Worklog Tuần 4"
date: "2026-06-15"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục Tiêu Tuần 4:

* Vận hành hệ thống từ xa qua Systems Manager và Session Manager.
* Triển khai hạ tầng bằng code với CloudFormation và AWS CDK.
* Tối ưu chi phí và tài nguyên: EC2 Resource Optimization, Service Quotas, IAM giới hạn usage.
* Tự động hoá vòng đời snapshot và phát hiện bất thường trong backup.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: AWS Systems Manager** <br>&emsp;+ Chuẩn bị VPC, EC2 Windows và IAM Role cho SSM Agent <br>&emsp;+ Dùng Patch Manager để vá lỗi tự động <br>&emsp;+ Dùng Run Command để chạy lệnh trên nhiều server cùng lúc <br>- **Lab: Systems Manager — Session Manager** <br>&emsp;+ Chuẩn bị VPC với subnet public/private, EC2 public và private instance <br>&emsp;+ Kết nối instance private qua VPC Endpoint (ssm, ssmmessages, ec2messages) không cần bastion host <br>&emsp;+ Quản lý session log ghi vào S3 và thực hành Port Forwarding <br>- **Lab: AWS CloudFormation** <br>&emsp;+ Viết CloudFormation Template cơ bản trong Cloud9 <br>&emsp;+ Custom Resource với Lambda, Mappings và StackSets, Drift Detection | 15/06/2026 | 15/06/2026 | <https://000031.awsstudygroup.com/>, <https://000058.awsstudygroup.com/>, <https://000037.awsstudygroup.com/> |
| 2   | - **Lab: AWS CDK Essentials** <br>&emsp;+ Tìm hiểu CDK và mối quan hệ với CloudFormation <br>&emsp;+ Tạo workspace, cấu hình môi trường Cloud9 <br>&emsp;+ Viết và cập nhật CDK Template đầu tiên, deploy EC2 qua user data <br>- **Lab: AWS CDK Advanced** <br>&emsp;+ Dùng CDK dựng kiến trúc gồm API Gateway, ALB, ECS và Lambda <br>&emsp;+ Kết hợp Lambda với S3 <br>&emsp;+ Tạo Nested Stack bằng CDK <br>- **Lab: Infrastructure as Code Workshop Series** <br>&emsp;+ Ôn khái niệm IaC và so sánh các framework phổ biến <br>&emsp;+ Tạo Lambda Function, VPC và EC2 bằng code <br>&emsp;+ Triển khai kiến trúc Three-Tier (Web/Application/Database) bằng CloudFormation Stack và kiểm tra quyền truy cập từng tầng | 16/06/2026 | 16/06/2026 | <https://000038.awsstudygroup.com/>, <https://000076.awsstudygroup.com/>, <https://000102.awsstudygroup.com/> |
| 3   | - **Lab: Right-Sizing cho Amazon EC2** <br>&emsp;+ Làm quen Amazon CloudWatch, tạo IAM Role cho CloudWatch Agent <br>&emsp;+ Cài đặt CloudWatch Agent để thu thập chỉ số bộ nhớ <br>&emsp;+ Xem khuyến nghị từ EC2 Resource Optimization và AWS Compute Optimizer <br>- **Lab: Giám sát hạ tầng mạng với VPC Flow Logs** <br>&emsp;+ Tạo và bật VPC Flow Logs, đẩy dữ liệu vào CloudWatch Logs <br>&emsp;+ Phân tích traffic để xác định Security Group đang quá chặt hay quá lỏng <br>- **Lab: Ủy quyền truy cập Billing Console** <br>&emsp;+ Tạo IAM User Group, bật quyền truy cập Billing <br>&emsp;+ Tạo và gán IAM Policy cho phép xem/quản lý chi phí, kiểm tra quyền truy cập | 17/06/2026 | 17/06/2026 | <https://000032.awsstudygroup.com/>, <https://000074.awsstudygroup.com/>, <https://000075.awsstudygroup.com/> |
| 4   | - **Lab: Quản lý Service Quotas** <br>&emsp;+ Tìm hiểu Service Quotas — giới hạn mặc định của từng dịch vụ AWS <br>&emsp;+ Thực hành gửi yêu cầu tăng quota <br>- **Lab: Quản lý chi phí & usage bằng IAM** <br>&emsp;+ Tạo IAM Group/User, viết Policy giới hạn theo Region <br>&emsp;+ Giới hạn theo họ EC2 instance, theo kích thước instance, theo loại EBS volume <br>&emsp;+ Kiểm tra hiệu lực từng policy giới hạn <br>- **Lab: Tự động lưu trữ EBS Snapshot bằng Data Lifecycle Manager** <br>&emsp;+ Tạo EC2 instance có snapshot mẫu <br>&emsp;+ Thiết lập chính sách lưu trữ đơn (Single Policy Schedule) và đa chính sách (Multiple Policy Schedules) <br>&emsp;+ Kiểm tra kết quả tự động archive/xoá snapshot | 18/06/2026 | 18/06/2026 | <https://000063.awsstudygroup.com/>, <https://000064.awsstudygroup.com/>, <https://000088.awsstudygroup.com/> |
| 5   | - **Lab: Phát hiện bất thường trong AWS Backup cho EBS** <br>&emsp;+ Chuẩn bị S3 bucket, EBS volume và hạ tầng bằng CloudFormation <br>&emsp;+ Tạo backup và tìm hiểu pipeline phát hiện bất thường (AWS Backup → EventBridge → Lambda → DynamoDB → CloudWatch → SNS) <br>&emsp;+ Theo dõi cảnh báo khi số block thay đổi giữa các snapshot vượt ngưỡng <br>- **Lab: AWS Toolkit for VS Code — Amazon Q & CodeWhisperer** <br>&emsp;+ Cài đặt AWS Toolkit cho VS Code, kết nối tài khoản AWS <br>&emsp;+ Dùng AWS Explorer để thao tác trực tiếp với dịch vụ AWS trong IDE <br>&emsp;+ Dùng Amazon Q để hỏi đáp và debug code, dùng Amazon CodeWhisperer để gợi ý code và quét lỗ hổng bảo mật | 19/06/2026 | 19/06/2026 | <https://000089.awsstudygroup.com/>, <https://000087.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 4**

**1. Vận hành từ xa**

* Dùng Systems Manager Patch Manager và Run Command để vá lỗi và chạy lệnh hàng loạt
* Kết nối EC2 private instance qua Session Manager và VPC Endpoint, không cần mở port SSH/RDP ra ngoài

**2. Infrastructure as Code**

* Viết CloudFormation Template từ cơ bản đến nâng cao (Custom Resource, StackSets, Drift Detection)
* Dùng AWS CDK để định nghĩa hạ tầng bằng code, dựng kiến trúc ECS/ALB/API Gateway/Lambda và Nested Stack
* Triển khai kiến trúc Three-Tier bằng CloudFormation

**3. Tối ưu chi phí & tài nguyên**

* Right-size EC2 dựa trên chỉ số CloudWatch Agent và khuyến nghị Compute Optimizer
* Giám sát traffic mạng bằng VPC Flow Logs, ủy quyền Billing Console qua IAM
* Giới hạn usage bằng IAM Policy theo Region, họ EC2, kích thước instance và loại EBS volume
* Quản lý Service Quotas và gửi yêu cầu tăng hạn mức

**4. Tự động hoá & bảo vệ dữ liệu**

* Tự động hoá vòng đời EBS Snapshot bằng Data Lifecycle Manager
* Tìm hiểu pipeline phát hiện bất thường trong AWS Backup (EventBridge, Lambda, DynamoDB, CloudWatch, SNS)

**5. Công cụ hỗ trợ lập trình**

* Cài đặt AWS Toolkit for VS Code, dùng Amazon Q và CodeWhisperer khi viết code

### Kết luận Tuần 4

Tuần 4 gồm hai mảng chính: vận hành hệ thống từ xa (Systems Manager, Session Manager) và Infrastructure as Code (CloudFormation, CDK). CDK sinh CloudFormation ở phía sau, nên học CloudFormation trước giúp đọc hiểu CDK nhanh hơn. Các lab về Service Quotas, IAM giới hạn usage và Data Lifecycle Manager là các thao tác quản trị chi phí sẽ lặp lại khi vận hành tài khoản AWS trong thời gian dài.
