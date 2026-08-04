---
title: "Worklog Tuần 10"
date: "2026-07-27"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục Tiêu Tuần 10:

* Hoàn tất mục Container: EKS Blueprints với CDK, CI/CD cho EKS bằng CodePipeline, triển khai Red Hat OpenShift Service on AWS (ROSA).
* Bắt đầu mục Data & Analytics: xây dựng Data Lake trên AWS bằng Glue, Athena và QuickSight.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Giới thiệu EKS Blueprints** <br>&emsp;+ Tạo VPC, EC2, IAM Role, cài công cụ cần thiết <br>&emsp;+ Tạo EKS Blueprints và CDK Project, dựng pipeline triển khai cluster <br>&emsp;+ Cấu hình team truy cập cluster theo IaC, cài add-on (Cluster Autoscaler), triển khai workload bằng ArgoCD | 27/07/2026 | 27/07/2026 | <https://000065.awsstudygroup.com/> |
| 2   | - **Lab: CI/CD cho EKS với CodePipeline và GitHub** <br>&emsp;+ Tạo Cloud9 workspace, cài công cụ Kubernetes, cấu hình IAM Role, tạo EKS Cluster bằng eksctl <br>&emsp;+ Deploy ứng dụng mẫu lên cluster <br>&emsp;+ Tạo CodePipeline (S3, service role cho CodePipeline/CodeBuild, cấu hình RBAC), fork source code và kiểm tra CI/CD | 28/07/2026 | 28/07/2026 | <https://000062.awsstudygroup.com/> |
| 3   | - **Lab: Red Hat OpenShift Service on AWS (ROSA)** <br>&emsp;+ Bật ROSA, tạo access key, cài đặt và xác thực ROSA <br>&emsp;+ Tạo OpenShift cluster trên AWS, deploy ứng dụng lên cluster <br>&emsp;+ Thiết lập CI/CD cơ bản cho ROSA bằng CodeCommit, CodeBuild, CodePipeline | 29/07/2026 | 29/07/2026 | <https://000071.awsstudygroup.com/> |
| 4   | - **Lab: Data Lake trên AWS** <br>&emsp;+ Tạo IAM Role/Policy, tạo S3 bucket và Kinesis Delivery Stream để nạp dữ liệu mẫu <br>&emsp;+ Tạo Data Catalog bằng Glue Crawler, kiểm tra dữ liệu đã nạp <br>&emsp;+ Phân tích bằng Athena và trực quan hoá bằng QuickSight | 30/07/2026 | 30/07/2026 | <https://000035.awsstudygroup.com/> |
| 5   | - **Thực hành & Ôn tập:** <br>&emsp;+ Ôn lại kiến trúc EKS Blueprints, cách quản lý team/add-on theo IaC và deploy workload bằng ArgoCD <br>&emsp;+ Thực hành lại quy trình CI/CD cho EKS bằng CodePipeline và cụm ROSA <br>&emsp;+ Ôn lại các bước dựng Data Lake cơ bản trên AWS (ingest, catalog, query, visualize) | 31/07/2026 | 31/07/2026 | <https://000065.awsstudygroup.com/>, <https://000062.awsstudygroup.com/>, <https://000071.awsstudygroup.com/>, <https://000035.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 10**

**1. Hoàn tất mục Container**

* Triển khai EKS Cluster theo mô hình Blueprint bằng CDK, quản lý team và add-on theo Infrastructure as Code, deploy workload bằng ArgoCD
* Xây dựng CI/CD cho ứng dụng chạy trên EKS bằng CodePipeline và GitHub
* Triển khai và vận hành cụm Red Hat OpenShift (ROSA) trên AWS, thiết lập CI/CD cơ bản

**2. Bắt đầu Data Lake trên AWS**

* Nạp dữ liệu mẫu bằng Kinesis Delivery Stream, tạo Data Catalog bằng Glue Crawler
* Truy vấn dữ liệu bằng Athena, trực quan hoá bằng QuickSight

### Kết luận Tuần 10

Tuần 10 hoàn tất toàn bộ mục Container với ba lab liên tiếp về EKS Blueprints, CI/CD cho EKS và ROSA — cả ba đều xoay quanh việc quản lý cluster Kubernetes theo hướng Infrastructure as Code, chỉ khác nhau ở nền tảng orchestration (EKS thuần hay OpenShift). Lab Data Lake vào cuối tuần mở đầu cho mục Data & Analytics, giới thiệu luồng cơ bản ingest-catalog-query-visualize sẽ được lặp lại và mở rộng trong các tuần tiếp theo. Ngày cuối tuần dành để ôn lại toàn bộ nội dung đã học, đảm bảo nắm chắc trước khi bước sang tuần sau.
