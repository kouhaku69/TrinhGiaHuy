---
title: "Cấu hình AWS Secrets Manager"
date: "2026-08-07"
weight: 4
chapter: false
pre: "<b> 5.4.4. </b>"
---

# Lưu cấu hình nhạy cảm trong AWS Secrets Manager

## 1. Cách mã nguồn tải secret

File backend/config/runtimeConfig.js đọc bốn biến Secret ID, gọi GetSecretValue, parse JSON và cache kết quả trong tiến trình. Ở production, thiếu một secret bắt buộc sẽ khiến backend không khởi động.

## 2. Tạo bốn secret

Với mỗi secret, vào **Secrets Manager → Store a new secret → Other type of secret**, chọn Key/value hoặc Plaintext JSON.

### balan-coffee/dev/database

~~~json
{
  "POSTGRES_URI": "postgresql://<USER>:<PASSWORD>@<RDS_ENDPOINT>:5432/balancoffee?sslmode=no-verify"
}
~~~

### balan-coffee/dev/smtp

~~~json
{
  "EMAIL_HOST": "<SMTP_HOST>",
  "EMAIL_PORT": "587",
  "EMAIL_USER": "<SMTP_USER>",
  "EMAIL_PASSWORD": "<SMTP_PASSWORD>",
  "EMAIL_FROM": "<FROM_ADDRESS>"
}
~~~

### balan-coffee/dev/cognito

~~~json
{
  "COGNITO_CLIENT_SECRET": "<APP_CLIENT_SECRET>"
}
~~~

### balan-coffee/dev/auth

~~~json
{
  "JWT_SECRET": "<LONG_RANDOM_VALUE>",
  "SESSION_SECRET": "<DIFFERENT_LONG_RANDOM_VALUE>"
}
~~~

## 3. Encryption và rotation

+ Có thể dùng AWS managed key aws/secretsmanager cho workshop.
+ Nếu dùng customer managed KMS key, gắn thêm kms:Decrypt cho EC2 role.
+ Không bật rotation tùy ý cho JWT/session/Cognito secret nếu ứng dụng chưa có quy trình đồng bộ.

## 4. Gắn Secret ID vào backend

~~~dotenv
DATABASE_SECRET_ID=balan-coffee/dev/database
SMTP_SECRET_ID=balan-coffee/dev/smtp
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AUTH_SECRET_ID=balan-coffee/dev/auth
~~~

## 5. Kiểm tra an toàn

Chỉ kiểm tra metadata, không in SecretString vào terminal hoặc ảnh chụp:

~~~bash
aws secretsmanager describe-secret --secret-id balan-coffee/dev/database
docker compose logs --tail=100 backend
~~~

Log cần cho thấy các nhóm secret được tải thành công nhưng không được hiển thị giá trị.

