---
title: Tài liệu thiết kế giải pháp
date: "2026-08-07"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Tài liệu thiết kế giải pháp

> Hiện đại hóa Balan Coffee & Roastery trên AWS

---

# Mục lục

1. Tóm tắt tổng quan
2. Bối cảnh kinh doanh
3. Phát biểu vấn đề
4. Mục tiêu dự án
5. Phạm vi dự án
6. Các bên liên quan
7. Tổng quan giải pháp
8. Kiến trúc tổng thể
9. Ngăn xếp công nghệ
10. Các dịch vụ AWS
11. Yêu cầu phi chức năng
12. Các yêu cầu về bảo mật
13. Chiến lược triển khai
14. Giám sát và khả năng quan sát
15. Rủi ro
16. Lộ trình phát triển

---

# 1. Tóm tắt tổng quan

## Bối cảnh

Balan Coffee & Roastery là một ứng dụng web thương mại điện tử hiện có, ban đầu được phát triển bằng React, Express.js và MongoDB.

Dự án này hiện đại hóa ứng dụng bằng cách di chuyển cơ sở hạ tầng và các dịch vụ nền tảng cốt lõi lên Amazon Web Services (AWS), đồng thời đưa vào sử dụng các dịch vụ đám mây được quản lý nhằm cải thiện khả năng mở rộng, bảo mật, bảo trì và khả năng quan sát hoạt động.

Thay vì thiết kế lại ứng dụng nghiệp vụ, dự án tập trung vào việc áp dụng điện toán đám mây và hiện đại hóa cơ sở hạ tầng.

---

# 2. Bối cảnh kinh doanh

## Những thách thức hiện tại

Mô hình triển khai cũ có một số hạn chế trong vận hành:

- Quy trình triển khai thủ công
- Khả năng mở rộng hạn chế
- Không có hệ thống giám sát tập trung
- Chưa tối ưu hóa việc phân phối nội dung
- Quản lý thông tin bí mật cục bộ
- Khả năng tích hợp với các dịch vụ cloud-native còn hạn chế

Những hạn chế này làm giảm hiệu quả vận hành và mức độ sẵn sàng khi đưa hệ thống vào môi trường thực tế.

---

# 3. Phát biểu vấn đề

Làm thế nào để hiện đại hóa nền tảng Balan Coffee & Roastery hiện có bằng các dịch vụ được quản lý của AWS, đồng thời vẫn duy trì các chức năng nghiệp vụ hiện tại của ứng dụng?

---

# 4. Mục tiêu dự án

Dự án hướng đến các mục tiêu sau:

- Hiện đại hóa cơ sở hạ tầng ứng dụng
- Cải thiện khả năng mở rộng trong vận hành
- Tăng cường bảo mật ứng dụng
- Xây dựng hệ thống giám sát tập trung
- Tích hợp các tính năng trí tuệ nhân tạo
- Nâng cao tính nhất quán của quá trình triển khai
- Minh họa các phương pháp thực hành tốt trong kiến trúc đám mây AWS

---

# 5. Phạm vi dự án

## Trong phạm vi

- Amazon EC2
- Docker và Docker Compose
- Amazon RDS for PostgreSQL
- Amazon CloudFront
- Amazon CloudWatch
- Amazon S3
- Amazon Cognito
- Amazon SES
- Amazon Bedrock
- AWS Secrets Manager

## Ngoài phạm vi

- Quy trình CI/CD
- Auto Scaling
- Load Balancer
- Route 53
- Infrastructure as Code
- Kubernetes/Amazon ECS
- Khôi phục sau thảm họa

---

# 6. Các bên liên quan

| Vai trò | Trách nhiệm |
|---|---|
| Quản lý dự án | Điều phối dự án |
| Kỹ sư hạ tầng | Xây dựng cơ sở hạ tầng AWS |
| Lập trình viên Backend | Phát triển API và di chuyển cơ sở dữ liệu |
| Lập trình viên Frontend | Tích hợp giao diện người dùng |
| Kỹ sư AI | Tích hợp Amazon Bedrock |

