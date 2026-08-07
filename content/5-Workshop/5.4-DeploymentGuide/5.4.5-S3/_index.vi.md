---
title: "Cấu hình Amazon S3"
date: "2026-08-07"
weight: 5
chapter: false
pre: "<b> 5.4.5. </b>"
---

# Tạo bucket lưu ảnh ứng dụng

## 1. Tạo bucket

1. Mở **Amazon S3 → Create bucket**.
2. Bucket name đề xuất: balan-coffee-media-ACCOUNT_ID; tên S3 phải duy nhất toàn cầu.
3. Region: ap-southeast-1.
4. Object Ownership: **ACLs disabled**.
5. Default encryption: **SSE-S3** hoặc **SSE-KMS** theo yêu cầu.
6. Versioning: bật nếu cần khôi phục ảnh; có thể tắt để giảm chi phí workshop.

Backend lưu object theo các prefix: products/, users/, blogs/, categories/, logos/, banners/ và temp/.

## 2. Quyền của EC2 role

EC2 role cần s3:GetObject, s3:PutObject và s3:DeleteObject trên:

~~~text
arn:aws:s3:::<MEDIA_BUCKET>/*
~~~

Đặt tên bucket vào:

~~~dotenv
AWS_S3_BUCKET_NAME=<MEDIA_BUCKET>
AWS_REGION=ap-southeast-1
~~~

## 3. Chọn mô hình đọc ảnh

Mã nguồn hiện trả URL trực tiếp:

~~~text
https://<BUCKET>.s3.ap-southeast-1.amazonaws.com/<KEY>
~~~

Vì vậy phải chọn và ghi rõ một trong hai mô hình:

| Mô hình | Cấu hình | Đánh giá |
|---|---|---|
| Private bucket | Giữ Block Public Access; backend tạo presigned URL hoặc CloudFront phân phối media | Khuyến nghị |
| Public-read prefix | Chỉ mở quyền đọc cho prefix ảnh công khai bằng bucket policy | Chỉ dùng khi ảnh là dữ liệu công khai |

Không tắt toàn bộ Block Public Access nếu chưa có phê duyệt. CloudFront hiện tại dùng **EC2 custom origin**, không phải S3 OAC.

## 4. CORS

Luồng hiện tại upload qua Express/Nginx nên không bắt buộc cấu hình S3 CORS. Nếu sau này browser upload trực tiếp, chỉ cho phép domain CloudFront và methods/headers thật sự cần thiết.

## 5. Kiểm thử

1. Upload ảnh JPEG/PNG/WebP nhỏ hơn 10 MB.
2. Xác nhận object xuất hiện đúng prefix.
3. Xác nhận URL ảnh hiển thị theo mô hình private/public đã chọn.
4. Thử xóa ảnh và kiểm tra CloudWatch log.

**Ảnh cần chụp:** bucket Properties, Permissions/Block Public Access, encryption và một object mẫu.

