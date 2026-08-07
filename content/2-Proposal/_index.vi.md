---
title: "Bản đề xuất"
date: "2025-12-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Balan Coffee & Roastery – Hiện đại hóa nền tảng bán cà phê trực tuyến trên AWS

### **1. Tóm tắt điều hành**

Balan Coffee & Roastery là dự án hiện đại hóa hệ thống website bán cà phê hiện có bằng cách triển khai lên nền tảng AWS, nhằm cải thiện khả năng vận hành, bảo mật và khả năng quan sát hệ thống.

Ứng dụng được triển khai trên một instance **Amazon EC2** sử dụng **Docker Compose** để chạy đồng thời container frontend và backend, kết hợp **Amazon CloudFront** làm lớp phân phối nội dung cho toàn bộ yêu cầu từ người dùng.

Backend kết nối với các dịch vụ AWS được quản lý hoàn toàn (managed services) gồm **Amazon RDS (PostgreSQL)**, **Amazon S3**, **Amazon Cognito**, **Amazon SES**, **Amazon Bedrock** và **AWS Secrets Manager**, đảm bảo dữ liệu, xác thực, gửi email và tính năng AI được xử lý an toàn và tách biệt khỏi tầng ứng dụng.

Khả năng giám sát vận hành được đảm bảo thông qua **Amazon CloudWatch** kết hợp **CloudWatch Agent** cài đặt trực tiếp trên EC2.

### **2. Phát biểu vấn đề**

**Vấn đề:**

- Hệ thống Balan Coffee hiện tại được xây dựng và vận hành theo cách truyền thống, chưa tận dụng các dịch vụ quản lý của AWS, dẫn đến khó khăn trong việc mở rộng, sao lưu dữ liệu và giám sát tình trạng hệ thống.
- Thông tin nhạy cảm (khóa API, thông tin kết nối cơ sở dữ liệu) nếu được cấu hình thủ công hoặc lưu trực tiếp trong mã nguồn sẽ tiềm ẩn rủi ro bảo mật cao.
- Trải nghiệm khách hàng còn hạn chế do chưa có tính năng gợi ý sản phẩm thông minh dựa trên hành vi hoặc sở thích người dùng.

**Giải pháp:**

- Container hóa frontend và backend bằng **Docker Compose**, triển khai trên **Amazon EC2**, giúp đơn giản hóa quá trình vận hành trong khuôn khổ workshop trong khi vẫn giữ toàn quyền kiểm soát hạ tầng.
- Sử dụng **Amazon RDS (PostgreSQL)** trong private subnet để đảm bảo dữ liệu quan hệ được quản lý, sao lưu tự động và cách ly khỏi truy cập trực tiếp từ Internet.
- Tích hợp **Amazon Cognito** cho xác thực người dùng, **AWS Secrets Manager** để quản lý an toàn các thông tin cấu hình nhạy cảm, **Amazon S3** để lưu trữ tệp/hình ảnh sản phẩm, **Amazon SES** để gửi email giao dịch, và **Amazon Bedrock** để xây dựng tính năng chatbot/gợi ý sản phẩm bằng AI.
- Toàn bộ hệ thống được giám sát tập trung qua **Amazon CloudWatch**.

**Lợi ích và lợi tức đầu tư**

Giải pháp giúp Balan Coffee & Roastery chuyển đổi từ mô hình vận hành thủ công sang mô hình có cấu trúc rõ ràng trên AWS, tách biệt tầng ứng dụng, dữ liệu và bảo mật. Việc sử dụng các dịch vụ quản lý (RDS, S3, Cognito, SES, Bedrock) giúp giảm khối lượng công việc vận hành thủ công, đồng thời chi phí hạ tầng ở giai đoạn workshop chủ yếu nằm trong hạn mức miễn phí/chi phí thấp của AWS (EC2 loại nhỏ, RDS single-AZ, S3 dung lượng thấp). Đây là nền tảng có thể mở rộng dần lên kiến trúc production đầy đủ (ECS, Auto Scaling, Load Balancer) mà không cần thiết kế lại từ đầu.

