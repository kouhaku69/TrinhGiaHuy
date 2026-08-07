---
title: "Cấu hình Amazon CloudFront"
date: "2026-08-07"
weight: 9
chapter: false
pre: "<b> 5.4.9. </b>"
---

# Tạo CloudFront distribution cho EC2 origin

## 1. Tạo distribution

1. Mở **CloudFront → Distributions → Create distribution**.
2. Origin type: **Other/custom origin**.
3. Origin domain: dùng **EC2 public DNS** đang phân giải về Elastic IP 54.251.119.230.
4. Không nhập raw Elastic IP vào trường Origin domain.
5. Protocol: **HTTP only**, port 80 cho kiến trúc hiện tại.
6. Viewer protocol policy: **Redirect HTTP to HTTPS**.
7. Default root object: index.html.
8. Compress objects automatically: Yes.

Distribution đang vận hành:

~~~text
https://d3pn12mzrv3aqy.cloudfront.net
~~~

## 2. Default behavior cho frontend

| Cấu hình | Giá trị |
|---|---|
| Path | Default (*) |
| Allowed methods | GET, HEAD, OPTIONS |
| Cache policy | CachingOptimized hoặc policy static tương đương |
| Origin request policy | Chỉ chuyển tiếp dữ liệu cần thiết |
| Viewer policy | Redirect HTTP to HTTPS |

Nginx đã có SPA fallback. Chỉ tạo custom error response 403/404 → /index.html khi đã kiểm thử để không che lỗi API.

## 3. Ordered behaviors cho route động

| Path pattern | Allowed methods | Cache policy | Origin request |
|---|---|---|---|
| /api/* | Tất cả methods cần thiết | CachingDisabled | Cookie, query string và headers cần thiết |
| /health | GET, HEAD | CachingDisabled | Tối thiểu |
| /uploads/* | GET, HEAD | CachingDisabled hoặc TTL ngắn | Theo mô hình media |
| /backend/uploads/* | GET, HEAD | CachingDisabled hoặc TTL ngắn | Theo mô hình media |

Flow Cognito dùng cookie và credentials: true, vì vậy /api/* phải chuyển tiếp cookie.

## 4. CORS và cookie

~~~dotenv
CORS_ORIGIN=https://d3pn12mzrv3aqy.cloudfront.net
FRONTEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
BACKEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
AUTH_COOKIE_SECURE=true
~~~

~~~bash
docker compose up -d --force-recreate backend
~~~

## 5. Kiểm thử

~~~bash
curl -I https://d3pn12mzrv3aqy.cloudfront.net/
curl -i https://d3pn12mzrv3aqy.cloudfront.net/health
curl -i https://d3pn12mzrv3aqy.cloudfront.net/api/products
~~~

Kiểm tra login/refresh trong browser; ảnh log cũ cho thấy CORS từng chặn CloudFront origin.

## 6. Hardening origin

Khi không cần direct EIP test, giới hạn EC2 Security Group port 80 bằng CloudFront origin-facing managed prefix list. Kiến trúc hiện tại mã hóa viewer → CloudFront nhưng CloudFront → EC2 vẫn dùng HTTP; production nên bổ sung TLS tới origin.

**Ảnh cần chụp:** distribution overview, origin, default behavior, /api/* behavior và kết quả /health.

