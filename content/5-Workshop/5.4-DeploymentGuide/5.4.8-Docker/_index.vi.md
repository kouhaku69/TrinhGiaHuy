---
title: "Triển khai Docker trên EC2"
date: "2026-08-07"
weight: 8
chapter: false
pre: "<b> 5.4.8. </b>"
---

# Triển khai frontend và backend bằng Docker

## 1. Cấu trúc container

| Container | Chức năng | Port |
|---|---|---|
| balan-frontend | Build React/Vite, phục vụ bằng Nginx | 80:80 |
| balan-backend | Node.js/Express API | 5000:5000, không mở trên Security Group |

Nginx proxy /api/, /health và các route upload tới backend:5000. Docker Compose chỉ khởi động frontend sau khi backend healthy.

## 2. Cài công cụ

Cài Git, Docker Engine và Docker Compose plugin theo AMI đã chọn, sau đó xác minh:

~~~bash
docker --version
docker compose version
git --version
~~~

## 3. Lấy đúng nhánh

~~~bash
git clone <AUTHORIZED_REPOSITORY_URL> balan-coffee
cd balan-coffee
git checkout aws-workshop-v2
git pull --ff-only origin aws-workshop-v2
git rev-parse --short HEAD
~~~

Ghi lại commit SHA đã triển khai. Không đặt GitHub token trong lệnh hoặc ảnh chụp.

## 4. Tạo backend/.env

~~~dotenv
NODE_ENV=production
PORT=5000
DATABASE_PROVIDER=postgres
POSTGRES_SSLMODE=no-verify
CORS_ORIGIN=https://d3pn12mzrv3aqy.cloudfront.net
FRONTEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
BACKEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
AWS_REGION=ap-southeast-1
AWS_S3_BUCKET_NAME=<MEDIA_BUCKET>
COGNITO_USER_POOL_ID=<USER_POOL_ID>
COGNITO_CLIENT_ID=<APP_CLIENT_ID>
DATABASE_SECRET_ID=balan-coffee/dev/database
SMTP_SECRET_ID=balan-coffee/dev/smtp
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AUTH_SECRET_ID=balan-coffee/dev/auth
BEDROCK_MODEL_ID=apac.amazon.nova-lite-v1:0
AUTH_COOKIE_SECURE=true
~~~

Không thêm access key hoặc secret value vào file này.

~~~bash
git check-ignore backend/.env
docker compose config
~~~

## 5. Build và chạy

~~~bash
docker compose build --pull
docker compose up -d
docker compose ps
docker compose logs --tail=100 backend
~~~

Kết quả mong đợi:

+ balan-backend ở trạng thái healthy.
+ balan-frontend ở trạng thái running.
+ Log xác nhận PostgreSQL connected và các secret được tải thành công.

## 6. Kiểm tra local

~~~bash
curl -i http://localhost/
curl -i http://localhost/health
curl -i http://localhost/api/products
~~~

![Minh chứng hai container đang chạy](/images/5-Workshop/evidence-docker-containers.png)

## 7. Cập nhật và rollback

~~~bash
git pull --ff-only origin aws-workshop-v2
docker compose up -d --build
curl -f http://localhost/health
~~~

Nếu release lỗi, checkout commit ổn định gần nhất rồi chạy lại. Rollback source không tự rollback schema/data RDS.

