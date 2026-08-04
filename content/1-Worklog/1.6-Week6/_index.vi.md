---
title: "Worklog Tuần 6"
date: "2026-06-29"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục Tiêu Tuần 6:

* Xây dựng khả năng chịu lỗi (reliability) cho hạ tầng: AWS Backup, VPC Peering, Transit Gateway.
* Xây dựng kiến trúc hướng sự kiện với SNS/SQS, chia sẻ dữ liệu qua EBS Multi-Attach.
* Triển khai high availability cho database trên Windows: Windows Failover Cluster, SQL Server HA (2019, 2022).
* Đóng gói và triển khai ứng dụng bằng container: Docker, ECS, CDK; thiết lập CI/CD cho container và EC2.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Triển khai AWS Backup** <br>&emsp;+ Tạo S3 bucket, triển khai hạ tầng cho backup <br>&emsp;+ Tạo Backup Plan cho EBS, RDS, DynamoDB, EFS <br>&emsp;+ Cấu hình thông báo qua SNS, kiểm tra Restore, dọn dẹp tài nguyên <br>- **Lab: VPC Peering** <br>&emsp;+ Ôn lại cách kết nối trực tiếp hai VPC không qua internet <br>&emsp;+ Cấu hình Route Table và Network ACL cho kết nối peer <br>- **Lab: AWS Transit Gateway** <br>&emsp;+ So sánh VPC Peering và Transit Gateway khi số lượng VPC tăng lên <br>&emsp;+ Kết nối 4 VPC qua một Transit Gateway <br>&emsp;+ Tạo Transit Gateway Attachment và Route Table, thêm route vào VPC Route Table | 29/06/2026 | 29/06/2026 | <https://000013.awsstudygroup.com/>, <https://000019.awsstudygroup.com/>, <https://000020.awsstudygroup.com/> |
| 2   | - **Lab: Kiến trúc hướng sự kiện với SNS và SQS** <br>&emsp;+ Triển khai hạ tầng và bộ sinh sự kiện mẫu <br>&emsp;+ Thực hành mô hình pub/sub cơ bản <br>&emsp;+ Cấu hình message filtering và advanced message filtering để định tuyến message theo attribute <br>- **Lab: Chia sẻ dữ liệu qua nhiều VPC bằng EBS Multi-Attach (NVMe Reservation)** <br>&emsp;+ Tạo 2 VPC (Prod, Test) và EC2 instance cho mỗi VPC <br>&emsp;+ Tạo một EBS volume và gắn cho cả hai instance <br>&emsp;+ Cài PostgreSQL, dùng NVMe Reservation để kiểm soát quyền truy cập volume dùng chung <br>- **Lab: MySQL HA Cluster bằng EBS Multi-Attach** <br>&emsp;+ Tạo VPC, EC2 instance, EBS volume dùng chung, cấu hình LVM <br>&emsp;+ Cài MySQL, cấu hình Network Load Balancer, deploy WordPress kết nối qua NLB <br>&emsp;+ Tạo Response Plan trong Incident Manager, CloudWatch Alarm, kiểm tra Failover | 30/06/2026 | 30/06/2026 | <https://000077.awsstudygroup.com/>, <https://100000.awsstudygroup.com/>, <https://100001.awsstudygroup.com/> |
| 3   | - **Lab: Windows Failover Cluster bằng EBS Multi-Attach** <br>&emsp;+ Tạo VPC, Active Directory, các EC2 instance node cluster <br>&emsp;+ Tạo EBS volume dùng chung, join domain, cài Windows Feature cho cluster <br>&emsp;+ Tạo Failover Cluster, cấu hình network và storage cho cluster <br>- **Lab: SQL Server 2019 trên Windows Failover Cluster** <br>&emsp;+ Cài SQL Server 2019 trên node 1, cài SSMS <br>&emsp;+ Thêm node 2 vào cluster, cài SSMS trên node 2 <br>&emsp;+ Kiểm tra kết quả cài đặt cluster <br>- **Lab: MSSQL Cluster với Windows Failover Cluster (2022)** <br>&emsp;+ Lặp lại quy trình dựng WSFC với SQL Server 2022 <br>&emsp;+ Cài SQL Server, thêm node 2, kiểm tra kết quả | 01/07/2026 | 01/07/2026 | <https://100002.awsstudygroup.com/>, <https://100003.awsstudygroup.com/>, <https://100004.awsstudygroup.com/> |
| 4   | - **Lab: Deploy ứng dụng bằng Docker** <br>&emsp;+ Deploy ứng dụng ở local trước, sau đó chuẩn bị VPC/SG/IAM Role cho ECR <br>&emsp;+ Tạo RDS instance, cấu hình EC2 để chạy container <br>&emsp;+ Deploy bằng Docker image và bằng Docker Compose, push image lên ECR/Docker Hub <br>- **Lab: Deploy ứng dụng trên Amazon ECS** <br>&emsp;+ Chuẩn bị hạ tầng, tạo ECS Cluster và Task Definition <br>&emsp;+ Cấu hình Application Load Balancer, tạo ECS Service <br>&emsp;+ Kiểm tra kết quả deploy <br>- **Lab: Deploy Spring Boot lên ECS Fargate bằng AWS CDK** <br>&emsp;+ Tạo ECR repository, VPC và NAT Gateway bằng CDK <br>&emsp;+ Tạo ECS Cluster, Service, API Gateway và DynamoDB table bằng CDK <br>&emsp;+ Gắn AWS X-Ray để theo dõi request | 02/07/2026 | 02/07/2026 | <https://000015.awsstudygroup.com/>, <https://000016.awsstudygroup.com/>, <https://000118.awsstudygroup.com/> |
| 5   | - **Lab: CI/CD cho ứng dụng container trên ECS** <br>&emsp;+ Thiết lập CI/CD với GitLab Runner, với GitHub Actions, và với CodeBuild <br>&emsp;+ Giám sát ứng dụng bằng Container Insights (CloudWatch) <br>&emsp;+ Định tuyến log bằng Firelens, lưu log vào S3 <br>- **Lab: Deploy ứng dụng lên EC2 bằng AWS CodePipeline** <br>&emsp;+ Chuẩn bị hạ tầng, S3 bucket, Git connection, IAM Role/Instance Profile <br>&emsp;+ Cấu hình CodeDeploy Agent, CodeCommit, CodeBuild, CodeDeploy <br>&emsp;+ Ghép thành một CodePipeline hoàn chỉnh, khắc phục lỗi thường gặp | 03/07/2026 | 03/07/2026 | <https://000017.awsstudygroup.com/>, <https://000023.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 6**

