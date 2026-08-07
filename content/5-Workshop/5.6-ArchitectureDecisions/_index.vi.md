---
title: "Các quyết định kiến trúc"
date: "2026-08-07"
weight: 6
chapter: false
pre: "<b> 5.6. </b>"
---

## Các quyết định kiến trúc

Nội dung dưới đây ghi nhận những quyết định phản ánh đúng repository `aws-workshop-v2` và luồng triển khai đã xác nhận.

### ADR-001 — Một EC2 và Docker Compose

**Trạng thái:** Chấp nhận cho workshop.

**Quyết định:** Chạy frontend và backend trên cùng một EC2 bằng Docker Compose.

**Lý do:** Dễ học, triển khai, trình diễn và khắc phục lỗi; đồng thời giảm chi phí.

**Hệ quả:** EC2 là single point of failure, chỉ scale thủ công và deployment có thể ảnh hưởng cả hai service. Production nên có nhiều task/instance sau ALB.

### ADR-002 — Elastic IP làm địa chỉ origin ổn định

**Trạng thái:** Chấp nhận cho phiên bản hiện tại.

**Quyết định:** Gắn Elastic IP `54.251.119.230` với public EC2. CloudFront dùng EC2 public DNS đang phân giải về địa chỉ này làm custom origin.

**Lý do:** EC2 origin cần địa chỉ ổn định qua chu kỳ stop/start thông thường, trong khi CloudFront yêu cầu origin hostname có thể phân giải thay vì raw IP trong trường origin domain.

**Hệ quả:** EIP vẫn hữu ích để direct test origin nhưng người dùng nên vào qua CloudFront. Hop CloudFront-to-origin hiện vẫn là HTTP và một EC2 vẫn là single point of failure.

### ADR-003 — Nginx phục vụ frontend và reverse proxy

**Trạng thái:** Chấp nhận.

**Quyết định:** Build Vite bằng Node.js, đưa `dist` vào Nginx Alpine và chuyển API/health/upload đến `backend:5000`.

**Lý do:** Nginx phục vụ static files hiệu quả, hỗ trợ SPA fallback và giúp frontend/API dùng cùng origin.

**Hệ quả:** Cấu hình proxy là một phần của release và phải test khi route API thay đổi.

### ADR-004 — Amazon RDS for PostgreSQL

**Trạng thái:** Đã triển khai.

**Quyết định:** PostgreSQL là runtime database duy nhất, backend kết nối bằng `pg` pool.

**Lý do:** RDS cung cấp database được quản lý, backup, metric và khả năng kiểm soát dữ liệu quan hệ. Repository có kế hoạch migration từ MongoDB và số liệu đối chiếu.

**Hệ quả:** Phải quản lý connection limit, schema migration, backup, TLS và network của RDS.

### ADR-005 — Cognito cho xác thực người dùng

**Trạng thái:** Đã triển khai.

**Quyết định:** Cognito User Pool xử lý đăng ký, xác minh, đăng nhập và đặt lại mật khẩu.

**Lý do:** Không cần lưu password ứng dụng trong PostgreSQL và tận dụng identity service được quản lý.

**Hệ quả:** User Pool/App Client, token validation và service availability trở thành phụ thuộc runtime.

### ADR-006 — S3 lưu ảnh ứng dụng

**Trạng thái:** Đã triển khai cho upload/delete từ backend.

**Quyết định:** Ảnh được xử lý bằng Sharp và lưu theo prefix trong S3.

**Lý do:** Object storage phù hợp hơn filesystem EC2 cho media bền vững và tách vòng đời ảnh khỏi container.

**Hệ quả:** Cần chọn rõ cách trình duyệt đọc object private; có thể dùng signed URL hoặc CloudFront OAC ở giai đoạn sau.

### ADR-007 — Secrets Manager cho cấu hình nhạy cảm

**Trạng thái:** Đã triển khai.

**Quyết định:** Nạp database, SMTP, Cognito client, JWT và session secret theo secret ID rồi cache trong tiến trình.

**Lý do:** Secret không nằm trong Git, có thể kiểm soát bằng IAM và audit.

**Hệ quả:** Backend production phụ thuộc Secrets Manager và EC2 role; secret rotation cần xem xét cơ chế refresh cache.

### ADR-008 — Bedrock cho tư vấn AI

**Trạng thái:** Đã triển khai.

**Quyết định:** Gửi product context và kiến thức barista tới Bedrock; mặc định `apac.amazon.nova-lite-v1:0`.

**Lý do:** Cung cấp tư vấn song ngữ mà không tự host mô hình AI trên EC2.

**Hệ quả:** Có độ trễ/chi phí sử dụng; phải kiểm tra quyền model và xử lý output JSON lỗi.

### ADR-009 — CloudWatch Logs cho log backend

**Trạng thái:** Đã triển khai.

**Quyết định:** Docker `awslogs` gửi log tới `/balancoffee/backend`.

**Lý do:** Log tập trung hỗ trợ tìm kiếm, dashboard và alarm, không phụ thuộc phiên SSH.

**Hệ quả:** IAM/region sai có thể làm log driver hoặc container lỗi; cần đặt retention để kiểm soát chi phí.

### ADR-010 — CloudFront làm viewer endpoint; hoãn Amazon SES

**Trạng thái:** CloudFront đã triển khai; SES hoãn.

**Quyết định:** Công bố ứng dụng tại `https://d3pn12mzrv3aqy.cloudfront.net` và chuyển request tới EC2 origin. Tiếp tục dùng Nodemailer/SMTP hiện có, không mô tả SES là đã triển khai.

**Lý do:** Kiểm thử endpoint và backend log xác nhận CloudFront nằm trong luồng active, cung cấp HTTPS cho viewer và cache static. Email service vẫn dùng Nodemailer/SMTP, không dùng AWS SES SDK.

**Hệ quả:** Behavior API/health phải tắt cache; chuyển tiếp cookie/header/query string cần thiết; backend CORS phải khớp CloudFront URL. Origin chưa có TLS. SES vẫn là lựa chọn tương lai.

### Lộ trình nâng cấp

| Ưu tiên | Nâng cấp | Kết quả |
|---:|---|---|
| 1 | Custom domain, WAF và TLS từ CloudFront tới ALB/Nginx origin | Mã hóa đầu cuối và tăng bảo vệ edge. |
| 2 | RDS private subnet và rule chặt chẽ | Giảm bề mặt truy cập database. |
| 3 | Nhiều instance/task và health-based routing | Loại single point of failure. |
| 4 | CI/CD và Infrastructure as Code | Deployment lặp lại, có review. |
| 5 | Signed URL hoặc CloudFront OAC cho S3 | Phân phối media private an toàn. |
| 6 | WAF, thử restore backup và incident runbook | Tăng khả năng bảo mật/phục hồi. |
