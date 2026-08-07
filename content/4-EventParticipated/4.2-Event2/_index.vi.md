---
title: "Sự kiện 2"
date: "2026-06-06"
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Báo Cáo Tóm Tắt: "First Cloud Journey Meetup - Cloud, DevOps, AI và Career Development"

### Mục tiêu sự kiện

- Chia sẻ kiến thức thực tiễn về cloud, DevOps, cybersecurity, AI và phát triển phần mềm
- Kết nối các khái niệm nền tảng với kiến trúc và use case triển khai trên AWS
- Giúp người tham dự hiểu thêm về teamwork, self-learning và định hướng nghề nghiệp trong ngành IT
- Tạo không gian giao lưu, trao đổi kinh nghiệm giữa diễn giả và người tham dự

### Nội dung nổi bật theo từng phiên

#### Phiên 1 | Docker: A Containerization Technology

**Nội dung chính:**

- Giới thiệu virtualization và những lợi ích như isolation, encapsulation và portability
- Phân tích hạn chế của Virtual Machine: mỗi VM có hệ điều hành riêng, tiêu tốn nhiều CPU, memory và storage, cần cập nhật độc lập và tương đối nặng đối với ứng dụng nhỏ
- Giải thích containerization là cách đóng gói ứng dụng cùng dependencies và configurations để chạy nhất quán trên nhiều môi trường
- So sánh Virtual Machine với Container về kiến trúc, tốc độ khởi động, mức sử dụng tài nguyên và khả năng triển khai
- Giới thiệu Docker theo nguyên tắc “build once, run anywhere”
- Làm rõ Docker Image, Docker Container, Dockerfile, image layer và build cache
- Tổng quan các nhóm Docker commands và demo thao tác thực tế
- Trình bày các use case: CI/CD, microservices, môi trường development/testing, cloud-native applications và modernizing legacy applications

**Điểm rút ra:**

Em hiểu rõ hơn sự khác biệt giữa virtualization và containerization. Container không thay thế VM trong mọi trường hợp, nhưng tạo ra một đơn vị triển khai nhẹ, nhất quán và có tính di động cao. Dockerfile còn giúp biến cấu hình môi trường thành mã có thể version control, tái tạo và tích hợp vào CI/CD pipeline.

#### Phiên 2 | Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS

**Nội dung chính:**

- Tổng quan AWS WAF và khả năng bảo vệ CloudFront, Application Load Balancer, API Gateway và Cognito trước SQL Injection, XSS, bot traffic, brute force và các HTTP/HTTPS requests bất thường
- Phân tích giới hạn của cơ chế rule-based hoặc signature-based khi đối mặt với zero-day attacks, hybrid attacks và hành vi chưa từng xuất hiện
- Giới thiệu Network Intrusion Detection System (NIDS) và khả năng giám sát traffic, phân tích hành vi, phát hiện, cảnh báo và lưu lại sự kiện
- Giải thích vai trò của Machine Learning trong việc học từ network behavior và nhận diện mẫu tấn công mới
- Sử dụng bộ dữ liệu CSE-CIC-IDS2018 với các nhãn benign, bot, DoS, brute force, XSS, SQL Injection, FTP/SSH brute force và DDoS
- Thực hiện quy trình data preprocessing: hợp nhất CSV, làm sạch label, xử lý negative values, NaN và infinity, loại bỏ cột không cần thiết, cân bằng class và chia train/test set
- Đánh giá mô hình LightGBM bằng confusion matrix và cải thiện khả năng phát hiện minority attack classes
- Thiết kế kiến trúc AWS gồm Amazon EC2, Application Load Balancer, AWS WAF, Amazon S3, Kinesis Data Firehose, AWS Lambda, Security Hub, GuardDuty, Inspector, SNS, IAM, AWS Config và CloudWatch
- Xây dựng dashboard theo dõi tấn công theo thời gian thực và tương quan kết quả dự đoán NIDS với các sự kiện AWS WAF
- Định hướng cải tiến: tiếp nhận real-world data streams, tích hợp Amazon Bedrock và tự động hóa incident response

