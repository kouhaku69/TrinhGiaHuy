---
title: "Workshop"
date: "2026-08-07"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai Balan Coffee & Roastery trên AWS với Docker và Amazon EC2

#### Tổng quan

Workshop hướng dẫn đầy đủ quá trình thiết kế, cấu hình AWS, triển khai và kiểm thử ứng dụng **Balan Coffee & Roastery**. Nội dung được xây dựng từ repository `trinpce192008/AWS_Workshop`, nhánh `aws-workshop-v2`, đồng thời trình bày theo cách từng bước như báo cáo mẫu `workshop-template/content/5-Workshop`.

Luồng truy cập đang vận hành:

**User → HTTPS → Amazon CloudFront → EC2 public DNS/Elastic IP `54.251.119.230` → Amazon EC2 → Docker frontend/Nginx → Docker backend**

Backend sử dụng Amazon RDS for PostgreSQL và tích hợp Amazon S3, Amazon Cognito, Amazon Bedrock, AWS Secrets Manager cùng Amazon CloudWatch.

#### Nội dung workshop

1. [Tài liệu thiết kế giải pháp](5.1-SolutionDesignDocument/)
2. [Kiến trúc giải pháp](5.2-SolutionArchitecture/)
3. [Chuẩn bị môi trường](5.3-EnvironmentSetup/)
4. [Cấu hình AWS và triển khai](5.4-DeploymentGuide/)
5. [Hướng dẫn giám sát](5.5-MonitoringGuide/)
6. [Các quyết định kiến trúc](5.6-ArchitectureDecisions/)

{{% notice info %}}
Điểm truy cập chính là [CloudFront](https://d3pn12mzrv3aqy.cloudfront.net). Địa chỉ [Elastic IP](http://54.251.119.230/) được dùng làm origin ổn định và hỗ trợ kiểm tra trực tiếp. Các tên tài nguyên chưa được cung cấp trong ảnh hoặc repository được ghi dưới dạng **giá trị đề xuất**; cần thay bằng ID thực tế trước khi nộp báo cáo cuối.
{{% /notice %}}