### **3. Kiến trúc giải pháp**

Nền tảng Balan Coffee & Roastery được xây dựng theo mô hình triển khai container trên **Amazon EC2**, kết hợp với các dịch vụ quản lý của AWS để xử lý dữ liệu, xác thực, lưu trữ, gửi email và tính năng AI. Kiến trúc đảm bảo tách biệt rõ giữa lớp phân phối nội dung, lớp ứng dụng (public subnet) và lớp dữ liệu (private subnet).

Luồng xử lý chính: người dùng truy cập qua **HTTPS**, yêu cầu được **Amazon CloudFront** phân phối tới **Internet Gateway**, sau đó chuyển đến **Amazon EC2** đặt trong public subnet của VPC. Trên EC2, **Docker** chạy đồng thời container **Frontend** và **Backend**. Backend giao tiếp với **Amazon RDS (PostgreSQL)** đặt trong private subnet (cổng TCP 5432, được kiểm soát bởi Security Group), đồng thời gọi đến các dịch vụ **Amazon S3** (lưu trữ file/ảnh), **Amazon Bedrock** (AI gợi ý sản phẩm/chatbot), **Amazon SES** (gửi email) và **Amazon Cognito** (xác thực người dùng).

Toàn bộ hoạt động của EC2 được **CloudWatch Agent** thu thập (metrics hệ thống, log ứng dụng) và gửi về **Amazon CloudWatch** để hiển thị qua Dashboard và thiết lập cảnh báo.

Kiến trúc tổng thể được mô tả chi tiết trong sơ đồ bên dưới:

![Kiến trúc giải pháp Balan Coffee & Roastery](/images/2-Proposal/architecture.jpg)

### Dịch vụ AWS đã sử dụng

1.  **Amazon EC2:** Máy chủ tính toán, chạy ứng dụng thông qua Docker.
2.  **Docker (Docker Compose):** Đóng gói và chạy đồng thời container frontend và backend.
3.  **Amazon CloudFront:** Phân phối nội dung toàn cầu, xử lý HTTPS và cải thiện hiệu năng tải trang.
4.  **Amazon RDS (PostgreSQL):** Cơ sở dữ liệu quan hệ, lưu trữ toàn bộ dữ liệu nghiệp vụ (sản phẩm, đơn hàng, người dùng).
5.  **Amazon S3:** Lưu trữ đối tượng (hình ảnh sản phẩm, tệp tĩnh).
6.  **Amazon Cognito:** Xác thực và phân quyền người dùng.
7.  **Amazon SES:** Gửi email giao dịch/thông báo.
8.  **Amazon Bedrock:** Cung cấp năng lực AI cho chatbot và tính năng gợi ý sản phẩm.
9.  **AWS Secrets Manager:** Lưu trữ và quản lý an toàn các thông tin cấu hình nhạy cảm (biến môi trường, khóa kết nối).
10. **Amazon CloudWatch (và CloudWatch Agent):** Thu thập metrics, log và hiển thị dashboard giám sát hệ thống.
11. **Amazon VPC (Public/Private Subnet, Internet Gateway, Security Group):** Thiết kế mạng, cô lập tầng dữ liệu khỏi truy cập trực tiếp từ Internet.

### **Thiết kế thành phần**

1.  **Lớp giao diện người dùng (Frontend):**

    - Chạy dưới dạng container Docker trên EC2, phục vụ giao diện web cho khách hàng.
    - Được phân phối toàn cầu qua **Amazon CloudFront**, đảm bảo tốc độ tải nhanh và luôn sử dụng HTTPS.

2.  **Lớp xử lý nghiệp vụ (Backend):**

    - Chạy dưới dạng container Docker riêng biệt trên cùng EC2, xử lý toàn bộ logic nghiệp vụ (sản phẩm, đơn hàng, xác thực, tích hợp AI).
    - Truy cập các thông tin cấu hình nhạy cảm (kết nối RDS, API key của Bedrock/SES...) thông qua **AWS Secrets Manager**, không lưu trực tiếp trong mã nguồn.

