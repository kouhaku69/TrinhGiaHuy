---
title: "Các bài blogs đã đăng"
date: "2026-07-31"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Tại đây sẽ là phần liệt kê, giới thiệu các blogs mà các bạn đã dịch. Ví dụ:

###  [Blog 1 - AWS Cost Anomaly Detection: Phát hiện hóa đơn bất thường và tìm nguyên nhân bằng Amazon Q](3.1-Blog1/)
Blog này hướng dẫn cách sử dụng AWS Cost Anomaly Detection để phát hiện các mẫu chi tiêu bất thường và tính năng AI-powered cost investigation với Amazon Q để hỗ trợ tìm root cause. Bài viết trình bày Cost Monitor, Alert Subscription, Cost Explorer, CloudTrail, cross-account investigation và các lưu ý về độ trễ billing cũng như chi phí CloudWatch Logs Insights.

###  [Blog 2 - Tự động bật/tắt EC2 bằng Amazon EventBridge Scheduler mà không cần viết Lambda](3.2-Blog2/)
Blog này hướng dẫn cách sử dụng Amazon EventBridge Scheduler để tự động bật EC2 vào đầu giờ làm việc và tắt vào cuối ngày. Bài viết tập trung vào cron schedule, timezone Asia/Ho_Chi_Minh, universal target, IAM execution role theo least privilege, retry/DLQ và cách giảm idle compute trong môi trường development/test.

### [Blog 3 - Amazon S3 Lifecycle: Tự động giảm chi phí lưu trữ mà không cần quản lý thủ công](3.3-Blog3/)
Bài viết giải thích cách sử dụng Amazon S3 Lifecycle để tự động chuyển dữ liệu cũ sang storage class phù hợp hơn hoặc xóa khi hết thời gian lưu trữ. Đây là một cách đơn giản để giảm chi phí S3 mà không phải quản lý từng file thủ công.