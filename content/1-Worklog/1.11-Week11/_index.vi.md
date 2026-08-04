---
title: "Worklog Tuần 11"
date: "2026-08-03"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục Tiêu Tuần 11:

* Xây dựng Data Lake trên AWS với dữ liệu tự chuẩn bị bằng Glue DataBrew và Glue ETL.
* Có cái nhìn tổng quan về các dịch vụ Data Analytics trên AWS: ingest, transform, phân tích, trực quan hoá.
* Dựng dashboard tương tác bằng Amazon QuickSight.
* Thực hành Data Engineering Immersion Day: streaming, ETL, tự động hoá Data Lake.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Xây dựng Data Lake với dữ liệu tự chuẩn bị** <br>&emsp;+ Tải và làm sạch dataset bằng Glue DataBrew (profiling, transform) <br>&emsp;+ Nạp dữ liệu bằng Glue, chuyển định dạng sang Parquet, tạo Data Catalog mới <br>&emsp;+ Truy vấn bằng Athena (join, CTAS, view, partition) và trực quan hoá bằng QuickSight | 03/08/2026 | 03/08/2026 | <https://000070.awsstudygroup.com/> |
| 2   | - **Lab: Tổng quan dịch vụ Data Analytics trên AWS** <br>&emsp;+ Ingest dữ liệu bằng Kinesis Firehose, catalog bằng Glue Crawler <br>&emsp;+ Transform dữ liệu bằng Glue (interactive session, Glue Studio, DataBrew) và bằng EMR <br>&emsp;+ Phân tích bằng Athena và Kinesis Data Analytics, trực quan hoá bằng QuickSight, phục vụ dữ liệu qua Lambda và Redshift | 04/08/2026 | 04/08/2026 | <https://000072.awsstudygroup.com/> |
| 3   | - **Lab: Bắt đầu với Amazon QuickSight** <br>&emsp;+ Cập nhật Dataset, dựng dashboard đầu tiên (line chart, KPI, pie chart, pivot table) <br>&emsp;+ Cải thiện dashboard: định dạng, thêm biểu đồ, bảng chi tiết dữ liệu <br>&emsp;+ Thêm tính năng tương tác: filter, filter action, navigation action, và publish dashboard | 05/08/2026 | 05/08/2026 | <https://000073.awsstudygroup.com/> |
| 4   | - **Lab: Data Engineering Immersion Day** <br>&emsp;+ Phát hiện bất thường trong luồng clickstream bằng Amazon Managed Service for Apache Flink, streaming ETL với Glue, Kinesis, MSK <br>&emsp;+ Nhập dữ liệu bằng DMS, transform dữ liệu bằng Glue (data validation, xử lý incremental với Hudi) <br>&emsp;+ Truy vấn/trực quan hoá bằng Athena, QuickSight, Athena Federated Query; tự động hoá Data Lake bằng Lake Formation | 06/08/2026 | 06/08/2026 | <https://000105.awsstudygroup.com/> |
| 5   | - **Thực hành & Ôn tập:** <br>&emsp;+ Ôn lại quy trình làm sạch và nạp dữ liệu tự chuẩn bị bằng Glue DataBrew/Glue ETL <br>&emsp;+ Ôn lại các dịch vụ trong luồng phân tích dữ liệu: Kinesis, Glue, EMR, Athena, Redshift <br>&emsp;+ Thực hành lại dựng dashboard bằng QuickSight và các bước trong Data Engineering Immersion Day | 07/08/2026 | 07/08/2026 | <https://000070.awsstudygroup.com/>, <https://000072.awsstudygroup.com/>, <https://000073.awsstudygroup.com/>, <https://000105.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 11**

**1. Xây dựng Data Lake với dữ liệu tự chuẩn bị**

* Làm sạch và chuẩn hoá dataset bằng Glue DataBrew, chuyển sang định dạng Parquet
* Truy vấn dữ liệu bằng Athena (join, CTAS, view, partition), trực quan hoá bằng QuickSight

**2. Toàn cảnh dịch vụ Data Analytics**

* Nắm luồng ingest-transform-analyze-visualize với Kinesis, Glue, EMR, Athena, Redshift, Lambda

**3. Trực quan hoá và Data Engineering nâng cao**

* Xây dựng và cải thiện dashboard tương tác bằng Amazon QuickSight
* Thực hành Data Engineering Immersion Day: phát hiện bất thường clickstream, streaming ETL, Lake Formation

### Kết luận Tuần 11

Tuần 11 tiếp tục mảng Data & Analytics, đi sâu hơn vào việc tự chuẩn bị và làm sạch dữ liệu bằng Glue DataBrew thay vì dùng dữ liệu mẫu dựng sẵn như tuần trước. Lab tổng quan dịch vụ Data Analytics giúp nhìn lại toàn bộ các công cụ đã và sẽ dùng trong một pipeline phân tích dữ liệu điển hình. Lab QuickSight tập trung vào kỹ năng trực quan hoá, còn Data Engineering Immersion Day tổng hợp gần như toàn bộ các kỹ thuật đã học trong tuần (streaming, ETL, Lake Formation) vào một bài thực hành duy nhất. Ngày cuối tuần dành để ôn lại các bước quan trọng trước khi bước sang tuần cuối.