3.  **Lớp dữ liệu (Cơ sở dữ liệu):**

    - **Amazon RDS PostgreSQL** đặt trong private subnet, chỉ cho phép kết nối từ Security Group của EC2 backend (cổng 5432), không mở ra Internet.
    - Đảm nhận lưu trữ toàn bộ dữ liệu giao dịch, sản phẩm và thông tin người dùng.

4.  **Bảo mật và xác thực:**

    - **Amazon Cognito** cung cấp cơ chế đăng ký/đăng nhập và xác thực bằng JWT cho người dùng.
    - **Security Group** kiểm soát chặt chẽ luồng truy cập giữa các thành phần (chỉ backend mới truy cập được RDS).
    - **AWS Secrets Manager** đảm bảo các thông tin nhạy cảm được luân phiên và quản lý tập trung.
    - Toàn bộ truy cập từ người dùng đều đi qua **HTTPS** nhờ CloudFront.

5.  **Giám sát và vận hành:**

    - **CloudWatch Agent** cài trên EC2 thu thập metrics hạ tầng (CPU, RAM, disk) và log ứng dụng.
    - **Amazon CloudWatch** tổng hợp dữ liệu để hiển thị Dashboard và có thể thiết lập cảnh báo khi phát hiện bất thường.

### **4. Triển khai kỹ thuật**

Dự án được triển khai theo các giai đoạn chính sau:

**Giai đoạn 1: Thiết kế kiến trúc**

- Nghiên cứu mô hình triển khai container trên EC2, lựa chọn các dịch vụ AWS phù hợp (RDS, S3, Cognito, SES, Bedrock, Secrets Manager).
- Thiết kế sơ đồ mạng VPC (public/private subnet), quy tắc Security Group.
- **Sản phẩm đầu ra:** Sơ đồ kiến trúc giải pháp (Solution Architecture).

**Giai đoạn 2: Khởi tạo hạ tầng AWS**

- Tạo VPC, subnet, Internet Gateway, Security Group.
- Khởi tạo EC2, cấu hình Docker/Docker Compose, khởi tạo RDS PostgreSQL, S3 bucket.
- **Sản phẩm đầu ra:** Hạ tầng AWS sẵn sàng cho việc triển khai ứng dụng.

**Giai đoạn 3: Tích hợp dịch vụ quản lý**

- Tích hợp Cognito cho xác thực, Secrets Manager cho cấu hình nhạy cảm, SES cho gửi email, Bedrock cho tính năng gợi ý/chatbot AI.
- **Sản phẩm đầu ra:** Backend hoàn chỉnh với đầy đủ tích hợp AWS.

**Giai đoạn 4: Triển khai và giám sát**

- Deploy frontend/backend qua Docker Compose trên EC2, cấu hình CloudFront.
- Cài đặt CloudWatch Agent, xây dựng Dashboard giám sát, kiểm thử toàn bộ luồng ứng dụng.
- **Sản phẩm đầu ra:** Hệ thống vận hành hoàn chỉnh, có giám sát và sẵn sàng demo.

**Yêu cầu kỹ thuật**

- **Hạ tầng:** VPC với public subnet (EC2, Internet Gateway) và private subnet (RDS), Security Group kiểm soát truy cập.
- **Công nghệ:** Docker/Docker Compose cho container hóa; PostgreSQL cho cơ sở dữ liệu.
- **Bảo mật:** Cognito cho xác thực (JWT), Secrets Manager cho quản lý cấu hình, HTTPS qua CloudFront.
- **Giám sát:** CloudWatch Agent + CloudWatch (Metrics, Logs, Dashboard).
- **Khu vực triển khai:** ap-southeast-1 (Singapore) để tối ưu tốc độ truy cập tại Việt Nam.

### **5. Dòng thời gian & các mốc quan trọng**

