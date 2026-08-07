---
title: "Cấu hình AWS và triển khai"
date: "2026-08-07"
weight: 4
chapter: false
pre: "<b> 5.4. </b>"
---

# Cấu hình AWS và triển khai ứng dụng

Phần này chuyển kiến trúc thành các thao tác thực hành cụ thể trên AWS Console. Mỗi bài gồm mục tiêu, thông số cần nhập, cấu hình liên quan đến mã nguồn, bước kiểm tra và minh chứng cần chụp.

#### Thứ tự thực hiện

1. [Tạo VPC, subnet, route table và security group](5.4.1-Networking/)
2. [Tạo IAM role, EC2 và Elastic IP](5.4.2-IAM-EC2/)
3. [Tạo Amazon RDS for PostgreSQL](5.4.3-RDS/)
4. [Tạo bí mật trong AWS Secrets Manager](5.4.4-SecretsManager/)
5. [Tạo và cấu hình Amazon S3](5.4.5-S3/)
6. [Tạo Amazon Cognito User Pool và App Client](5.4.6-Cognito/)
7. [Cấp quyền sử dụng Amazon Bedrock](5.4.7-Bedrock/)
8. [Triển khai frontend và backend bằng Docker](5.4.8-Docker/)
9. [Tạo Amazon CloudFront distribution](5.4.9-CloudFront/)
10. [Cấu hình Amazon CloudWatch](5.4.10-CloudWatch/)
11. [Kiểm thử, nghiệm thu và clean-up](5.4.11-Test-Cleanup/)

#### Thông số đã xác minh

| Hạng mục | Giá trị |
|---|---|
| Region | `ap-southeast-1` (Singapore) |
| EC2 | `i-03642ee2788132cb3`, `t3.medium`, `ap-southeast-1a` |
| Elastic IP | `54.251.119.230` |
| CloudFront | `d3pn12mzrv3aqy.cloudfront.net` |
| Frontend container | `balan-frontend`, `80:80` |
| Backend container | `balan-backend`, `5000:5000`, health check `/health` |
| CloudWatch log group | `/balancoffee/backend` |

{{% notice warning %}}
Không đưa access key, password, token, secret value hoặc chuỗi kết nối thật vào Markdown và ảnh chụp. Các giá trị nhạy cảm phải nằm trong Secrets Manager hoặc file `.env` không được commit.
{{% /notice %}}
