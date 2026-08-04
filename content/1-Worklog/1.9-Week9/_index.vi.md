---
title: "Worklog Tuần 9"
date: "2026-07-20"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục Tiêu Tuần 9:

* Hoàn tất chuỗi Document Management System: tích hợp front-end với API Gateway, triển khai bằng AWS SAM, cấu hình CloudFront/SSL, xây dựng tính năng tìm kiếm bằng OpenSearch, CI/CD bằng CodePipeline, giám sát bằng CloudWatch/X-Ray.
* Serverless Web App Workshop: hoàn thiện một ứng dụng công viên giải trí bằng Lambda/API Gateway/SAM, xây dựng một ứng dụng chat serverless hoàn chỉnh có xác thực người dùng.
* Elastic Beanstalk: triển khai ứng dụng với 2 môi trường Dev/Production và swap URL, thiết lập CI/CD bằng CDK Pipelines.
* Kết thúc toàn bộ mục Modernize và bắt đầu mục Container: làm quen với Kubernetes/Amazon EKS, chuyển đổi ứng dụng monolith sang microservices bằng Docker, ECS và AWS Fargate, deploy WordPress bằng CodeDeploy.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Tích hợp Front-end với API Gateway cho Document Management System** <br>&emsp;+ Deploy front-end, cấu hình API Gateway <br>&emsp;+ Kiểm tra API bằng Postman và bằng front-end <br>- **Lab: Triển khai Document Management System bằng AWS SAM** <br>&emsp;+ Deploy Cognito và S3 bucket, deploy front-end bằng SAM <br>&emsp;+ Cấu hình API và Lambda function bằng SAM <br>&emsp;+ Kiểm tra API với front-end <br>- **Lab: Cấu hình CloudFront/SSL cho Document Management System** <br>&emsp;+ Tạo Domain và Hosted Zone <br>&emsp;+ Yêu cầu chứng chỉ SSL từ ACM <br>&emsp;+ Tạo CloudFront distribution phục vụ ứng dụng qua HTTPS | 20/07/2026 | 20/07/2026 | <https://000135.awsstudygroup.com/>, <https://000136.awsstudygroup.com/>, <https://000137.awsstudygroup.com/> |
| 2   | - **Lab: Xây dựng tính năng tìm kiếm bằng Amazon OpenSearch** <br>&emsp;+ Tạo Lambda function nạp dữ liệu từ DynamoDB Stream vào OpenSearch <br>&emsp;+ Tạo OpenSearch instance và API tìm kiếm <br>&emsp;+ Kiểm tra tính năng tìm kiếm tài liệu theo tên, loại file, tag <br>- **Lab: CI/CD cho Document Management System bằng CodePipeline** <br>&emsp;+ Tạo Git repository và pipeline cho backend (SAM) <br>&emsp;+ Tạo Git repository và pipeline riêng cho front-end <br>&emsp;+ Kiểm tra tự động build/deploy khi push code <br>- **Lab: Giám sát Document Management System bằng CloudWatch và X-Ray** <br>&emsp;+ Debug Lambda function bằng CloudWatch Logs <br>&emsp;+ Tạo custom metric và CloudWatch Alarm <br>&emsp;+ Trace request bằng AWS X-Ray | 21/07/2026 | 21/07/2026 | <https://000138.awsstudygroup.com/>, <https://000139.awsstudygroup.com/>, <https://000140.awsstudygroup.com/> |
| 3   | - **Lab: Serverless với Lambda, API Gateway và SAM (ứng dụng công viên giải trí)** <br>&emsp;+ Deploy front-end bằng AWS Amplify Console, deploy backend (Lambda, API Gateway, DynamoDB) <br>&emsp;+ Nạp dữ liệu mẫu vào DynamoDB, kiểm tra cấu hình <br>&emsp;+ Xây dựng tính năng thời gian chờ theo thời gian thực và xử lý ảnh chụp trong lúc chơi (Lambda xử lý ảnh, ghép ảnh) <br>- **Lab: Xây dựng ứng dụng Chat Serverless hoàn chỉnh** <br>&emsp;+ Xây dựng chat tĩnh với S3, tạo API bằng Lambda và API Gateway, cấu hình CORS <br>&emsp;+ Chuyển sang lưu trữ hội thoại bằng DynamoDB, tách API thành nhiều Lambda function riêng biệt <br>&emsp;+ Thêm xác thực người dùng bằng Cognito (đăng ký, đăng nhập, authorizer cho API Gateway), tối ưu tốc độ tải bằng CloudFront <br>- **Lab: Triển khai ứng dụng với Elastic Beanstalk** <br>&emsp;+ Tạo Key Pair và IAM instance role <br>&emsp;+ Tạo môi trường Development và Production trên Elastic Beanstalk <br>&emsp;+ Cập nhật ứng dụng ở môi trường Dev và swap URL giữa 2 môi trường | 22/07/2026 | 22/07/2026 | <https://000066.awsstudygroup.com/>, <https://000117.awsstudygroup.com/>, <https://000112.awsstudygroup.com/> |
| 4   | - **Lab: CI/CD cho Elastic Beanstalk bằng AWS CDK Pipelines** <br>&emsp;+ Tạo GitHub repository, chuẩn bị môi trường và ứng dụng web mẫu <br>&emsp;+ Định nghĩa hạ tầng và môi trường Elastic Beanstalk bằng CDK <br>&emsp;+ Tạo CDK Pipeline Stack, deploy và kiểm tra kết quả tự động triển khai <br>- **Lab: Giới thiệu Kubernetes và Amazon EKS** <br>&emsp;+ Tìm hiểu kiến trúc Kubernetes (Control Plane, Data Plane) và kiến trúc Amazon EKS <br>&emsp;+ Chuẩn bị workspace, cài công cụ Kubernetes, tạo IAM Role, khởi tạo cluster bằng eksctl <br>&emsp;+ Deploy Kubernetes Dashboard và một ứng dụng microservice mẫu lên cluster, thử scale service | 23/07/2026 | 23/07/2026 | <https://000113.awsstudygroup.com/>, <https://000126.awsstudygroup.com/> |
| 5   | - **Lab: Deploy WordPress lên EC2 bằng AWS CodeDeploy** <br>&emsp;+ Tạo access key, instance profile và service role <br>&emsp;+ Khởi chạy EC2 instance, cài đặt CodeDeploy Agent <br>&emsp;+ Tạo S3 bucket, Deployment Group, và triển khai ứng dụng WordPress <br>- **Lab: Chuyển đổi Monolith sang Microservices bằng Docker, ECS và AWS Fargate** <br>&emsp;+ Tìm hiểu khái niệm Docker và container image <br>&emsp;+ Đóng gói ứng dụng monolith mẫu (Mythical Mysfits) thành container <br>&emsp;+ Triển khai container bằng AWS Fargate, cấu hình ALB và ECS Service, tách dần thành các microservice riêng | 24/07/2026 | 24/07/2026 | <https://000091.awsstudygroup.com/>, <https://000067.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 9**

