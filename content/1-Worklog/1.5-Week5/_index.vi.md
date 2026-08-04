---
title: "Worklog Tuần 5"
date: "2026-06-22"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục Tiêu Tuần 5:

* Quản lý identity tập trung với IAM Identity Center (SSO), giới hạn quyền bằng Permission Boundary và IAM Condition.
* Giám sát tuân thủ bảo mật với Security Hub, kiểm soát truy cập S3 qua VPC Endpoint, chặn tấn công web bằng WAF.
* Mã hoá dữ liệu với KMS, phát hiện dữ liệu nhạy cảm với Macie, quản lý secret với Secrets Manager.
* Quản lý Security Group tập trung với Firewall Manager, phát hiện mối đe doạ với GuardDuty, tự động vá lỗi với EC2 Image Builder.
* Xác thực người dùng đa nền tảng với Cognito, áp dụng các best practice bảo mật cho S3.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: IAM Identity Center (AWS SSO)** <br>&emsp;+ Tạo tài khoản AWS trong AWS Organizations, cấu hình Organization Unit <br>&emsp;+ Mời tài khoản thành viên vào Organization, cấp quyền theo Permission Set <br>&emsp;+ Truy cập qua AWS CLI, giới hạn truy cập theo thời gian, dùng Customer Managed Policy <br>- **Lab: IAM Permission Boundary** <br>&emsp;+ Tạo Restriction Policy đóng vai trò Permission Boundary <br>&emsp;+ Tạo IAM User bị giới hạn quyền, kiểm tra quyền thực tế của user <br>- **Lab: IAM Role & Condition** <br>&emsp;+ Tạo IAM Group/User riêng cho EC2 và RDS <br>&emsp;+ Tạo Admin Role, cấu hình switch role <br>&emsp;+ Giới hạn switch role theo địa chỉ IP và theo khung giờ | 22/06/2026 | 22/06/2026 | <https://000012.awsstudygroup.com/>, <https://000030.awsstudygroup.com/>, <https://000044.awsstudygroup.com/> |
| 2   | - **Lab: AWS Security Hub** <br>&emsp;+ Xem các Security Standards áp dụng cho tài khoản <br>&emsp;+ Bật Security Hub, xem điểm bảo mật theo từng standard <br>- **Lab: Truy cập S3 an toàn qua VPC Endpoint** <br>&emsp;+ Tạo Gateway Endpoint để truy cập S3 từ VPC, kiểm tra kết nối <br>&emsp;+ Tạo Interface Endpoint để truy cập S3 từ on-premises, mô phỏng DNS on-premises <br>&emsp;+ Cấu hình VPC Endpoint Policy giới hạn truy cập <br>- **Lab: AWS WAF** <br>&emsp;+ Chuẩn bị S3 bucket và deploy sample web app <br>&emsp;+ Tạo Web ACL với managed rule, tạo custom rule và advanced custom rule <br>&emsp;+ Kiểm tra rule mới và bật logging request | 23/06/2026 | 23/06/2026 | <https://000018.awsstudygroup.com/>, <https://000111.awsstudygroup.com/>, <https://000026.awsstudygroup.com/> |
| 3   | - **Lab: Mã hoá dữ liệu với AWS KMS** <br>&emsp;+ Tạo Policy/Role, Group/User, tạo KMS key <br>&emsp;+ Tạo S3 bucket, upload dữ liệu được mã hoá bằng KMS key <br>&emsp;+ Tạo CloudTrail ghi log, dùng Athena truy vấn log, thử chia sẻ dữ liệu đã mã hoá <br>- **Lab: Phát hiện dữ liệu nhạy cảm với Amazon Macie** <br>&emsp;+ Tạo S3 bucket, bật Macie <br>&emsp;+ Tạo Custom Data Identifier, tạo Macie Job quét dữ liệu <br>&emsp;+ Xem kết quả và các finding về dữ liệu nhạy cảm/bucket cấu hình sai <br>- **Lab: AWS Secrets Manager với RDS và Fargate** <br>&emsp;+ Chuẩn bị hạ tầng (VPC, Bastion Host, RDS private) <br>&emsp;+ Truy cập RDS bằng credential lưu trong Secrets Manager, thực hành Secret Rotation <br>&emsp;+ Truy cập RDS từ ứng dụng chạy trên Fargate | 24/06/2026 | 24/06/2026 | <https://000033.awsstudygroup.com/>, <https://000090.awsstudygroup.com/>, <https://000096.awsstudygroup.com/> |
| 4   | - **Lab: Quản lý Security Group với AWS Firewall Manager** <br>&emsp;+ Ôn khái niệm Security Group trong kiểm soát truy cập từ xa (RDP, SSH) <br>&emsp;+ Cấu hình AWS Firewall Manager để audit Security Group tập trung nhiều tài khoản <br>&emsp;+ Áp dụng policy giới hạn Security Group <br>- **Lab: Amazon GuardDuty** <br>&emsp;+ Tìm hiểu cơ chế hoạt động của GuardDuty <br>&emsp;+ Mô phỏng 3 tình huống: EC2 bị xâm nhập, IAM credential bị lộ, IAM Role bị đánh cắp credential <br>&emsp;+ Xử lý tự động bằng EventBridge + Lambda cho một số tình huống <br>- **Lab: Tự động vá lỗi với EC2 Image Builder & Systems Manager** <br>&emsp;+ Dựng hạ tầng cơ bản và hạ tầng ứng dụng <br>&emsp;+ Tạo AMI Builder Pipeline bằng EC2 Image Builder <br>&emsp;+ Tự động hoá quy trình build bằng SSM Automation Document, deploy AMI mới qua CloudFormation AutoScalingReplacingUpdate | 25/06/2026 | 25/06/2026 | <https://000097.awsstudygroup.com/>, <https://000098.awsstudygroup.com/>, <https://000099.awsstudygroup.com/> |
| 5   | - **Lab: Amazon Cognito Cross-Site** <br>&emsp;+ Phân biệt User Pool và Identity Pool trong Cognito <br>&emsp;+ Chuẩn bị hạ tầng, đọc hiểu code mẫu <br>&emsp;+ Deploy và test xác thực Cognito giữa nhiều site/ứng dụng <br>- **Lab: S3 Security Best Practices** <br>&emsp;+ Chuẩn bị hạ tầng bằng CloudFormation, tạo access key <br>&emsp;+ Bắt buộc HTTPS và mã hoá SSE-S3, chặn Public ACL và cấu hình S3 Block Public Access <br>&emsp;+ Giới hạn truy cập qua S3 VPC Endpoint, dùng AWS Config phát hiện bucket public và Access Analyzer cho S3 | 26/06/2026 | 26/06/2026 | <https://000141.awsstudygroup.com/>, <https://000069.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 5**

