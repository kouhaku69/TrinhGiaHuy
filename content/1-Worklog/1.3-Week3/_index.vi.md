---
title: "Worklog Tuần 3"
date: "2026-06-08"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục Tiêu Tuần 3:

* Đào sâu mạng AWS: VPC nâng cao, Transit Gateway, CloudFront và Lambda@Edge.
* Làm quen Windows on AWS: WorkSpaces và AWS Managed Microsoft AD.
* Thực hành di trú hệ thống: VM Import/Export, Database Migration (SCT/DMS), Disaster Recovery.
* Tối ưu chi phí và giám sát nâng cao: Lambda tự động hoá, Grafana, CloudWatch, Tag & Resource Group.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: AWS Networking and Content Delivery** <br>&emsp;+ Ôn sâu các thành phần VPC: Subnet, Route Table, ENI, EIP, VPC Endpoint, Internet Gateway, NAT Gateway, Security Group vs NACL <br>&emsp;+ Triển khai Transit Gateway và Site-to-Site VPN qua Cisco CSR router (truy cập bằng Cloud9) <br>&emsp;+ Thiết lập Route 53 DNS Endpoint và Internal Hosted Zone, VPC Endpoint cho AWS Service và VPC Endpoint Service (PrivateLink) <br>&emsp;+ Thực hành VPC Peering và Transit Gateway Network Manager <br>- **Lab: CloudFront với S3 Origin** <br>&emsp;+ Tạo S3 bucket và upload file index.html mẫu <br>&emsp;+ Cấu hình CloudFront phân phối nội dung từ S3, dọn dẹp tài nguyên <br>- **Lab: AWS CloudFront nâng cao** <br>&emsp;+ Tạo CloudFront Distribution với EC2 làm Origin, kiểm tra ứng dụng <br>&emsp;+ Cấu hình Distribution Invalidations, Custom Error Page, Origin Group, Response Headers, Cache Behavior <br>&emsp;+ Viết và deploy Lambda@Edge function, xem Metrics và Logs | 08/06/2026 | 08/06/2026 | <https://000092.awsstudygroup.com/>, <https://000094.awsstudygroup.com/>, <https://000130.awsstudygroup.com/> |
| 2   | - **Lab: Windows on AWS — Amazon WorkSpaces** <br>&emsp;+ Chuẩn bị và triển khai Amazon WorkSpaces <br>&emsp;+ Truy cập WorkSpaces qua trình duyệt và qua WorkSpaces Client <br>&emsp;+ Dọn dẹp tài nguyên <br>- **Lab: Windows on AWS — AWS Managed Microsoft AD** <br>&emsp;+ Triển khai AWS Managed Directory Service <br>&emsp;+ Deploy EC2 tham gia domain, kiểm tra communication giữa các server <br>&emsp;+ Dọn dẹp tài nguyên <br>- **Lab: Building Highly Available Web Applications** <br>&emsp;+ Ôn khái niệm kiến trúc HA tiêu chuẩn: triển khai đa AZ, Load Balancer, Auto Scaling, RDS Multi-AZ <br>&emsp;+ (Ghi chú: trang lab hiện chưa tải được nội dung chi tiết, chỉ ôn khái niệm) | 09/06/2026 | 09/06/2026 | <https://000093.awsstudygroup.com/>, <https://000095.awsstudygroup.com/>, <https://000101.awsstudygroup.com/> |
| 3   | - **Lab: VM Import/Export** <br>&emsp;+ Chuẩn bị VMware Workstation, export VM từ môi trường on-premises <br>&emsp;+ Upload và import VM vào AWS, tạo AMI, deploy instance từ AMI <br>&emsp;+ Cấu hình S3 bucket ACL để export ngược instance/AMI về on-premises <br>- **Lab: Database Schema Conversion & Migration (SCT/DMS)** <br>&emsp;+ Chuẩn bị Key Pair, môi trường, kết nối nguồn Oracle/SQL Server <br>&emsp;+ Convert schema bằng AWS Schema Conversion Tool, cấu hình database đích (RDS SQL Server, Aurora MySQL/PostgreSQL) <br>&emsp;+ Tạo Replication Instance, DMS Endpoint, Migration Task; thử DMS Serverless và theo dõi scaling qua CloudWatch <br>&emsp;+ Xử lý sự cố Memory Pressure và Table Error thường gặp <br>- **Lab: AWS Elastic Disaster Recovery** <br>&emsp;+ Chuẩn bị hạ tầng mô phỏng on-premises, kết nối Bastion Host <br>&emsp;+ Cấu hình DRS, cài đặt Agent, thiết lập Launch Template <br>&emsp;+ Thực hiện Failover và dọn dẹp tài nguyên | 10/06/2026 | 10/06/2026 | <https://000014.awsstudygroup.com/>, <https://000043.awsstudygroup.com/>, <https://000100.awsstudygroup.com/> |
| 4   | - **Lab: Tối ưu chi phí EC2 bằng Lambda** <br>&emsp;+ Chuẩn bị VPC, Security Group, EC2 instance và Slack incoming webhook <br>&emsp;+ Tạo Tag cho instance, tạo IAM Role cho Lambda <br>&emsp;+ Viết Lambda function start/stop instance tự động, kiểm tra kết quả <br>- **Lab: Giám sát với Grafana** <br>&emsp;+ Chuẩn bị VPC, EC2, IAM User/Role <br>&emsp;+ Cài đặt Grafana trên EC2 <br>&emsp;+ Dựng dashboard giám sát tài nguyên AWS <br>- **Lab: CloudWatch Advanced Workshop** <br>&emsp;+ Ôn sâu hơn CloudWatch Metrics (search/math expression, dynamic label) <br>&emsp;+ Logs Insights, Metric Filter, Alarm và Dashboard nâng cao | 11/06/2026 | 11/06/2026 | <https://000022.awsstudygroup.com/>, <https://000029.awsstudygroup.com/>, <https://000036.awsstudygroup.com/> |
| 5   | - **Lab: Quản lý tài nguyên bằng Tag & Resource Group** <br>&emsp;+ Gắn Tag cho EC2 qua Console và CLI <br>&emsp;+ Lọc tài nguyên theo Tag <br>&emsp;+ Tạo Resource Group theo Tag hoặc theo CloudFormation stack <br>- **Lab: Kiểm soát truy cập EC2 bằng Resource Tag qua IAM** <br>&emsp;+ Tạo IAM Policy có điều kiện theo Tag, tạo IAM Role riêng cho EC2 Administrator <br>&emsp;+ Switch Role và kiểm tra quyền tạo/sửa EC2 instance ở nhiều Region (Tokyo, North Virginia) theo đúng/thiếu Tag yêu cầu | 12/06/2026 | 12/06/2026 | <https://000027.awsstudygroup.com/>, <https://000028.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 3**

