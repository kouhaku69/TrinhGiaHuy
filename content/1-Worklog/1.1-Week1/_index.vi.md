---
title: "Worklog Tuần 1"
date: "2026-05-22"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Mục Tiêu Tuần 1:

* Kết nối và làm quen với các thành viên của First Cloud Journey.
* Hiểu về các dịch vụ AWS cơ bản, cách sử dụng console & CLI.

### Các nhiệm vụ được thực hiện trong tuần này:
| Ngày | Nhiệm vụ                                                                                                                                                                                                   | Ngày Bắt Đầu | Ngày Hoàn Thành | Tài Liệu Tham Khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - Gặp gỡ các thành viên và quản trị viên AWS <br> - Tham gia sự kiện AWS <br> - Tìm thành viên và tạo nhóm dự án | 22/05/2026 | 22/05/2026 | |
| 2   | - Module 01 <br>&emsp; + Cloud Computing là gì, và tại sao doanh nghiệp chuyển dịch lên cloud <br>&emsp; + Điều gì làm AWS khác biệt so với các nhà cung cấp khác <br>&emsp; + Cách bắt đầu hành trình Cloud một cách bài bản <br>&emsp; + Hạ tầng toàn cầu AWS (Region, Availability Zone, Edge Location) <br>&emsp; + Các công cụ quản lý dịch vụ AWS (Console, CLI, SDK) <br>&emsp; + Làm quen với chương trình AWS Free Tier hiện hành để tối ưu chi phí học tập <br>&emsp; + Thực hành và nghiên cứu bổ sung | 25/05/2026 | 25/05/2026 | <https://000001.awsstudygroup.com/> |
| 3   | - Tạo tài khoản AWS mới <br> - Kích hoạt MFA cho tài khoản Root, tránh rủi ro mất quyền kiểm soát <br> - Tạo Admin Group và Admin User riêng, hạn chế dùng tài khoản Root cho việc hàng ngày <br> - Tìm hiểu các kênh hỗ trợ xác thực tài khoản <br> - Khám phá và cấu hình AWS Management Console <br> - Ghi chú một số lưu ý để tránh phát sinh chi phí ngoài dự kiến, đồng thời tạo thử một Support Case để làm quen quy trình | 26/05/2026 | 26/05/2026 | <https://000001.awsstudygroup.com/> |
| 4   | - Tìm hiểu về AWS Budgets — công cụ theo dõi và cảnh báo chi phí trên tài khoản <br>&emsp; + Cost Budget: cảnh báo khi tổng chi phí vượt ngưỡng <br>&emsp; + Usage Budget: cảnh báo theo mức sử dụng của từng dịch vụ (VD: giờ chạy EC2) <br>&emsp; + RI Budget: theo dõi mức sử dụng Reserved Instance <br>&emsp; + Savings Plans Budget: theo dõi cam kết sử dụng dài hạn (linh hoạt hơn RI) <br>&emsp; + Thực hành tạo budget theo 5 bước: xác định mục tiêu, phân tích chi phí hiện tại, đặt ngưỡng cảnh báo, cấu hình thông báo, theo dõi và điều chỉnh định kỳ <br>&emsp; + Dọn dẹp các budget thử nghiệm | 27/05/2026 | 27/05/2026 | <https://000007.awsstudygroup.com/> |
| 5   | - Tìm hiểu về các gói AWS Support Plans và sự khác biệt giữa từng gói <br> - Truy cập AWS Support <br>&emsp; + Các loại yêu cầu hỗ trợ <br>&emsp; + Thay đổi gói hỗ trợ khi nhu cầu hệ thống thay đổi <br> - Thực hành tạo và quản lý Support Request <br>&emsp; + Tạo Support Case mẫu <br>&emsp; + Chọn đúng mức độ nghiêm trọng (severity) cho từng loại sự cố | 28/05/2026 | 28/05/2026 | <https://000009.awsstudygroup.com/> |
| 6   | - Module 02: Amazon VPC và AWS Site-to-Site VPN <br>&emsp; + Nắm các khái niệm nền: Subnet, Route Table, Internet Gateway, NAT Gateway <br>&emsp; + Phân biệt Security Group và Network ACL, làm quen VPC Resource Map <br>&emsp; + Thực hành dựng VPC từ đầu: tạo VPC, Subnet, Internet Gateway, Route Table, Security Group, và bật VPC Flow Logs <br>&emsp; + Triển khai EC2 instance trong VPC vừa tạo, kiểm tra kết nối <br>&emsp; + Thiết lập Site-to-Site VPN: tạo Virtual Private Gateway, Customer Gateway, VPN Connection, chỉnh sửa VPN Tunnel <br>&emsp; + Tìm hiểu thêm các cấu hình VPN thay thế và cách khắc phục sự cố kết nối | 29/05/2026 | 29/05/2026 | <https://000003.awsstudygroup.com/> |

### 🏆 **Thành Tựu Tuần 1**

**1. Kết Nối & Hợp Tác**

* Gặp gỡ **các thành viên và quản trị viên AWS**, làm quen văn hóa làm việc của team FCJ
* Tham gia **sự kiện AWS** đầu tiên trong chương trình
* Tìm được đồng đội và thành lập **nhóm dự án**

**2. Nền tảng AWS Cloud**

* Nắm được bản chất **Cloud Computing** và **hạ tầng toàn cầu** của AWS (Region/AZ/Edge Location)
* Làm quen chương trình **AWS Free Tier** để tối ưu chi phí trong quá trình học
* **Tạo và bảo vệ** tài khoản AWS mới bằng **MFA**, tách bạch **Admin Group/User** khỏi tài khoản Root
* Làm quen quy trình **tạo và quản lý Support Case** trên AWS Console

**3. Quản lý chi phí & Hỗ trợ kỹ thuật**

* Phân biệt và thực hành tạo 4 loại **AWS Budgets** (Cost, Usage, RI, Savings Plans)
* Nắm quy trình 5 bước thiết lập budget hiệu quả
* Hiểu cơ chế **severity level** khi tạo AWS Support Case

**4. Nền tảng mạng với Amazon VPC**

* Dựng thành công một **VPC hoàn chỉnh** (Subnet, Route Table, Internet Gateway, Security Group) và bật **VPC Flow Logs**
* Phân biệt rõ **Security Group** và **Network ACL** trong việc kiểm soát lưu lượng
* Triển khai **EC2 instance** và thiết lập **Site-to-Site VPN** kết nối on-premises với AWS

### Kết luận Tuần 1

Tuần đầu tiên chủ yếu là làm quen: từ con người, quy trình làm việc của FCJ, đến những khái niệm nền tảng nhất của AWS. Điểm hữu ích nhất là việc gắn Budgets và Support ngay từ đầu — nhờ vậy có thói quen theo dõi chi phí và biết kênh hỗ trợ để dùng ngay khi bắt đầu thực hành các dịch vụ nặng hơn ở VPC. Phần VPC và Site-to-Site VPN tuy mới chỉ ở mức làm quen nhưng đã hình dung được luồng kết nối mạng cơ bản, đây sẽ là nền tảng bắt buộc cho hầu hết các lab về sau.
