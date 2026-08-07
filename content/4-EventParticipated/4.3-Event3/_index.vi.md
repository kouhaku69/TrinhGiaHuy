---
title: "Event 3"
date: 2026-06-13
weight: 3
chapter: false
pre: "<b> 4.3. </b>"
---

# Báo cáo tổng kết: "First Cloud Journey Meetup - Career, DevOps và AWS Architecture"

### Mục tiêu sự kiện

- Tiếp cận kinh nghiệm thực tế về nghề nghiệp và văn hóa làm việc trong doanh nghiệp công nghệ
- Hiểu rõ hơn vai trò, nền tảng kiến thức và tư duy của một DevOps Engineer
- Tìm hiểu lộ trình phát triển từ cộng đồng First Cloud Journey đến môi trường AWS Partner
- Khám phá kiến trúc dịch vụ rút gọn URL có khả năng mở rộng trên AWS

### Nội dung chính theo từng phiên

#### Phiên 1 | Câu chuyện thực tế đến văn hóa tại tập đoàn đa quốc gia

**Nội dung chính:**

- Chia sẻ công việc thực tế của Data Analytics Engineer trong doanh nghiệp: xây dựng báo cáo, thiết kế dashboard, phân tích nguyên nhân và hỗ trợ ra quyết định
- Nhấn mạnh bốn năng lực quan trọng gồm tư duy phản biện, giao tiếp, kể chuyện bằng dữ liệu và giải quyết vấn đề
- Giới thiệu lộ trình phát triển từ Follower, Learner, Problem Solver đến System Thinker và người dẫn dắt
- Trình bày quy trình tuyển dụng phổ biến tại tập đoàn đa quốc gia: sàng lọc, kiểm tra năng lực, phỏng vấn chuyên môn và đánh giá mức độ phù hợp văn hóa
- Chia sẻ các giá trị như No-Blame Post-Mortem, môi trường Caring & Inclusive và tư duy làm việc theo tiêu chuẩn toàn cầu

**Điểm rút ra:**

Em hiểu rằng năng lực phân tích không chỉ nằm ở việc tạo báo cáo mà còn ở khả năng tìm nguyên nhân, truyền đạt insight và đề xuất giải pháp. Để phát triển lâu dài, người làm công nghệ cần chuyển từ tư duy hoàn thành nhiệm vụ sang tư duy giải quyết vấn đề và tối ưu hệ thống.

#### Phiên 2 | What Does a DevOps Engineer Really Do?

**Nội dung chính:**

- Làm rõ những hiểu lầm phổ biến khi xem DevOps chỉ là CI/CD, Docker, Kubernetes, cloud hoặc xử lý sự cố production
- Giải thích phạm vi công việc DevOps thay đổi theo quy mô công ty, sản phẩm, cấu trúc nhóm và mức độ trưởng thành của hạ tầng
- Đề xuất học nền tảng trước: Linux, networking, Python hoặc Golang, Git, CI/CD và containers
- Khuyến khích xây dựng dự án nhỏ để thực hành deploy, automation, monitoring, troubleshooting và khôi phục hệ thống
- Nhấn mạnh việc hiểu bản chất thay vì sao chép câu lệnh, xác định đúng chủ sở hữu vấn đề, đặt câu hỏi “vì sao” và giao tiếp rõ ràng

**Điểm rút ra:**

Em nhận thấy DevOps không phải một danh sách công cụ cố định mà là cách tư duy hệ thống và hỗ trợ đội ngũ đưa phần mềm vào vận hành ổn định. Công cụ có thể thay đổi, nhưng kiến thức nền tảng, khả năng tự học, automation và communication vẫn là những năng lực cốt lõi.

#### Phiên 3 | From First Cloud AI Journey to AWS Partner

**Nội dung chính:**

- Chia sẻ lộ trình từ sự tò mò của sinh viên đến First Cloud Journey, workshop cộng đồng, hands-on labs và các dự án tại trường
- Nhấn mạnh vai trò của portfolio trong việc thể hiện năng lực và kết nối kiến thức với bài toán thực tế
- Giới thiệu First Cloud AI Journey Program, AWS Student Builder Group Program và AWS Community Builder Program
- Chia sẻ cơ hội tham gia sự kiện, xây dựng cộng đồng, nhận badge và phát triển khả năng lãnh đạo
- Mở rộng định hướng nghề nghiệp tại AWS Partner và tinh thần “share back” để hỗ trợ thế hệ tiếp theo