**Điểm rút ra:**

Em nhận thấy AWS WAF và ML-based NIDS nên được xem là hai lớp bảo vệ bổ trợ. WAF xử lý tốt các mẫu tấn công đã biết, trong khi Machine Learning tăng khả năng phát hiện hành vi bất thường. Hiệu quả của hệ thống ML phụ thuộc mạnh vào data quality, class balance, real-time monitoring và việc cập nhật mô hình liên tục.

#### Phiên 3 | Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets

**Nội dung chính:**

- So sánh UDP/ENet, WebSocket và HTTP Polling theo độ trễ, độ tin cậy, chi phí kết nối và loại game phù hợp
- Lựa chọn WebSocket cho turn-based games, lobby và chat nhờ full-duplex communication và reliable delivery
- Xây dựng kiến trúc Godot Client → Amazon API Gateway WebSocket → AWS Lambda → Amazon DynamoDB, kết hợp Amazon CloudWatch để ghi log và monitoring
- Sử dụng `$connect`, `$disconnect`, `$default` và custom routes với route selection expression `$request.body.action`
- Thiết kế DynamoDB item với `connectionId` làm partition key cùng các thuộc tính `status`, `opponentId`, `choice` và `createdAt`
- Xử lý logic kết nối, ngắt kết nối, tìm đối thủ, ghép cặp, nhận lựa chọn và trả kết quả trong Lambda
- Tích hợp Godot 4 bằng `WebSocketPeer`, `connect_to_url()`, `poll()`, `send_text()` và vòng lặp nhận packet
- Demo hai clients kết nối, tạo records trong DynamoDB, matchmaking, gửi lựa chọn Rock/Paper/Scissors và nhận kết quả đồng thời
- Phân tích các thách thức: stale connections và `GoneException`, chi phí của DynamoDB Scan, cùng đặc tính stateless của Lambda
- So sánh mô hình serverless WebSocket + Lambda với dedicated server trên AWS GameLift cho các game cần high-frequency updates và authoritative in-memory state

**Điểm rút ra:**

Em hiểu rằng kiến trúc multiplayer phải bắt đầu từ yêu cầu gameplay. WebSocket serverless phù hợp với game theo lượt và lobby, nhưng không tối ưu cho việc đồng bộ vật lý liên tục như FPS hoặc racing game. Ngoài chức năng kết nối, hệ thống production cần quản lý connection lifecycle, tối ưu truy vấn DynamoDB, xử lý failure và kiểm soát chi phí ở quy mô lớn.

#### Phiên 4 | The Art of Effective Teamwork

**Nội dung chính:**

- Phân biệt hiệu suất làm việc cá nhân với hiệu suất của cả team
- Trình bày 4 nguyên tắc cốt lõi của teamwork hiệu quả:
  1. Clear & Shared Goals - mục tiêu phải rõ ràng và được cả team cùng hiểu
  2. Right Person, Right Place - phân công phù hợp với năng lực và thế mạnh
  3. Open Communication & Active Listening - giao tiếp cởi mở và lắng nghe chủ động
  4. Personal Accountability - mỗi thành viên chịu trách nhiệm với phần việc và cam kết của mình
- Giới thiệu các công cụ hỗ trợ cộng tác và quản lý công việc như ClickUp, Trello, Slack, Google Workspace và Discord
- Minh họa cách Discord có thể tổ chức channel, trao đổi nội dung và hỗ trợ phối hợp trong team

**Điểm rút ra:**

Em nhận thấy công cụ chỉ hỗ trợ quy trình; nền tảng của teamwork vẫn là mục tiêu chung, phân công đúng người, giao tiếp minh bạch và trách nhiệm cá nhân. Một team hiệu quả cần thống nhất cách sử dụng công cụ, quy tắc trao đổi và tiêu chuẩn hoàn thành công việc thay vì chỉ tạo thêm nhiều channels hoặc tasks.

