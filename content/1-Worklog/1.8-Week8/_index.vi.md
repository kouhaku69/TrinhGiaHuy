---
title: "Worklog Tuần 8"
date: "2026-07-13"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục Tiêu Tuần 8:

* Hoàn tất chuỗi DevAx (Monolith to Microservices): xác thực cho Single Page Application, tích hợp các dịch vụ AI của AWS (Polly, Rekognition, Lex).
* Toàn bộ chuỗi Serverless Book Store: xây dựng Lambda function xử lý ảnh và ghi DynamoDB, dựng front-end gọi API Gateway, triển khai bằng AWS SAM, xác thực bằng Cognito, cấu hình SSL/custom domain, xử lý đơn hàng bằng SQS/SNS, CI/CD bằng CodePipeline, giám sát bằng CloudWatch/X-Ray, và làm quen với AppSync/GraphQL.
* Bắt đầu chuỗi Document Management System: tạo bảng DynamoDB và Lambda function quản lý tài liệu, dùng Amplify để xác thực và lưu trữ file.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Xác thực cho Single Page Application** <br>&emsp;+ Tạo DynamoDB table, build và deploy thủ công một microservice serverless <br>&emsp;+ Tạo và expose API bằng API Gateway, triển khai qua CodeStar/CI-CD <br>&emsp;+ Thêm xác thực cho SPA bằng Cognito User Pool, cấu hình đăng ký/đăng nhập, đo hiệu năng ứng dụng bằng X-Ray <br>- **Lab: Trải nghiệm các dịch vụ AI của Amazon** <br>&emsp;+ Dùng Amazon Polly để chuyển văn bản thành giọng nói qua Console, CLI và Java SDK, tạo speech mark <br>&emsp;+ Dùng Amazon Rekognition để nhận diện object và khuôn mặt trong ảnh <br>&emsp;+ Xây dựng chatbot bằng Amazon Lex, gắn Lambda function xử lý hội thoại cho ứng dụng TravelBuddy <br>- **Lab: Bắt đầu với AWS Lambda cho ứng dụng Serverless Bookstore** <br>&emsp;+ Tạo Lambda function xử lý ảnh khi có sự kiện upload lên S3 <br>&emsp;+ Tạo IAM Policy cho Lambda truy cập S3, kiểm tra hoạt động của function <br>&emsp;+ Tạo DynamoDB table và ghi dữ liệu từ Lambda | 13/07/2026 | 13/07/2026 | <https://000055.awsstudygroup.com/>, <https://000056.awsstudygroup.com/>, <https://000078.awsstudygroup.com/> |
| 2   | - **Lab: Xây dựng Front-end gọi API Gateway** <br>&emsp;+ Deploy front-end, tạo DynamoDB table cho dữ liệu ứng dụng <br>&emsp;+ Viết các Lambda function ghi/liệt kê/xoá dữ liệu <br>&emsp;+ Cấu hình method và CORS trên API Gateway, kiểm tra API bằng Postman và bằng front-end <br>- **Lab: Triển khai ứng dụng Serverless bằng AWS SAM** <br>&emsp;+ Viết lại toàn bộ ứng dụng ở lab trước bằng cú pháp SAM (YAML) <br>&emsp;+ Deploy front-end, Lambda function (list/write/delete/resize ảnh) và cấu hình API Gateway (GET/POST/DELETE) qua SAM <br>&emsp;+ Kiểm tra lại API bằng Postman và front-end <br>- **Lab: Xác thực bằng Amazon Cognito cho ứng dụng Serverless** <br>&emsp;+ Tạo Cognito User Pool <br>&emsp;+ Tạo API và Lambda function yêu cầu xác thực <br>&emsp;+ Kiểm tra luồng đăng nhập/đăng ký trên front-end | 14/07/2026 | 14/07/2026 | <https://000079.awsstudygroup.com/>, <https://000080.awsstudygroup.com/>, <https://000081.awsstudygroup.com/> |
| 3   | - **Lab: Cấu hình SSL cho ứng dụng Serverless** <br>&emsp;+ Tạo Domain và Hosted Zone trên Route 53 <br>&emsp;+ Yêu cầu chứng chỉ SSL từ AWS Certificate Manager <br>&emsp;+ Tạo CloudFront distribution phục vụ ứng dụng qua HTTPS với custom domain <br>- **Lab: Xử lý đơn hàng bằng SQS và SNS** <br>&emsp;+ Tạo SQS queue và SNS topic <br>&emsp;+ Tạo DynamoDB table lưu đơn hàng và các Lambda function checkout/quản lý/xử lý/xoá đơn hàng <br>&emsp;+ Kiểm tra luồng: đặt hàng đưa vào queue, SNS thông báo cho admin, admin xử lý hoặc xoá đơn <br>- **Lab: CI/CD cho ứng dụng Serverless bằng AWS CodePipeline** <br>&emsp;+ Tạo Git repository và pipeline cho phần backend (SAM) <br>&emsp;+ Tạo Git repository và pipeline riêng cho phần front-end <br>&emsp;+ Kiểm tra việc tự động build/deploy khi push code mới | 15/07/2026 | 15/07/2026 | <https://000082.awsstudygroup.com/>, <https://000083.awsstudygroup.com/>, <https://000084.awsstudygroup.com/> |
| 4   | - **Lab: Giám sát ứng dụng Serverless bằng CloudWatch và X-Ray** <br>&emsp;+ Debug Lambda function bằng CloudWatch Logs <br>&emsp;+ Tạo custom metric và CloudWatch Alarm để cảnh báo <br>&emsp;+ Trace request xuyên suốt ứng dụng bằng AWS X-Ray <br>- **Lab: Làm quen với AWS AppSync** <br>&emsp;+ Tìm hiểu cách AppSync kết hợp với GraphQL <br>&emsp;+ Cấu hình resolver cho DynamoDB: ghi, đọc, cập nhật, xoá, scan và query dữ liệu qua GraphQL <br>&emsp;+ Tạo và truy vấn một complex object nhiều cấp | 16/07/2026 | 16/07/2026 | <https://000085.awsstudygroup.com/>, <https://000086.awsstudygroup.com/> |
| 5   | - **Lab: Xây dựng nền tảng cho Document Management System** <br>&emsp;+ Tạo DynamoDB table lưu thông tin file <br>&emsp;+ Viết Lambda function liệt kê, upload và xoá tài liệu <br>&emsp;+ Kiểm tra hoạt động của từng Lambda function <br>- **Lab: Dùng Amplify cho xác thực và lưu trữ** <br>&emsp;+ Cấu hình Amplify Authentication dựa trên Cognito <br>&emsp;+ Cấu hình Amplify Storage để upload/quản lý file trên S3 <br>&emsp;+ Thiết lập access level (private/protected/public) cho từng loại file | 17/07/2026 | 17/07/2026 | <https://000133.awsstudygroup.com/>, <https://000134.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 8**

