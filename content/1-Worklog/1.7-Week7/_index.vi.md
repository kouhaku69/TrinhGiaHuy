---
title: "Worklog Tuần 7"
date: "2026-07-06"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục Tiêu Tuần 7:

* Hoàn tất nhóm Performance của Optimize: CI/CD cho container trên EKS, lưu trữ hybrid (Storage Gateway, FSx), thiết kế nâng cao cho DynamoDB, điều phối workflow bằng Step Functions, đo hiệu năng storage.
* Sang nhóm Cost Optimization: Savings Plan/Reserved Instance, trực quan hoá chi phí, phân tích chi phí bằng Glue và Athena.
* Bắt đầu chuỗi Modernize - DevAx (Monolith sang Microservices): lift-and-shift ứng dụng Java monolith, CI/CD tự động, tạo microservice bằng Lambda, tách dữ liệu sang DynamoDB, kiến trúc hướng sự kiện với SQS/SNS/Kinesis.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: CI/CD cho Amazon EKS bằng AWS CodePipeline** <br>&emsp;+ Tạo IAM Role cho pipeline, chỉnh sửa aws-auth để cấp quyền cho CI/CD <br>&emsp;+ Fork repository mẫu, tạo GitHub access token <br>&emsp;+ Cài đặt CodePipeline, kích hoạt một release mới và theo dõi việc triển khai tự động lên cluster <br>- **Lab: AWS Storage Gateway** <br>&emsp;+ Tạo S3 bucket và EC2 instance chạy Storage Gateway <br>&emsp;+ Tạo Storage Gateway và File Share <br>&emsp;+ Mount File Share từ máy on-premises mô phỏng <br>- **Lab: Amazon FSx for Windows File Server** <br>&emsp;+ Tạo môi trường bằng CloudFormation, tạo file system SSD và HDD Multi-AZ <br>&emsp;+ Tạo file share mới, kiểm tra hiệu năng, bật data deduplication và shadow copies <br>&emsp;+ Quản lý user session, quota lưu trữ, mở rộng throughput và storage capacity | 06/07/2026 | 06/07/2026 | <https://000152.awsstudygroup.com/>, <https://000024.awsstudygroup.com/>, <https://000025.awsstudygroup.com/> |
| 2   | - **Lab: Thiết kế nâng cao cho Amazon DynamoDB** <br>&emsp;+ Tạo bảng và nạp dữ liệu mẫu, đo capacity unit và cơ chế partition <br>&emsp;+ Thực hành Sequential Scan và Parallel Scan <br>&emsp;+ Xây dựng Global Secondary Index (sharding, key overloading, sparse index), thử Composite Key và Adjacency List <br>&emsp;+ Kết hợp DynamoDB Streams với Lambda để đồng bộ dữ liệu sang bảng replica <br>- **Lab: Bắt đầu với AWS Step Functions** <br>&emsp;+ Triển khai 2 Lambda function mẫu, tạo state machine đầu tiên với Task state <br>&emsp;+ Thêm Choice state cho logic rẽ nhánh, Parallel state để xử lý song song <br>&emsp;+ Dùng waitForTaskToken để tạm dừng/tiếp tục workflow, xử lý lỗi bằng Retry và Catch <br>- **Lab: Storage Performance Lab** <br>&emsp;+ Đo và tối ưu throughput của S3 (prefix, sync, thao tác file nhỏ, copy) <br>&emsp;+ Đo IOPS, I/O size, sync frequency và multi-threading trên EFS <br>&emsp;+ So sánh hiệu năng giữa các storage class và performance mode của EFS | 07/07/2026 | 07/07/2026 | <https://000039.awsstudygroup.com/>, <https://000047.awsstudygroup.com/>, <https://000068.awsstudygroup.com/> |
| 3   | - **Lab: Savings Plan, Reserved Instance và Reserved DB Instance** <br>&emsp;+ Tìm hiểu các loại Savings Plan và so sánh với Reserved Instance <br>&emsp;+ Xem gợi ý Savings Plan Recommendation, mua thử một Savings Plan <br>&emsp;+ Tìm hiểu các loại Reserved Instance và Reserved DB Instance cho RDS <br>- **Lab: Trực quan hoá chi phí (Cost Visualization)** <br>&emsp;+ Xem chi phí/usage theo service và theo account <br>&emsp;+ Xem phạm vi áp dụng của Savings Plan và Reserved Instance, xem độ co giãn (elasticity) <br>&emsp;+ Tạo custom report cho EC2, phân tích chi phí bằng Cost Explorer và xem chi phí data transfer <br>- **Lab: Phân tích chi phí và hiệu năng bằng AWS Glue và Amazon Athena** <br>&emsp;+ Chuẩn bị và build database bằng Glue Crawler <br>&emsp;+ Truy vấn dữ liệu Cost & Usage Report bằng Athena <br>&emsp;+ Phân tích chi phí theo tag, phân bổ chi phí (cost allocation) và usage | 08/07/2026 | 08/07/2026 | <https://000042.awsstudygroup.com/>, <https://000034.awsstudygroup.com/>, <https://000040.awsstudygroup.com/> |
| 4   | - **Lab: Migrating the Monolith (TravelBuddy)** <br>&emsp;+ Tạo Key Pair, CloudFormation stack, kết nối instance Windows và cấu hình database <br>&emsp;+ Chạy thử ứng dụng Java monolith bằng Eclipse IDE <br>&emsp;+ Deploy ứng dụng lên Elastic Beanstalk, cập nhật ứng dụng và gọi thử API <br>- **Lab: Cấu hình CI/CD tự động cho ứng dụng (Configure app auto-release)** <br>&emsp;+ Tạo project AWS CodeStar, kết nối Eclipse IDE với CodeCommit <br>&emsp;+ Thay source code mẫu, triển khai qua CodePipeline và xác định lỗi khi triển khai <br>&emsp;+ Triển khai một Windows Service lên EC2 bằng CodeDeploy, giám sát service <br>- **Lab: Tạo một Microservice bằng Lambda** <br>&emsp;+ Tạo và test Lambda function cục bộ, sau đó upload lên AWS Lambda <br>&emsp;+ Viết function xử lý ảnh: tạo thumbnail cho file JPEG, xoá các file không phải ảnh, gắn trigger từ S3 <br>&emsp;+ Đóng gói và tự động hoá việc triển khai function bằng SAM/CloudFormation, orchestrate qua CodeStar | 09/07/2026 | 09/07/2026 | <https://000050.awsstudygroup.com/>, <https://000051.awsstudygroup.com/>, <https://000052.awsstudygroup.com/> |
| 5   | - **Lab: Tách dữ liệu và Workflow (Refactor Your Data & Workflows)** <br>&emsp;+ Tạo bảng DynamoDB mới và Global Secondary Index cho microservice tìm chuyến đi <br>&emsp;+ Orchestrate microservice bằng CodeStar, cập nhật region và IAM Policy cho API <br>&emsp;+ Xây dựng một microservice tính toán bằng AWS Step Functions gọi Lambda <br>- **Lab: Messaging và Eventing giữa các Microservice** <br>&emsp;+ So sánh các mô hình messaging: SQS pub/sub (đơn và nhiều subscriber, FIFO), SNS fan-out ra nhiều SQS <br>&emsp;+ Thực hành Kinesis publisher gửi dữ liệu tới SQS subscriber <br>&emsp;+ Triển khai message streaming bằng Kinesis, kiểm tra dữ liệu trong ElasticSearch/Kibana | 10/07/2026 | 10/07/2026 | <https://000053.awsstudygroup.com/>, <https://000054.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 7**