**1. Hoàn tất chuỗi Document Management System**

* Ghép front-end với backend qua API Gateway, triển khai lại toàn bộ bằng AWS SAM
* Cấu hình custom domain/SSL bằng Route 53, ACM, CloudFront
* Xây dựng tính năng tìm kiếm tài liệu bằng OpenSearch kết hợp DynamoDB Stream
* Thiết lập CI/CD riêng cho backend và front-end, giám sát ứng dụng bằng CloudWatch/X-Ray

**2. Serverless Web App Workshop**

* Hoàn thiện một ứng dụng serverless nhiều tính năng (dữ liệu tĩnh, thời gian chờ thực, xử lý ảnh) cho kịch bản công viên giải trí
* Xây dựng một ứng dụng chat serverless từ đầu: từ dữ liệu tĩnh trên S3 đến API tách microservice và xác thực Cognito

**3. Elastic Beanstalk**

* Triển khai ứng dụng với 2 môi trường Dev/Production, thực hành swap URL để chuyển đổi phiên bản
* Xây dựng pipeline CI/CD cho Elastic Beanstalk bằng AWS CDK

**4. Kết thúc mục Modernize, làm quen Kubernetes/EKS**

* Tìm hiểu kiến trúc Kubernetes và Amazon EKS, khởi tạo cluster bằng eksctl và deploy ứng dụng microservice mẫu
* Deploy WordPress lên EC2 bằng CodeDeploy, hoàn tất toàn bộ mục Modernize

**5. Bắt đầu Container Services**

* Đóng gói một ứng dụng monolith thành container bằng Docker
* Triển khai container bằng ECS và AWS Fargate, bắt đầu tách ứng dụng thành các microservice độc lập

### Kết luận Tuần 9

Tuần 9 khép lại toàn bộ mục Modernize, bắt đầu bằng việc hoàn thiện chuỗi Document Management System (front-end, SAM, CloudFront/SSL, tìm kiếm OpenSearch, CI/CD, giám sát) rồi chuyển sang hai workshop độc lập là ứng dụng công viên giải trí và ứng dụng chat serverless, cả hai đều lặp lại mô hình Lambda/API Gateway/DynamoDB nhưng ở quy mô lớn hơn và có thêm xác thực người dùng. Phần Elastic Beanstalk cho thấy một cách triển khai khác so với serverless, dùng 2 môi trường song song để giảm rủi ro khi update. Cuối tuần bắt đầu chuyển sang Container: làm quen kiến trúc Kubernetes/EKS, deploy WordPress bằng CodeDeploy, và đóng gói một ứng dụng monolith mẫu thành container để bắt đầu tách thành microservice chạy trên ECS/Fargate.
