---
title: Kiến trúc giải pháp
date: "2026-08-07"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Kiến trúc giải pháp

---

# Mục lục

1. Mục đích
2. Nguyên tắc kiến trúc
3. Tổng quan giải pháp
4. Kiến trúc tổng thể
5. Các thành phần hệ thống
6. Luồng yêu cầu
7. Luồng dữ liệu
8. Các dịch vụ AWS
9. Kiến trúc mạng
10. Kiến trúc bảo mật
11. Giám sát và khả năng quan sát
12. Các quyết định thiết kế
13. Hướng phát triển trong tương lai

---

# 1. Mục đích

Tài liệu này mô tả kiến trúc kỹ thuật của nền tảng Balan Coffee & Roastery sau khi được hiện đại hóa trên AWS. Tài liệu cung cấp cái nhìn tổng quan về các thành phần hệ thống, cơ sở hạ tầng đám mây, sự tương tác giữa các dịch vụ và những quyết định kiến trúc hỗ trợ việc triển khai ứng dụng trên Amazon Web Services (AWS).

---

# 2. Nguyên tắc kiến trúc

Giải pháp được thiết kế theo các nguyên tắc sau:

- Kiến trúc cloud-native
- Ưu tiên sử dụng các dịch vụ được quản lý khi phù hợp
- Tách biệt ứng dụng và cơ sở hạ tầng
- Cấu hình bảo mật theo mặc định
- Ưu tiên khả năng quan sát trong vận hành
- Thiết kế theo mô-đun và dễ bảo trì
- Triển khai bằng container

---

# 3. Tổng quan giải pháp

Ứng dụng được triển khai trên một Amazon EC2 instance bằng Docker Compose. Amazon CloudFront đóng vai trò là lớp phân phối nội dung cho các yêu cầu từ người dùng.

Backend giao tiếp với các dịch vụ được quản lý của AWS, bao gồm Amazon RDS, Amazon S3, Amazon Bedrock, Amazon Cognito, Amazon SES và AWS Secrets Manager.

Khả năng quan sát hoạt động được cung cấp bởi Amazon CloudWatch và CloudWatch Agent đang chạy trên EC2 instance.

---

# 4. Kiến trúc tổng thể


![Architecture Diagram](/images/5-Workshop/ArchitectureDiagram.jpg)

**Hình 1.** Kiến trúc hiện đại hóa Balan Coffee & Roastery trên AWS.

---

# 5. Các thành phần hệ thống

## Lớp người dùng

- Trình duyệt web
- Yêu cầu HTTP

Trách nhiệm:

- Truy cập ứng dụng web
- Gửi yêu cầu của người dùng
- Hiển thị giao diện ứng dụng

---

## Lớp phân phối nội dung

### Amazon CloudFront

Trách nhiệm:

- Phân phối nội dung trên phạm vi toàn cầu
- Kết thúc kết nối HTTP
- Lưu nội dung vào bộ nhớ đệm tại edge location
- Tối ưu hóa hiệu năng

---

## Lớp điện toán

### Amazon EC2

Trách nhiệm:

- Lưu trữ ứng dụng
- Thực thi các Docker container
- Kết nối với các dịch vụ được quản lý của AWS

### Docker

Các container:

- Frontend
- Backend

---

## Lớp dữ liệu

### Amazon RDS for PostgreSQL

Trách nhiệm:

- Cung cấp cơ sở dữ liệu quan hệ có khả năng lưu trữ lâu dài
- Lưu trữ dữ liệu ứng dụng
- Xử lý giao dịch

---

## Các dịch vụ AWS hỗ trợ

### Amazon S3

Lưu trữ đối tượng.

### Amazon Cognito

Xác thực và phân quyền.

### Amazon SES

Gửi email.

### Amazon Bedrock

Cung cấp chatbot và tính năng đề xuất được hỗ trợ bởi AI.

### AWS Secrets Manager

Quản lý cấu hình ứng dụng một cách an toàn.

---

## Lớp giám sát

### Amazon CloudWatch Agent

Thu thập:

- Chỉ số máy chủ
- Nhật ký ứng dụng

### Amazon CloudWatch

Cung cấp:

- Chỉ số giám sát
- Nhật ký
- Bảng điều khiển

