---
title: "Worklog Tuần 12"
date: "2026-08-10"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục Tiêu Tuần 12:

* Thực hành Amazon Athena nâng cao: Athena Spark và Athena Federated Query.
* Amazon RDS PostgreSQL: nâng cấp, giám sát hiệu năng, backup/recovery, khả năng mở rộng, high availability.
* Kết nối ứng dụng tới RDS PostgreSQL bằng Secrets Manager.
* Làm quen Machine Learning với Amazon SageMaker: feature engineering, train/tune/deploy mô hình.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Amazon Athena Workshop** <br>&emsp;+ Tìm hiểu Athena là dịch vụ phân tích serverless dựa trên Trino/Presto và Spark <br>&emsp;+ Thực hành Athena Spark <br>&emsp;+ Thực hành Athena Federated Query, kết nối tới nguồn dữ liệu RDS PostgreSQL | 10/08/2026 | 10/08/2026 | <https://000106.awsstudygroup.com/> |
| 2   | - **Lab: Nền tảng Amazon RDS PostgreSQL** <br>&emsp;+ Nâng cấp minor/engine version cho RDS PostgreSQL, kiểm tra kết quả nâng cấp <br>&emsp;+ Giám sát hiệu năng bằng CloudWatch Logs/Alert, tạo tải kiểm tra và tối ưu tốc độ load test <br>&emsp;+ Cấu hình backup tự động, snapshot thủ công, Point-in-Time Restore, AWS Backup <br>&emsp;+ Tạo Read Replica để mở rộng khả năng đọc, scale vertical, migrate sang Multi-AZ DB Cluster <br>&emsp;+ Tạo Custom Parameter Group, cấu hình Multi-AZ cho high availability và test failover | 11/08/2026 | 11/08/2026 | <https://000115.awsstudygroup.com/> |
| 3   | - **Lab: Amazon RDS PostgreSQL cho Developer** <br>&emsp;+ Cấu hình môi trường, dùng AWS Secrets Manager lưu thông tin kết nối <br>&emsp;+ Viết code kết nối database đơn giản và code tự động lấy credential để kết nối <br>&emsp;+ Deploy ứng dụng quản lý người dùng (AWS FCJ User Management System) kết nối RDS PostgreSQL | 12/08/2026 | 12/08/2026 | <https://000116.awsstudygroup.com/> |
| 4   | - **Lab: Amazon SageMaker Immersion Day** <br>&emsp;+ Tạo SageMaker Studio, tải dataset mẫu, phân tích và xử lý tương quan dữ liệu bằng Data Wrangler <br>&emsp;+ Chuyển đổi dữ liệu, lưu vào Feature Store, export dữ liệu ra S3 <br>&emsp;+ Train, tune và deploy mô hình XGBoost, đánh giá hiệu suất và thử tự động tune mô hình | 13/08/2026 | 13/08/2026 | <https://000200.awsstudygroup.com/> |
| 5   | - **Thực hành & Ôn tập, tổng kết chương trình thực tập:** <br>&emsp;+ Ôn lại Athena Spark và Athena Federated Query <br>&emsp;+ Ôn lại các thao tác vận hành RDS PostgreSQL (upgrade, backup, Multi-AZ) và cách kết nối ứng dụng bằng Secrets Manager <br>&emsp;+ Ôn lại quy trình feature engineering, train/tune/deploy mô hình trên SageMaker <br>&emsp;+ Tổng kết lại toàn bộ 12 tuần thực tập, từ hạ tầng cơ bản đến các dịch vụ chuyên sâu của AWS | 14/08/2026 | 14/08/2026 | <https://000106.awsstudygroup.com/>, <https://000115.awsstudygroup.com/>, <https://000116.awsstudygroup.com/>, <https://000200.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 12**

**1. Amazon Athena nâng cao**

* Thực hành Athena Spark và Athena Federated Query kết nối tới RDS PostgreSQL

**2. Vận hành Amazon RDS PostgreSQL**

* Nâng cấp engine, giám sát hiệu năng, cấu hình backup/recovery và Point-in-Time Restore
* Mở rộng bằng Read Replica, Multi-AZ Cluster, kiểm tra failover cho high availability

**3. Kết nối ứng dụng tới RDS PostgreSQL**

* Dùng Secrets Manager lưu thông tin kết nối, deploy ứng dụng quản lý người dùng thực tế

**4. Machine Learning với Amazon SageMaker**

* Thực hành feature engineering bằng Data Wrangler, lưu trữ feature bằng Feature Store
* Train, tune và deploy mô hình XGBoost trên SageMaker, đánh giá hiệu suất mô hình

### Kết luận Tuần 12

Tuần 12 là tuần cuối cùng của chương trình thực tập, tập trung vào Amazon RDS PostgreSQL từ hai góc nhìn: vận hành (upgrade, backup, high availability) và phát triển ứng dụng (kết nối, Secrets Manager), cùng với lab Athena nâng cao mở đầu tuần. Lab SageMaker khép lại toàn bộ chương trình bằng một bài toán Machine Learning hoàn chỉnh, từ chuẩn bị dữ liệu bằng Data Wrangler đến train/tune/deploy mô hình XGBoost. Ngày cuối cùng của tuần vừa là ngày ôn tập nội dung trong tuần, vừa là dịp tổng kết lại toàn bộ 12 tuần thực tập.