**1. Khả năng chịu lỗi & kết nối mạng**

* Tự động hoá backup/restore cho EBS, RDS, DynamoDB, EFS bằng AWS Backup
* So sánh và triển khai VPC Peering, Transit Gateway cho các mô hình kết nối nhiều VPC

**2. Kiến trúc hướng sự kiện & chia sẻ storage**

* Xây dựng luồng pub/sub và message filtering với SNS/SQS
* Dùng EBS Multi-Attach để chia sẻ volume giữa nhiều EC2 instance, áp dụng NVMe Reservation kiểm soát truy cập
* Dựng MySQL HA cluster với NLB, Incident Manager và CloudWatch Alarm

**3. High Availability cho SQL Server trên Windows**

* Dựng Windows Failover Cluster bằng EBS Multi-Attach
* Cài đặt SQL Server 2019 và 2022 trên WSFC, kiểm tra cluster hoạt động

**4. Container hoá & CI/CD**

* Đóng gói ứng dụng bằng Docker, đẩy image lên ECR/Docker Hub
* Triển khai ứng dụng trên ECS bằng Console và bằng AWS CDK (kèm API Gateway, DynamoDB, X-Ray)
* Thiết lập CI/CD cho container (GitLab, GitHub Actions, CodeBuild) và cho EC2 (CodePipeline, CodeCommit, CodeDeploy)
* Giám sát ứng dụng container bằng Container Insights và định tuyến log bằng Firelens

### Kết luận Tuần 6

Tuần 6 gồm hai phần rõ rệt: mảng Reliability (Backup, VPC Peering, Transit Gateway, SNS/SQS, EBS Multi-Attach, Windows Failover Cluster) và mảng Performance mở đầu bằng container (Docker, ECS, CI/CD). Nhóm lab về Windows Failover Cluster với SQL Server là phần tốn thời gian nhất vì có nhiều bước chuẩn bị Active Directory và cấu hình cluster storage. Phần container và CI/CD giới thiệu một hướng triển khai khác với các lab EC2/VPC truyền thống trước đó, và CDK tiếp tục xuất hiện như công cụ chính để định nghĩa hạ tầng ECS bằng code.