---

# 6. Luồng yêu cầu

## Yêu cầu của người dùng

```
Người dùng
    │
HTTP
    ▼
CloudFront
    │
Internet Gateway
    ▼
EC2
    │
Frontend
    │
Backend
```

---

## Xử lý tại Backend

```
Backend
    │
    ├── Amazon RDS
    ├── Amazon S3
    ├── Amazon Cognito
    ├── Amazon SES
    ├── Amazon Bedrock
    └── AWS Secrets Manager
```

---

## Luồng giám sát

```
EC2
    │
CloudWatch Agent
    │
CloudWatch
```

Dữ liệu giám sát được thu thập bao gồm:

- Chỉ số cơ sở hạ tầng
- Nhật ký ứng dụng
- Chỉ số trên bảng điều khiển

---

# 7. Luồng dữ liệu

## Xác thực

```
Người dùng

↓

Frontend

↓

Backend

↓

Amazon Cognito
```

---

## Danh sách sản phẩm

```
Frontend

↓

Backend

↓

Amazon RDS
```

---

## Tải hình ảnh lên

```
Frontend

↓

Backend

↓

Amazon S3
```

---

## Đề xuất bằng AI

```
Người dùng

↓

Frontend

↓

Backend

↓

Amazon Bedrock

↓

Phản hồi của AI
```

---

# 8. Các dịch vụ AWS

| Dịch vụ | Vai trò |
|---|---|
| Amazon EC2 | Năng lực điện toán |
| Docker | Môi trường chạy ứng dụng |
| Amazon CloudFront | Phân phối nội dung |
| Amazon RDS | Cơ sở dữ liệu |
| Amazon S3 | Lưu trữ đối tượng |
| Amazon Cognito | Xác thực người dùng |
| Amazon SES | Gửi email |
| Amazon Bedrock | Trí tuệ nhân tạo |
| AWS Secrets Manager | Lưu trữ thông tin bí mật |
| Amazon CloudWatch | Giám sát hệ thống |

---

# 9. Kiến trúc mạng

## Public subnet

- Amazon EC2
- Internet Gateway
- Elastic IP

## Private subnet

- Amazon RDS for PostgreSQL

Security Group giới hạn kết nối giữa tài nguyên ứng dụng và tài nguyên cơ sở dữ liệu.

---

# 10. Kiến trúc bảo mật

Giải pháp áp dụng nhiều cơ chế bảo mật.

## Cơ sở hạ tầng

- Security Group
- Private subnet dành cho cơ sở dữ liệu

## Ứng dụng

- Amazon Cognito
- Xác thực bằng JWT

## Cấu hình

- AWS Secrets Manager

## Truyền dữ liệu

- HTTPS
- Amazon CloudFront

---

# 11. Giám sát và khả năng quan sát

Ứng dụng được giám sát bằng Amazon CloudWatch.

Các khả năng bao gồm:

- Theo dõi chỉ số cơ sở hạ tầng
- Thu thập nhật ký
- Trực quan hóa trên bảng điều khiển

CloudWatch Agent liên tục gửi dữ liệu đo lường từ EC2 instance lên CloudWatch.

---

# 12. Các quyết định thiết kế

| Quyết định | Lý do |
|---|---|
| Docker Compose | Đơn giản hóa quá trình triển khai trong workshop |
| Amazon EC2 | Cung cấp toàn quyền kiểm soát cơ sở hạ tầng |
| Amazon CloudFront | Cải thiện hiệu năng và hỗ trợ HTTPS |
| Amazon RDS | Cung cấp cơ sở dữ liệu quan hệ được quản lý |
| Amazon CloudWatch | Giám sát tập trung |
| AWS Secrets Manager | Quản lý cấu hình an toàn |
| Amazon Bedrock | Tích hợp khả năng AI |

---

# 13. Hướng phát triển trong tương lai

Kiến trúc có thể được mở rộng bằng các dịch vụ và giải pháp sau:

- Amazon ECS
- Application Load Balancer
- Auto Scaling Group
- Route 53
- AWS WAF
- AWS Certificate Manager
- Quy trình CI/CD
- Infrastructure as Code (AWS CloudFormation hoặc Terraform)

---