#### Phiên 5 | AWS Neptune for Building a Graph Knowledge Base for GraphRAG

**Nội dung chính:**

- Ôn lại Retrieval-Augmented Generation (RAG): truy xuất các passages liên quan từ knowledge base và đưa chúng vào prompt để LLM tạo câu trả lời dựa trên ngữ cảnh
- Phân tích giới hạn của vector RAG đối với câu hỏi cần multi-hop reasoning qua nhiều entities và documents
- Giới thiệu GraphRAG với hai ưu điểm chính: graph traversal cho multi-hop reasoning và lưu trữ rõ ràng relationships bằng edges
- Trình bày fully managed route với Amazon Bedrock Knowledge Bases và Amazon Neptune Analytics
- Amazon Bedrock Knowledge Bases đảm nhiệm chunking, entity extraction và embeddings generation; Neptune Analytics lưu graph data dưới dạng nodes/edges và hỗ trợ relationship discovery
- Trình bày custom route với LlamaIndex để chuẩn bị dữ liệu và xây dựng knowledge graph, kết hợp Amazon Neptune để lưu graph, thực hiện multi-hop traversal và Cypher queries
- So sánh managed GraphRAG và open-source GraphRAG theo mức độ vận hành, khả năng tùy biến, tốc độ triển khai và quyền kiểm soát pipeline

**Điểm rút ra:**

Em hiểu GraphRAG đặc biệt hữu ích khi câu hỏi phụ thuộc vào mối quan hệ giữa nhiều thực thể thay vì chỉ tìm đoạn văn tương đồng. Fully managed route phù hợp khi ưu tiên scalability và giảm operational complexity, còn custom route phù hợp khi cần kiểm soát sâu cách xây dựng graph, retrieval logic và query behavior.

#### Phiên 6 | From IT Helpdesk to Senior Sysadmin: Self-learning Journey and Transition to Cloud/DevOps

**Nội dung chính:**

- Chia sẻ hành trình nghề nghiệp thực tế bắt đầu từ IT Helpdesk, không có lợi thế đặc biệt nhưng phát triển bằng self-learning và hands-on experience
- Các kỹ năng nền tảng từ Helpdesk: troubleshooting dưới áp lực, giao tiếp với end users, problem-solving mindset và hiểu cách hệ thống IT vận hành
- Bước chuyển sang Sysadmin thông qua việc học Linux, Networking, xây dựng lab và tiếp cận infrastructure technologies
- Công việc của System Administrator: server provisioning, network management, security patching, capacity planning và monitoring
- Các bài học vận hành: tự động hóa tác vụ lặp lại, xây dựng tài liệu và runbook, thiết lập monitoring trước sự cố, phối hợp tốt với development team và không thử nghiệm thiếu kiểm soát trên production
- Chuyển đổi từ on-premises sang cloud mindset với AWS, elastic scaling, pay-as-you-go và managed services
- Tiếp cận Infrastructure as Code bằng Terraform, version control và repeatable deployments; mở rộng sang CI/CD, Docker, automation và DevOps culture
- Lộ trình DevOps hiện đại gồm Linux & Networking, Git, cloud fundamentals, Docker & Containers, CI/CD, Terraform, Kubernetes và Monitoring & Observability
- Chia sẻ kinh nghiệm phỏng vấn: nghiên cứu doanh nghiệp, tập trung vào dự án thực tế, architecture design, incident response và troubleshooting
- Lời khuyên nghề nghiệp: không học quá nhiều thứ cùng lúc, đi sâu vào 1-2 năng lực cốt lõi, thực hành qua dự án thật và xây dựng portfolio

**Điểm rút ra:**