**Điểm rút ra:**

Em hiểu rằng một lộ trình cloud hiệu quả cần kết hợp học tập, thực hành, dự án, portfolio và đóng góp cộng đồng. Việc có được một công việc chỉ là bước khởi đầu; giá trị bền vững đến từ khả năng giải quyết bài toán thực tế và chia sẻ kiến thức với người khác.

#### Phiên 4 | A Scalable URL Shortening Service on AWS

**Nội dung chính:**

- Giải thích quy trình cơ bản của dịch vụ rút gọn URL và hạn chế của kiến trúc đơn giản khi lưu lượng tăng cao
- Trình bày frontend với Amazon Route 53, Amazon CloudFront, AWS WAF và AWS Amplify
- Xây dựng backend bằng Amazon ECS/AWS Fargate, Application Load Balancer, Amazon ElastiCache for Redis và Amazon DynamoDB trong kiến trúc đa Availability Zone
- Sử dụng Key Generation Service để tạo sẵn short codes và đưa vào Redis queue, giúp yêu cầu tạo URL phản hồi nhanh và giảm nguy cơ trùng mã
- Áp dụng cache-aside pattern: đọc dữ liệu từ Redis trước, chỉ truy vấn DynamoDB khi cache miss
- Nhấn mạnh separation of concerns, defense at the edge, pre-computation và tối ưu riêng cho read path và write path

**Điểm rút ra:**

Em hiểu rằng một hệ thống có khả năng mở rộng cần được thiết kế dựa trên đặc điểm lưu lượng. Việc tách luồng đọc và ghi, tạo mã trước, sử dụng cache và đẩy lớp bảo mật ra gần người dùng giúp giảm độ trễ, hạn chế bottleneck và bảo vệ hệ thống lõi tốt hơn.

### Bài học rút ra chính

- Kiến thức nền tảng và tư duy giải quyết vấn đề có giá trị lâu dài hơn việc chỉ ghi nhớ công cụ
- Dữ liệu cần được chuyển thành insight và hành động thay vì dừng lại ở báo cáo
- DevOps là sự kết hợp giữa hệ thống, automation, vận hành và giao tiếp trong nhóm
- Portfolio và hoạt động cộng đồng giúp biến quá trình học thành năng lực có thể chứng minh
- Kiến trúc cloud cần cân bằng hiệu năng, khả năng mở rộng, bảo mật, chi phí và khả năng vận hành

### Áp dụng vào học tập và công việc

- Xây dựng một dự án nhỏ có CI/CD, container, monitoring và tài liệu vận hành
- Luyện cách trình bày kết quả phân tích dữ liệu theo cấu trúc vấn đề - nguyên nhân - giải pháp
- Hoàn thiện portfolio với các bài lab AWS và dự án có kiến trúc rõ ràng
- Thử thiết kế proof of concept cho URL shortener bằng Redis và DynamoDB
- Chủ động tham gia hoạt động cộng đồng và chia sẻ lại kiến thức đã học

### Đóng góp cá nhân

Trong vai trò người tham dự, em chủ động ghi chép và hệ thống hóa nội dung của bốn phiên theo ba nhóm: phát triển nghề nghiệp, tư duy DevOps và thiết kế kiến trúc AWS. Việc liên kết các bài học với kế hoạch học tập giúp em xác định rõ hơn những kỹ năng cần ưu tiên và cách chuyển kiến thức thành dự án thực tế.

### Trải nghiệm sự kiện

Meetup ngày 13/06/2026 mang lại góc nhìn cân bằng giữa kỹ thuật và nghề nghiệp. Bên cạnh kiến thức về DevOps và kiến trúc AWS, những chia sẻ về tư duy phân tích, văn hóa doanh nghiệp, portfolio và cộng đồng giúp em hiểu rõ hơn cách phát triển năng lực một cách bền vững.

#### Một Số Hình Ảnh Sự Kiện
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.1.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.2.jpg)

> Nhìn chung, sự kiện giúp em kết nối kiến thức chuyên môn với tư duy nghề nghiệp và tinh thần đóng góp cộng đồng. Đây là nền tảng quan trọng để tiếp tục phát triển theo hướng Cloud/DevOps và xây dựng các giải pháp có khả năng ứng dụng thực tế.