**1. Hoàn tất nhóm Performance (Optimize)**

* Thiết lập CI/CD cho ứng dụng chạy trên EKS bằng CodePipeline
* Triển khai lưu trữ hybrid với Storage Gateway và FSx for Windows File Server
* Áp dụng các kỹ thuật thiết kế nâng cao cho DynamoDB: sharding GSI, key overloading, sparse index, DynamoDB Streams kết hợp Lambda
* Xây dựng state machine với AWS Step Functions, xử lý rẽ nhánh, song song, tạm dừng/tiếp tục và lỗi
* Đo và so sánh hiệu năng của S3 và EFS ở nhiều cấu hình khác nhau

**2. Cost Optimization**

* So sánh và áp dụng Savings Plan, Reserved Instance, Reserved DB Instance
* Trực quan hoá chi phí và mức độ bao phủ của Savings Plan/Reserved Instance qua Cost Explorer
* Phân tích Cost & Usage Report bằng Glue và Athena, phân bổ chi phí theo tag

**3. Bắt đầu chuỗi Modernize - Monolith to Microservices**

* Lift-and-shift ứng dụng Java monolith lên Elastic Beanstalk, thiết lập CI/CD bằng CodeStar/CodePipeline/CodeDeploy
* Tạo microservice độc lập bằng Lambda, tự động hoá triển khai bằng SAM/CloudFormation
* Tách dữ liệu ứng dụng từ RDS sang DynamoDB, xây dựng workflow bằng Step Functions
* So sánh các mô hình messaging giữa microservice: SQS, SNS, Kinesis

### Kết luận Tuần 7

Tuần 7 khép lại nhóm Performance của Optimize và mở đầu chuỗi Modernize xoay quanh việc chuyển một ứng dụng monolith sang kiến trúc microservices. Phần đầu tuần vẫn là các dịch vụ storage và cost quen thuộc, trong đó lab Step Functions và thiết kế nâng cao DynamoDB đòi hỏi hiểu rõ luồng dữ liệu hơn các lab trước. Từ giữa tuần, chuỗi lab TravelBuddy cho thấy một quy trình modernize hoàn chỉnh: bắt đầu từ ứng dụng monolith chạy trên EC2/Elastic Beanstalk, thêm CI/CD, sau đó tách dần từng phần thành microservice chạy trên Lambda với dữ liệu trên DynamoDB, và cuối cùng là lựa chọn cơ chế giao tiếp giữa các microservice (SQS, SNS, Kinesis) tuỳ theo yêu cầu về thứ tự và độ trễ.