Em hiểu rằng xuất phát điểm không quyết định giới hạn nghề nghiệp. Năng lực vận hành thực tế, tư duy xử lý sự cố, documentation, automation và portfolio có thể tạo ra bước chuyển từ Helpdesk sang Sysadmin, Cloud và DevOps. Lộ trình hiệu quả cần có thứ tự ưu tiên rõ ràng và được củng cố bằng các dự án hands-on thay vì chỉ tích lũy chứng chỉ.

### Bài học rút ra chính

- **Lựa chọn kiến trúc theo bài toán:** Container, WebSocket, GameLift, vector RAG hay GraphRAG đều có phạm vi phù hợp riêng; không có một công nghệ tối ưu cho mọi use case.
- **Automation và reproducibility là nền tảng:** Dockerfile, Infrastructure as Code, CI/CD và serverless workflows giúp giảm thao tác thủ công và tăng tính nhất quán.
- **Security cần nhiều lớp:** Rule-based protection nên được kết hợp với behavioral detection, monitoring và automated response.
- **Dữ liệu quyết định chất lượng AI/ML:** Data cleaning, class balancing và model updates ảnh hưởng trực tiếp đến hiệu quả phát hiện tấn công.
- **Vận hành phải được thiết kế từ đầu:** Logging, monitoring, connection lifecycle, failure handling, documentation và cost control không nên để đến giai đoạn cuối.
- **Kỹ năng con người vẫn mang tính quyết định:** Mục tiêu chung, giao tiếp, trách nhiệm cá nhân, self-learning và hands-on experience là nền tảng cho team và sự nghiệp bền vững.

### Áp dụng vào học tập và công việc

- Containerize một ứng dụng nhỏ bằng Dockerfile và tích hợp build image vào CI/CD pipeline
- Thiết kế proof of concept dùng API Gateway WebSocket, Lambda và DynamoDB cho ứng dụng real-time theo lượt
- Tìm hiểu cách kết hợp WAF logs, network data và ML model để xây dựng dashboard cảnh báo
- So sánh vector RAG và GraphRAG trên cùng một bộ tài liệu có nhiều mối quan hệ giữa các thực thể
- Áp dụng 4 nguyên tắc teamwork vào việc phân công, communication và review tiến độ của nhóm
- Xây dựng lộ trình Cloud/DevOps theo từng giai đoạn, ưu tiên Linux, Networking, Git, Docker, AWS, IaC, CI/CD và monitoring

### Đóng góp cá nhân

Trong vai trò người tham dự, em chủ động hệ thống hóa kiến thức của 6 phiên thành các nhóm cloud-native, security, real-time application, AI, teamwork và career development. Việc so sánh các lựa chọn kiến trúc và ghi lại bài học có thể ứng dụng giúp em chuyển nội dung trình bày thành định hướng học tập cụ thể, đồng thời tạo nền tảng để tiếp tục trao đổi kiến thức với các thành viên trong nhóm và cộng đồng.

### Trải nghiệm sự kiện

First Cloud Journey Meetup ngày 06/06/2026 mang lại góc nhìn đa chiều, từ kiến thức nền tảng như Docker đến các kiến trúc chuyên sâu như ML-based NIDS và GraphRAG. Điểm giá trị nhất là sự kết nối giữa công nghệ, vận hành, con người và định hướng nghề nghiệp. Qua đó, em hiểu rằng phát triển trong lĩnh vực Cloud/DevOps không chỉ cần học dịch vụ AWS mà còn phải rèn tư duy hệ thống, khả năng tự học, kỹ năng cộng tác và năng lực giải quyết bài toán thực tế.

#### Một số hình ảnh sự kiện
![Hình ảnh sự kiện](/images/4-EventParticipated/event2.1.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event2.2.jpg)

> Nhìn chung, sự kiện giúp em kết nối kiến thức kỹ thuật với kỹ năng làm việc và lộ trình nghề nghiệp. Những nội dung được chia sẻ không chỉ mở rộng hiểu biết về AWS mà còn giúp em xác định rõ hơn các năng lực cần tiếp tục xây dựng để phát triển theo hướng Cloud/DevOps.
