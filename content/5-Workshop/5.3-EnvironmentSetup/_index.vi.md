---
title: "Chuẩn bị môi trường"
date: "2026-08-07"
weight: 3
chapter: false
pre: "<b> 5.3. </b>"
---

## Chuẩn bị môi trường

{{% notice info %}}
Phần này là checklist trước triển khai. Các thao tác AWS Console chi tiết cho VPC, IAM/EC2, RDS, Secrets Manager, S3, Cognito, Bedrock, CloudFront và CloudWatch nằm tại [5.4 — Cấu hình AWS và triển khai](../5.4-DeploymentGuide/).
{{% /notice %}}

### 1. Điều kiện tiên quyết

+ Có quyền truy cập private repository `trinpce192008/AWS_Workshop`, nhánh `aws-workshop-v2`.
+ Có quyền tạo/cấu hình VPC, EC2, Elastic IP, IAM, RDS, S3, Cognito, Bedrock, Secrets Manager, CloudFront và CloudWatch.
+ Sử dụng region `ap-southeast-1` nếu không chủ động thay đổi toàn bộ tài nguyên và biến môi trường.
+ EC2 đã cài Docker Engine và Docker Compose plugin.
+ Có kế hoạch lưu và phân quyền secret production.

### 2. Ghi nhận thông tin môi trường thực tế

Repository không chứa toàn bộ định danh hạ tầng. Điền bảng sau bằng thông tin AWS Console trước khi nộp báo cáo cuối:

| Hạng mục | Giá trị đã xác nhận/mặc định | Giá trị thực tế |
|---|---|---|
| Region | `ap-southeast-1` | |
| EC2 instance ID/type/AZ | Ảnh AWS Console | `i-03642ee2788132cb3` / `t3.medium` / `ap-southeast-1a` |
| Elastic IP | Console và kiểm thử endpoint | `54.251.119.230` |
| CloudFront domain | Distribution đang chạy | `d3pn12mzrv3aqy.cloudfront.net` |
| VPC/public subnet | Không có trong repository | |
| EC2 Security Group | Không có trong repository | |
| RDS identifier | `balan-coffee-postgres-dev` trong báo cáo migration | |
| Database/class | `balancoffee`, `db.t4g.micro` trong báo cáo migration | |
| S3 bucket | Biến `AWS_S3_BUCKET_NAME` | |
| Cognito | User Pool ID và App Client ID | |
| Secrets | Database, SMTP, Cognito và Auth secret IDs | |
| CloudWatch log group | `/balancoffee/backend` | |

### 3. Chuẩn bị mạng

1. Chọn VPC tại `ap-southeast-1`.
2. Chọn hoặc tạo public subnet.
3. Gắn Internet Gateway với VPC.
4. Thêm route `0.0.0.0/0 → Internet Gateway`.
5. Tạo EC2 Security Group: khi test origin, mở TCP 80 từ nguồn quản trị; sau khi xác minh, ưu tiên CloudFront origin-facing managed prefix list. Chỉ mở TCP 22 từ IP quản trị nếu không dùng Session Manager và không mở TCP 5000.
6. Tạo RDS Security Group chỉ nhận TCP 5432 từ EC2 Security Group.

### 4. IAM role cho EC2

Gắn instance profile với quyền tối thiểu:

| Dịch vụ | Quyền cần thiết |
|---|---|
| Secrets Manager | `secretsmanager:GetSecretValue` trên bốn secret ARN của ứng dụng. |
| S3 | `s3:PutObject`, `s3:DeleteObject` và quyền đọc cần thiết trên bucket/prefix được chọn. |
| Cognito IDP | Các thao tác đăng ký, xác minh, xác thực, reset và admin thực sự được mã nguồn gọi. |
| Bedrock | `bedrock:InvokeModel` đối với model/inference profile đã chọn. |
| CloudWatch Logs | `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents` và quyền describe cần thiết. |
| Systems Manager | Managed instance permissions nếu sử dụng Session Manager. |

Không đặt access key dài hạn trong `backend/.env`. AWS SDK sẽ tự dùng EC2 IAM role khi không có static credentials.

### 5. Chuẩn bị RDS PostgreSQL

1. Tạo/chọn PostgreSQL instance và database `balancoffee`.
2. Áp dụng schema từ `backend/database/postgres/schema.sql` nếu file có trong revision triển khai.
3. Bật backup và ghi rõ retention period.
4. Kiến trúc mục tiêu đặt RDS trong private subnet, chỉ nhận port 5432 từ EC2 Security Group.
5. Lưu chuỗi kết nối dưới dạng JSON trong database secret:

