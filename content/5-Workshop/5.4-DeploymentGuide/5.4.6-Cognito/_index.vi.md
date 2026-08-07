---
title: "Cấu hình Amazon Cognito"
date: "2026-08-07"
weight: 6
chapter: false
pre: "<b> 5.4.6. </b>"
---

# Tạo Cognito User Pool và App Client

## 1. Cấu hình phù hợp với mã nguồn

Backend tự gọi Cognito SDK cho đăng ký, xác minh email, đăng nhập, refresh, logout và reset mật khẩu. Ứng dụng **không dùng Hosted UI** trong luồng hiện tại và yêu cầu App Client có secret để tạo SECRET_HASH.

## 2. Tạo User Pool

1. Mở **Amazon Cognito → User pools → Create user pool**.
2. Name: balan-coffee-users.
3. Sign-in identifier: **Email**.
4. Self-registration: **Enabled**.
5. Required attributes: email; name nếu giao diện đăng ký yêu cầu.
6. Email verification: gửi mã xác minh bằng Cognito.
7. MFA: Optional hoặc No MFA cho workshop; khuyến nghị MFA cho tài khoản quản trị.

## 3. Password policy

| Cấu hình | Giá trị |
|---|---|
| Minimum length | 8 ký tự trở lên |
| Uppercase/lowercase | Required |
| Number | Required |
| Special character | Required |
| Temporary password validity | 7 ngày |

## 4. Tạo App Client

Vào **App integration → App clients → Create app client**:

| Cấu hình | Giá trị |
|---|---|
| Name | balan-coffee-web-client |
| Client type | Confidential client/có client secret |
| Authentication flow | ALLOW_USER_PASSWORD_AUTH |
| Refresh | ALLOW_REFRESH_TOKEN_AUTH |
| Token revocation | Enabled |
| Prevent user existence errors | Enabled |

Không đưa Client Secret vào frontend. Lưu secret này trong balan-coffee/dev/cognito.

## 5. Biến môi trường

~~~dotenv
COGNITO_USER_POOL_ID=<USER_POOL_ID>
COGNITO_CLIENT_ID=<APP_CLIENT_ID>
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AWS_REGION=ap-southeast-1
~~~

## 6. Kiểm thử

1. Đăng ký bằng email mới.
2. Nhận và nhập mã xác minh.
3. Đăng nhập, gọi route /api/auth/me.
4. Reload trang để kiểm tra refresh token.
5. Đăng xuất và thử flow quên mật khẩu.

{{% notice warning %}}
Log trước đây ghi nhận CORS blocked origin từ CloudFront. Phải đặt CORS_ORIGIN và FRONTEND_URL chính xác là https://d3pn12mzrv3aqy.cloudfront.net, recreate backend rồi kiểm thử lại toàn bộ flow.
{{% /notice %}}

**Ảnh cần chụp:** User Pool overview, sign-in/password policy, email verification, App Client flows; che User Pool ID/Client ID nếu tài liệu công khai.