**1. Mạng nâng cao & Content Delivery**

* Ôn sâu các thành phần VPC (ENI, EIP, VPC Endpoint, NAT/IGW) và triển khai Transit Gateway kết hợp Site-to-Site VPN
* Xây dựng CloudFront Distribution với cả S3 và EC2 Origin, viết Lambda@Edge function xử lý request

**2. Windows on AWS**
* Triển khai Amazon WorkSpaces và truy cập qua nhiều phương thức
* Triển khai AWS Managed Microsoft AD và kiểm tra kết nối giữa các server tham gia domain

**3. Di trú hệ thống (Migration)**

* Thực hành VM Import/Export để đưa máy ảo on-premises lên AWS và ngược lại
* Chuyển đổi schema và di trú dữ liệu bằng AWS SCT/DMS, bao gồm cả DMS Serverless
* Mô phỏng Disaster Recovery với AWS Elastic Disaster Recovery, thực hiện Failover

**4. Tối ưu chi phí & Giám sát**

* Tự động start/stop EC2 bằng Lambda để tiết kiệm chi phí
* Giám sát tài nguyên bằng Grafana và CloudWatch nâng cao (Logs Insights, Metric Filter)

**5. Quản lý tài nguyên & kiểm soát truy cập**

* Gắn Tag và tạo Resource Group để quản lý tài nguyên có hệ thống
* Áp dụng IAM Policy điều kiện theo Tag để giới hạn quyền EC2 Administrator theo Region

### Kết luận Tuần 3

Tuần 3 chuyển trọng tâm sang các bài toán vận hành ở quy mô lớn hơn: mạng nâng cao với Transit Gateway/CloudFront, hệ sinh thái Windows on AWS, và đặc biệt là mảng di trú hệ thống (VM, database, disaster recovery) — đây là nhóm kỹ năng quan trọng khi làm việc với khách hàng đang chuyển đổi hạ tầng lên cloud. Phần Database Migration với SCT/DMS là nội dung nặng nhất, cần thực hành lại nhiều lần để nhớ rõ luồng schema conversion và troubleshoot lỗi migration. Việc dùng Tag kết hợp IAM Policy để giới hạn quyền theo Region cũng là một kỹ thuật quản trị đáng áp dụng cho các dự án nhiều môi trường sau này.