```json
{
  "POSTGRES_URI": "postgresql://USERNAME:PASSWORD@RDS_ENDPOINT:5432/balancoffee?sslmode=no-verify"
}
```

### 6. Chuẩn bị secret

| Biến chọn secret | JSON keys cần có |
|---|---|
| `DATABASE_SECRET_ID` | `POSTGRES_URI` |
| `SMTP_SECRET_ID` | `EMAIL_HOST`, `EMAIL_PORT`, `EMAIL_USER`, `EMAIL_PASSWORD`, có thể thêm `EMAIL_FROM` |
| `COGNITO_SECRET_ID` | `COGNITO_CLIENT_SECRET` |
| `AUTH_SECRET_ID` | `JWT_SECRET`, `SESSION_SECRET` |

### 7. Chuẩn bị S3, Cognito và Bedrock

+ **S3:** tạo bucket ảnh cùng region, giữ Block Public Access nếu chưa có thiết kế public, đặt `AWS_S3_BUCKET_NAME` đúng tên bucket.
+ **Cognito:** tạo User Pool và App Client, ghi nhận `COGNITO_USER_POOL_ID`, `COGNITO_CLIENT_ID`, lưu client secret trong Secrets Manager nếu có.
+ **Bedrock:** xác nhận quyền truy cập `apac.amazon.nova-lite-v1:0` hoặc đặt `BEDROCK_MODEL_ID` khác đã được phê duyệt; giới hạn IAM chỉ cho model cần dùng.

#### CloudFront

+ Dùng EC2 public DNS đang phân giải về `54.251.119.230` làm custom origin domain.
+ Origin dùng HTTP cổng 80; viewer được redirect từ HTTP sang HTTPS.
+ Dùng `CachingDisabled` cho `/api/*` và `/health`; chuyển tiếp cookie, header, query string và methods cần thiết cho `/api/*`.
+ Đặt default root object là `index.html` và cấu hình SPA fallback khi cần.

### 8. Cấu hình ứng dụng

Frontend `.env`:

```dotenv
VITE_API_URL=/api
VITE_APP_URL=https://d3pn12mzrv3aqy.cloudfront.net
VITE_APP_NAME=Balan Coffee & Roastery
```

`src/config/api.js` hiện chủ động dùng production base URL rỗng, vì vậy browser gọi API cùng origin và request được Nginx proxy, không cần cấu hình một API host tuyệt đối.

`backend/.env`:

```dotenv
NODE_ENV=production
PORT=5000
DATABASE_PROVIDER=postgres
POSTGRES_SSLMODE=no-verify
CORS_ORIGIN=https://d3pn12mzrv3aqy.cloudfront.net
FRONTEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
BACKEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
AWS_REGION=ap-southeast-1
AWS_S3_BUCKET_NAME=<BUCKET_NAME>
COGNITO_USER_POOL_ID=<USER_POOL_ID>
COGNITO_CLIENT_ID=<APP_CLIENT_ID>
DATABASE_SECRET_ID=balan-coffee/dev/database
SMTP_SECRET_ID=balan-coffee/dev/smtp
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AUTH_SECRET_ID=balan-coffee/dev/auth
BEDROCK_MODEL_ID=apac.amazon.nova-lite-v1:0
AUTH_COOKIE_SECURE=true
```

Điểm truy cập production dùng HTTPS nên phải đặt `AUTH_COOKIE_SECURE=true`. Ảnh log cho thấy request xác thực từ CloudFront từng bị CORS chặn; hãy kiểm tra đúng các giá trị trên rồi chạy `docker compose up -d --force-recreate backend`.

### 9. Checklist sẵn sàng

+ [ ] EC2 kết nối được RDS qua 5432.
+ [ ] EC2 gọi được Secrets Manager, S3, Cognito, Bedrock và CloudWatch qua HTTPS.
+ [ ] Elastic IP đã được cấp và sẵn sàng gắn với EC2.
+ [ ] CloudFront trỏ tới EC2 public DNS và behavior API/health đã tắt cache.
+ [ ] `CORS_ORIGIN` và `FRONTEND_URL` khớp chính xác URL HTTPS CloudFront.
+ [ ] `backend/.env` tồn tại trên EC2 nhưng không được Git theo dõi.
+ [ ] Repository không chứa AWS key hoặc secret value.
+ [ ] Đã ghi nhận backup, retention và người chịu trách nhiệm clean-up.
