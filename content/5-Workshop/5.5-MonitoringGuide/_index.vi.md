---
title: "Hướng dẫn giám sát"
date: "2026-08-07"
weight: 5
chapter: false
pre: "<b> 5.5. </b>"
---

## Hướng dẫn giám sát

### 1. Mục tiêu

Hệ thống giám sát phải trả lời được:

1. EC2 có hoạt động và truy cập được không?
2. Hai container có chạy và backend health check có thành công không?
3. Backend có kết nối được RDS và các AWS service không?
4. Người dùng có gặp nhiều lỗi hoặc độ trễ bất thường không?
5. CloudFront có kết nối origin và phân phối đúng nội dung không?

### 2. Cấu hình log trong repository

Backend sử dụng Docker `awslogs` driver:

| Thuộc tính | Giá trị |
|---|---|
| Region | `ap-southeast-1` |
| Log group | `/balancoffee/backend` |
| Log stream | `backend` |
| Create group | `true` |

EC2 IAM role phải có quyền tạo/sử dụng log group và gửi log event. Nếu log driver không khởi tạo được, container có thể lỗi trước khi ứng dụng chạy.

### 3. Kiểm tra vận hành cơ bản

```bash
docker compose ps
docker inspect --format '{{json .State.Health}}' balan-backend
curl -f http://localhost/health
curl -f http://54.251.119.230/health
curl -f https://d3pn12mzrv3aqy.cloudfront.net/health
```

Sử dụng `docker compose logs --tail=100 backend` để chẩn đoán nhanh và CloudWatch Logs để tra cứu lịch sử.

### 4. CloudWatch dashboard

| Widget | Metric/nguồn | Mục đích |
|---|---|---|
| EC2 CPU | `AWS/EC2 – CPUUtilization` | Phát hiện tải CPU kéo dài. |
| EC2 network | `NetworkIn`, `NetworkOut` | Nhận biết thay đổi traffic. |
| EC2 status | `StatusCheckFailed` | Phát hiện lỗi host/instance. |
| RDS CPU | `AWS/RDS – CPUUtilization` | Phát hiện database quá tải. |
| RDS connections | `DatabaseConnections` | Phát hiện connection leak/cạn capacity. |
| RDS storage | `FreeStorageSpace` | Ngăn hết dung lượng. |
| RDS latency | Read/write latency | Phát hiện thao tác database chậm. |
| Backend logs | CloudWatch Logs Insights | Đếm lỗi và xem request pattern. |
| CloudFront | Requests, cache hit, 4xx/5xx | Phát hiện lỗi edge/origin và cache. |

Dashboard được cung cấp xác nhận CloudWatch Agent đã gửi memory và disk metrics, bên cạnh CPU, network và status check tiêu chuẩn của EC2.

![Minh chứng CloudWatch EC2 dashboard](/images/5-Workshop/evidence-cloudwatch-dashboard.png)

Trong khung ba giờ ngày 07/08/2026: memory khoảng 15,3%–19,1%, disk tăng từ khoảng 62,6% lên 64,4%, CPU tiêu chuẩn phần lớn dưới 1% và có đỉnh ngắn gần 7,37%, `StatusCheckFailed` luôn bằng 0. Đây chỉ là số liệu của cửa sổ ảnh chụp, chưa phải baseline dài hạn.

### 5. Alarm khuyến nghị

Ngưỡng cần được điều chỉnh sau khi có baseline:

| Alarm | Điều kiện ban đầu | Phản ứng |
|---|---|---|
| EC2 status | `StatusCheckFailed >= 1` trong 2 chu kỳ | Điều tra hoặc phục hồi instance. |
| EC2 CPU | Trên 80% trong 15 phút | Kiểm tra traffic/container và right-size. |
| RDS storage | Thấp hơn ngưỡng đã thống nhất | Tăng storage hoặc dọn dữ liệu. |
| RDS connections | Gần giới hạn an toàn | Kiểm tra pool và request bị treo. |
| Backend errors | Lặp lại `ERROR`, `503` hoặc exception | Đối chiếu RDS và AWS service. |
| Không có log | Không có event khi lẽ ra có traffic | Kiểm tra container, IAM và log driver. |
| CloudFront 5xx | Trên 5% trong 5 phút làm ngưỡng ban đầu | Kiểm tra origin, EC2/Nginx và behavior. |

Alarm phải gửi tới kênh đã được phê duyệt và có người chịu trách nhiệm xử lý.

### 6. CloudWatch Logs Insights

Tìm lỗi gần nhất:

```text
fields @timestamp, @message
| filter @message like /ERROR|Error|503/
| sort @timestamp desc
| limit 50
```

Tìm health-check:

```text
fields @timestamp, @message
| filter @message like /Health check|health/
| sort @timestamp desc
| limit 50
```

Không ghi password, token, secret, authorization header đầy đủ hoặc dữ liệu khách hàng nhạy cảm vào log.

### 7. Quy trình xử lý sự cố

1. Kiểm tra trạng thái CloudFront distribution, behavior và metric 4xx/5xx.
2. Gọi `/health` qua CloudFront rồi so sánh với Elastic IP và local health.
3. Xác nhận Elastic IP gắn với `i-03642ee2788132cb3` và xem EC2 status checks.
4. Kiểm tra Security Group, route table và CloudFront origin.
5. Chạy `docker compose ps` và xem backend log gần nhất trên CloudWatch.
6. Kiểm tra trạng thái, connection, storage và Security Group của RDS.
7. Chỉ kiểm tra quyền Secrets Manager, Cognito, S3 hoặc Bedrock khi lỗi liên quan dịch vụ đó.
8. Rollback nếu sự cố xuất hiện ngay sau deployment.

### 8. Bảng triệu chứng

| Triệu chứng | Khu vực nghi ngờ | Kiểm tra đầu tiên |
|---|---|---|
| Website không mở | CloudFront, origin, SG, Nginx, frontend | So sánh CloudFront, EIP, local health và `docker compose ps`. |
| Login/refresh bị chặn | Backend CORS hoặc CloudFront cookie forwarding | `CORS_ORIGIN`, `FRONTEND_URL`, API behavior và cookies. |
| `/health` trả 503 | RDS hoặc secret | RDS status, `POSTGRES_URI`, port 5432. |
| Đăng nhập lỗi | Cognito | User Pool/App Client và IAM. |
| Upload ảnh lỗi | S3 | Bucket, region, IAM và cơ chế đọc object. |
| Chatbot lỗi | Bedrock | Model/inference profile và `bedrock:InvokeModel`. |
| Không có log | CloudWatch Logs | EC2 IAM role và cấu hình `awslogs`. |

### 9. Checklist minh chứng

+ [ ] CloudWatch log group và backend events gần nhất.
+ [x] EC2 metrics dashboard và status checks đã có.
+ [ ] RDS metrics.
+ [ ] Dashboard có các widget đã thống nhất.
+ [ ] Ít nhất một alarm đã được test.
+ [ ] Local/public health response thành công.
+ [ ] CloudFront request/error/cache metrics và kiểm thử xác thực sau khi sửa CORS.
+ [ ] Ảnh chụp không chứa credential hoặc dữ liệu khách hàng.