**1. Quản lý identity & phân quyền**

* Cấu hình IAM Identity Center để quản lý truy cập tập trung nhiều tài khoản AWS
* Áp dụng Permission Boundary để giới hạn quyền tối đa của user
* Cấu hình IAM Role với Condition giới hạn theo IP và theo thời gian

**2. Giám sát tuân thủ & bảo vệ ứng dụng web**

* Bật Security Hub để xem điểm tuân thủ theo các security standard
* Cấu hình VPC Endpoint (Gateway, Interface) để truy cập S3 không qua internet công cộng
* Cấu hình AWS WAF với managed rule và custom rule để chặn tấn công web

**3. Bảo vệ dữ liệu**

* Mã hoá dữ liệu S3 bằng KMS, truy vấn log truy cập bằng CloudTrail và Athena
* Dùng Macie để phát hiện dữ liệu nhạy cảm (PII, dữ liệu tài chính) trong S3
* Quản lý và xoay vòng secret tự động bằng Secrets Manager cho RDS và Fargate

**4. Phát hiện và phản ứng với mối đe doạ**

* Audit và giới hạn Security Group tập trung bằng Firewall Manager
* Mô phỏng và xử lý các tình huống bị xâm nhập với GuardDuty, EventBridge và Lambda
* Tự động hoá quy trình vá lỗi hệ điều hành bằng EC2 Image Builder và Systems Manager

**5. Xác thực & bảo mật S3**

* Triển khai Cognito User Pool/Identity Pool cho xác thực cross-site
* Áp dụng các best practice bảo mật S3: bắt buộc HTTPS, mã hoá SSE-S3, chặn public access, dùng Access Analyzer

### Kết luận Tuần 5

Tuần 5 tập trung vào mảng Security trong danh mục Optimize, đi từ quản lý identity (SSO, Permission Boundary, Role Condition) đến bảo vệ dữ liệu (KMS, Macie, Secrets Manager) và phát hiện mối đe doạ (GuardDuty, Firewall Manager). Các lab đều dùng chung một số khái niệm nền đã học trước đó (IAM Policy, VPC, CloudFormation), nên phần khó nhất là ghép các dịch vụ lại thành một luồng xử lý sự cố hoàn chỉnh, ví dụ GuardDuty kết hợp EventBridge và Lambda để tự động phản ứng. Nội dung tuần này liên quan trực tiếp đến các hạng mục compliance mà nhiều tổ chức yêu cầu khi vận hành hệ thống trên AWS.
