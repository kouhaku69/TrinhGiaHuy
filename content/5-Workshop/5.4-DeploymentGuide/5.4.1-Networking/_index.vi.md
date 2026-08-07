---
title: "Cấu hình VPC và Security Group"
date: "2026-08-07"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b>"
---

# Tạo VPC, subnet, route table và security group

## 1. Mục tiêu

Tạo một public subnet cho EC2 và hai private subnet ở hai Availability Zone cho DB subnet group của RDS.

## 2. Kế hoạch địa chỉ đề xuất

| Tài nguyên | Tên đề xuất | CIDR/AZ |
|---|---|---|
| VPC | balan-coffee-vpc | 10.0.0.0/16 |
| Public subnet A | balan-public-a | 10.0.1.0/24, ap-southeast-1a |
| Private DB subnet A | balan-db-private-a | 10.0.11.0/24, ap-southeast-1a |
| Private DB subnet B | balan-db-private-b | 10.0.12.0/24, ap-southeast-1b |
| Internet Gateway | balan-coffee-igw | Gắn với VPC |

Đây là tên/CIDR đề xuất cho bài lab. Nếu tài nguyên hiện tại dùng giá trị khác, giữ nguyên giá trị đang vận hành và ghi lại trong báo cáo.

## 3. Tạo VPC và subnet

1. Mở **VPC → Your VPCs → Create VPC**.
2. Chọn **VPC only**, nhập tên và IPv4 CIDR theo bảng.
3. Bật **DNS resolution** và **DNS hostnames**.
4. Vào **Subnets → Create subnet**, tạo lần lượt ba subnet.
5. Với public subnet, chọn **Edit subnet settings** và bật **Auto-assign public IPv4 address** nếu cần.

## 4. Internet Gateway và route table

1. Vào **Internet gateways → Create internet gateway**.
2. Tạo balan-coffee-igw, sau đó chọn **Attach to a VPC**.
3. Tạo route table balan-public-rt, associate với balan-public-a.
4. Trong **Routes → Edit routes**, thêm:

| Destination | Target |
|---|---|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | balan-coffee-igw |

Hai private DB subnet không cần route Internet cho luồng EC2 → RDS.

## 5. Tạo Security Group

### EC2 Security Group: balan-coffee-ec2-sg

| Type | Port | Source | Ghi chú |
|---|---:|---|---|
| HTTP | 80 | 0.0.0.0/0 trong giai đoạn kiểm thử | Cho phép EIP và CloudFront gọi origin |
| SSH | 22 | YOUR_PUBLIC_IP/32 | Chỉ khi không dùng Session Manager |

Không tạo inbound rule cho port 5000; Nginx gọi backend qua Docker network.

Sau khi không còn cần truy cập trực tiếp Elastic IP, thay source của HTTP 80 bằng AWS-managed prefix list com.amazonaws.global.cloudfront.origin-facing để chỉ CloudFront tới origin.

### RDS Security Group: balan-coffee-rds-sg

| Type | Port | Source |
|---|---:|---|
| PostgreSQL | 5432 | Security Group balan-coffee-ec2-sg |

Không dùng 0.0.0.0/0 cho PostgreSQL.

## 6. Kiểm tra

+ Public subnet có route 0.0.0.0/0 tới IGW.
+ EC2 SG không mở port 5000.
+ RDS SG chỉ nhận port 5432 từ EC2 SG.
+ Hai private DB subnet thuộc hai AZ khác nhau.

**Ảnh cần chụp:** VPC resource map, public route table, inbound rules của hai Security Group.

