---
title: "Kiểm thử và Clean-up"
date: "2026-08-07"
weight: 11
chapter: false
pre: "<b> 5.4.11. </b>"
---

# Kiểm thử, nghiệm thu và dọn dẹp tài nguyên

## 1. Kiểm thử theo lớp

| Lớp | Kiểm thử | Kết quả mong đợi |
|---|---|---|
| EC2 local | curl localhost và /health | HTTP 200, PostgreSQL Connected |
| Elastic IP | http://54.251.119.230/ | Frontend tải được |
| CloudFront | HTTPS root, /health, /api/products | HTTP 200; API/health không cache |
| Cognito | Sign-up, verify, login, refresh, logout, forgot password | Không còn lỗi CORS |
| S3 | Upload/view/delete ảnh | Object đúng prefix và quyền đọc đúng mô hình |
| Bedrock | Chatbot VI/EN | Response hợp lệ |
| CloudWatch | Backend logs và dashboard | Có event/metrics mới |

## 2. Lệnh kiểm tra nhanh

~~~bash
docker compose ps
curl -f http://localhost/health
curl -f http://54.251.119.230/health
curl -f https://d3pn12mzrv3aqy.cloudfront.net/health
~~~

## 3. Tiêu chí nghiệm thu

+ Hai container running; backend healthy.
+ /health xác nhận database Connected.
+ CloudFront website và API hoạt động bằng HTTPS.
+ Login/refresh không còn CORS blocked origin.
+ RDS không public và chỉ nhận 5432 từ EC2 SG.
+ Port 5000 không mở ra Internet.
+ Không có secret/access key trong Git hoặc ảnh.
+ CloudWatch nhận log và metrics.

## 4. Clean-up

Chỉ dọn dẹp sau khi đã lưu ảnh minh chứng và backup:

1. Disable rồi xóa CloudFront distribution nếu không còn dùng.
2. Chạy docker compose down.
3. Tạo RDS final snapshot nếu cần, sau đó xóa RDS.
4. Terminate EC2.
5. Release Elastic IP không còn associate.
6. Xóa object rồi xóa S3 bucket nếu được phép.
7. Schedule deletion cho Secrets Manager hoặc xóa theo retention policy.
8. Xóa Cognito User Pool, CloudWatch log group/alarm/dashboard.
9. Xóa Security Group, route table, subnet, IGW và VPC sau cùng.
10. Xóa IAM policy/role khi không còn tài nguyên phụ thuộc.

{{% notice warning %}}
Xóa RDS, S3, Cognito hoặc secret có thể làm mất dữ liệu. Luôn xác định đúng resource, kiểm tra backup và owner trước khi xóa.
{{% /notice %}}