- **Giai đoạn chuẩn bị:** Nghiên cứu kiến trúc, phân chia công việc theo vai trò từng thành viên trong nhóm.
- **Giai đoạn 1:** Thiết kế kiến trúc và hạ tầng mạng (VPC, subnet, Security Group).
- **Giai đoạn 2:** Khởi tạo hạ tầng AWS (EC2, RDS, S3) và triển khai container cơ bản.
- **Giai đoạn 3:** Tích hợp các dịch vụ quản lý (Cognito, Secrets Manager, SES, Bedrock).
- **Giai đoạn 4:** Hoàn thiện giám sát (CloudWatch), kiểm thử và demo sản phẩm.

### **6. Phân công thực hiện**

| Thành viên | Phạm vi phụ trách |
|---|---|
| Trần Minh Quân | Hạ tầng AWS, triển khai (VPC, EC2, Docker), cơ sở dữ liệu (RDS) và giám sát (CloudWatch) |
| Nguyễn Võ Duy Tuân | Xác thực người dùng (Cognito) và quản lý cấu hình bí mật (Secrets Manager) |
| Phạm Nguyễn Quang Minh | Lưu trữ file (S3), tính năng AI (Bedrock) và tích hợp API backend |
| Trịnh Gia Huy | Tài liệu kỹ thuật (Documentation), thiết kế kiến trúc (Architecture) và demo sản phẩm |

### **7. Đánh giá rủi ro**

**Ma trận rủi ro**

- Sự cố hoặc gián đoạn dịch vụ AWS: Tác động trung bình, khả năng thấp.
- EC2 đơn instance gặp sự cố (không có Auto Scaling/Load Balancer): Tác động cao, khả năng trung bình.
- Cấu hình Security Group sai dẫn đến lộ cổng RDS: Tác động cao, khả năng thấp.
- Vượt hạn mức sử dụng miễn phí của Bedrock/SES: Tác động trung bình, khả năng trung bình.
- Mất dữ liệu do thiếu sao lưu định kỳ: Tác động cao, khả năng thấp.

**Chiến lược giảm thiểu**

- Thiết lập cảnh báo qua CloudWatch khi tài nguyên EC2/RDS vượt ngưỡng.
- Rà soát định kỳ cấu hình Security Group, chỉ mở cổng cần thiết.
- Bật sao lưu tự động (automated backup) cho RDS.
- Theo dõi mức sử dụng Bedrock/SES qua CloudWatch để tránh vượt hạn mức.

**Kế hoạch dự phòng**

- Lưu trữ cấu hình hạ tầng dưới dạng tài liệu để có thể dựng lại nhanh khi cần.
- Có thể mở rộng sang ECS/Auto Scaling khi cần đảm bảo tính sẵn sàng cao hơn (tham khảo phần Kết quả mong đợi).

### **8. Kết quả mong đợi**

**Cải tiến kỹ thuật**

- **Hiện đại hóa hạ tầng:** Chuyển từ vận hành thủ công sang mô hình container hóa trên AWS với các dịch vụ quản lý (RDS, S3, Cognito, SES, Bedrock).
- **Bảo mật tốt hơn:** Cách ly cơ sở dữ liệu trong private subnet, quản lý bí mật tập trung qua Secrets Manager, xác thực qua Cognito.
- **Giám sát tập trung:** Toàn bộ hệ thống được theo dõi qua CloudWatch, phát hiện sớm sự cố.
- **Trải nghiệm khách hàng tốt hơn:** Tích hợp tính năng gợi ý sản phẩm/chatbot AI qua Amazon Bedrock.

**Giá trị dài hạn**

- **Nền tảng có thể mở rộng:** Kiến trúc hiện tại là bước đệm để mở rộng lên **Amazon ECS, Application Load Balancer, Auto Scaling Group, Route 53, AWS WAF, AWS Certificate Manager** và **CI/CD Pipeline** trong tương lai, theo đúng định hướng đã nêu trong tài liệu kiến trúc giải pháp.
- **Tài sản kỹ thuật:** Tài liệu kiến trúc và cấu hình hạ tầng có thể tái sử dụng làm nền tảng cho các dự án tương tự của nhóm.