**1. Hoàn tất chuỗi DevAx**

* Thêm xác thực Cognito cho Single Page Application, đo hiệu năng bằng X-Ray
* Tích hợp AI service (Polly, Rekognition, Lex) vào ứng dụng TravelBuddy, xây dựng chatbot hội thoại

**2. Toàn bộ chuỗi Serverless Book Store**

* Xây dựng backend bằng Lambda, S3, DynamoDB và front-end gọi qua API Gateway
* Triển khai lại toàn bộ ứng dụng bằng AWS SAM, thêm xác thực Cognito
* Cấu hình custom domain và SSL bằng Route 53, ACM, CloudFront
* Xử lý đơn hàng bằng SQS/SNS, thiết lập CI/CD riêng cho backend và front-end bằng CodePipeline
* Giám sát ứng dụng bằng CloudWatch/X-Ray, làm quen với AppSync và GraphQL

**3. Bắt đầu chuỗi Document Management System**

* Xây dựng nền tảng lưu trữ metadata tài liệu bằng DynamoDB và Lambda
* Cấu hình Amplify để xác thực người dùng và quản lý lưu trữ file

### Kết luận Tuần 8

Tuần 8 gồm hai phần: hoàn tất chuỗi DevAx với xác thực SPA và tích hợp AI service, sau đó chuyển sang chuỗi Serverless Book Store đi từ một ứng dụng Lambda/DynamoDB đơn giản đến một hệ thống đầy đủ có SAM, Cognito, custom domain/SSL, SQS/SNS, CI/CD và giám sát. Đây là chuỗi lab có tính lặp lại cao, mỗi bước xây thêm một lớp trên kiến trúc serverless cơ bản đã có từ đầu tuần, nên phần khó nhất là giữ đồng bộ giữa các thay đổi ở Lambda, API Gateway và front-end qua từng bước. Hai lab cuối tuần mở đầu chuỗi Document Management System, tái sử dụng gần như nguyên mô hình Lambda/DynamoDB nhưng chuyển sang bài toán quản lý file với Amplify.