---

# 7. Tổng quan giải pháp

Giải pháp áp dụng kiến trúc cloud-native trong khi vẫn duy trì logic hiện có của ứng dụng.

Các cải tiến chính bao gồm:

- Triển khai ứng dụng bằng container
- Sử dụng cơ sở dữ liệu PostgreSQL được quản lý
- Phân phối nội dung trên phạm vi toàn cầu
- Xác thực an toàn
- Khả năng đề xuất được hỗ trợ bởi AI
- Giám sát tập trung
- Lưu trữ thông tin bí mật bằng dịch vụ được quản lý

---

# 8. Kiến trúc tổng thể

![Architecture Diagram](/images/5-Workshop/ArchitectureDiagram.jpg)

---

# 9. Ngăn xếp công nghệ

## Frontend

- React
- Vite

## Backend

- Express.js
- Node.js

## Cơ sở dữ liệu

- PostgreSQL

## Cơ sở hạ tầng

- Docker
- Docker Compose

---

# 10. Các dịch vụ AWS

| Dịch vụ | Mục đích sử dụng |
|---|---|
| Amazon EC2 | Năng lực điện toán |
| Amazon CloudFront | Mạng phân phối nội dung |
| Amazon RDS | Cơ sở dữ liệu |
| Amazon S3 | Lưu trữ đối tượng |
| Amazon Cognito | Xác thực người dùng |
| Amazon SES | Gửi email |
| Amazon Bedrock | Trí tuệ nhân tạo |
| Amazon CloudWatch | Giám sát hệ thống |
| AWS Secrets Manager | Quản lý thông tin bí mật |

---

# 11. Yêu cầu phi chức năng

## Tính sẵn sàng

- Cho phép truy cập ứng dụng công khai
- Cơ sở hạ tầng hoạt động ổn định

## Bảo mật

- IAM Role
- AWS Secrets Manager
- Security Group

## Hiệu năng

- Lưu nội dung vào bộ nhớ đệm của CloudFront
- Sử dụng cơ sở dữ liệu được quản lý

## Khả năng bảo trì

- Triển khai bằng Docker
- Kiến trúc theo mô-đun

## Khả năng quan sát

- CloudWatch Metrics
- CloudWatch Logs
- CloudWatch Dashboard

---

# 12. Các yêu cầu về bảo mật

- Sử dụng IAM Role để truy cập các dịch vụ AWS
- Lưu trữ thông tin bí mật trong AWS Secrets Manager
- Đặt cơ sở dữ liệu trong private subnet
- Cô lập tài nguyên bằng Security Group
- Phân phối nội dung qua HTTPS bằng CloudFront

---

# 13. Chiến lược triển khai

1. Khởi tạo cơ sở hạ tầng AWS
2. Cấu hình mạng
3. Cấu hình AWS Secrets Manager
4. Triển khai các Docker container
5. Cấu hình Amazon CloudFront
6. Cấu hình Amazon CloudWatch
7. Kiểm tra và xác nhận quá trình triển khai

---

# 14. Giám sát và khả năng quan sát

Amazon CloudWatch cung cấp:

- Chỉ số cơ sở hạ tầng
- Nhật ký ứng dụng
- Bảng điều khiển giám sát

CloudWatch Agent thu thập dữ liệu đo lường từ EC2 instance.

---

# 15. Rủi ro

| Rủi ro | Biện pháp giảm thiểu |
|---|---|
| Di chuyển cơ sở dữ liệu | Di chuyển theo từng giai đoạn |
| Đồng bộ thông tin bí mật | Sử dụng AWS Secrets Manager |
| Triển khai Docker | Sử dụng Docker Compose |
| Bộ nhớ đệm CloudFront | Thực hiện cache invalidation |

---

# 16. Lộ trình phát triển

Các hạng mục có thể được bổ sung trong tương lai:

- Quy trình CI/CD
- Amazon ECS
- Auto Scaling
- Route 53
- AWS WAF
- AWS Certificate Manager (ACM)
- Triển khai Multi-AZ
- Infrastructure as Code
