---
title: "Cấu hình Amazon Bedrock"
date: "2026-08-07"
weight: 7
chapter: false
pre: "<b> 5.4.7. </b>"
---

# Cấp quyền sử dụng Amazon Bedrock

## 1. Mô hình ứng dụng sử dụng

File backend/services/bedrockService.js dùng Bedrock Runtime Converse API. Giá trị mặc định:

~~~dotenv
AWS_REGION=ap-southeast-1
BEDROCK_MODEL_ID=apac.amazon.nova-lite-v1:0
~~~

## 2. Xác nhận quyền truy cập model

1. Mở **Amazon Bedrock** tại Region Singapore.
2. Vào **Model catalog** hoặc **Model access** theo giao diện hiện tại.
3. Xác nhận Amazon Nova Lite/inference profile đã khả dụng cho tài khoản và Region.
4. Chỉ yêu cầu quyền model thật sự dùng.

## 3. IAM

EC2 role cần bedrock:InvokeModel và, nếu dùng streaming, bedrock:InvokeModelWithResponseStream. Sau giai đoạn test, giới hạn Resource về đúng foundation model hoặc inference profile ARN.

## 4. Kiểm thử

1. Mở chatbot trên website.
2. Gửi một câu hỏi tư vấn cà phê bằng tiếng Việt và một câu bằng tiếng Anh.
3. Xác nhận response có reply và recommendedIds hợp lệ.
4. Kiểm tra CloudWatch Logs khi có AccessDeniedException, ValidationException hoặc throttling.

Không gửi secret, dữ liệu thanh toán hoặc thông tin cá nhân vào prompt. Cần theo dõi số request và chi phí Bedrock.

