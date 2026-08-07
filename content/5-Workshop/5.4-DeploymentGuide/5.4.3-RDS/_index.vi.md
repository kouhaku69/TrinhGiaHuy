---
title: "Cấu hình Amazon RDS PostgreSQL"
date: "2026-08-07"
weight: 3
chapter: false
pre: "<b> 5.4.3. </b>"
---

# Tạo Amazon RDS for PostgreSQL

## 1. Tạo DB subnet group

1. Mở **RDS → Subnet groups → Create DB subnet group**.
2. Name: balan-coffee-db-subnet-group.
3. Chọn balan-coffee-vpc.
4. Thêm balan-db-private-a và balan-db-private-b.

RDS DB subnet group cần subnet thuộc ít nhất hai Availability Zone.

## 2. Tạo database

Vào **RDS → Databases → Create database**:

| Cấu hình | Giá trị đề xuất |
|---|---|
| Creation method | Standard create |
| Engine | PostgreSQL |
| Template | Free tier/Dev-Test tùy tài khoản |
| DB identifier | balan-coffee-postgres-dev |
| Instance class | db.t4g.micro nếu khả dụng |
| Storage | gp3, 20 GiB, storage autoscaling bật |
| Multi-AZ | No cho workshop; Yes cho production |
| VPC | balan-coffee-vpc |
| DB subnet group | balan-coffee-db-subnet-group |
| Public access | **No** |
| Security Group | balan-coffee-rds-sg |
| Port | 5432 |
| Initial database name | balancoffee |
| Encryption | Enabled |
| Automated backup | 7 ngày đề xuất |
| Deletion protection | Bật khi workshop đang sử dụng |

Không ghi master password vào tài liệu.

## 3. Kiểm tra kết nối từ EC2

Sao chép RDS endpoint trong tab **Connectivity & security**, sau đó kiểm tra từ EC2:

~~~bash
nc -zv <RDS_ENDPOINT> 5432
psql "postgresql://<USER>@<RDS_ENDPOINT>:5432/balancoffee?sslmode=require"
~~~

Nếu timeout, kiểm tra VPC, route, RDS SG và source EC2 SG. Nếu authentication failed, kiểm tra database secret.

## 4. Tạo schema và migration

Repository dùng PostgreSQL qua package pg và chỉ hỗ trợ DATABASE_PROVIDER=postgres. Thực hiện schema/migration của đúng revision triển khai, sau đó xác minh:

~~~sql
SELECT current_database(), now();
~~~

Endpoint /health phải trả database: Connected và databaseProvider: postgres.

## 5. Minh chứng

+ RDS status Available, engine/version và instance class.
+ Connectivity: VPC, subnet group, Publicly accessible: No.
+ RDS Security Group rule 5432 từ EC2 Security Group.
+ Automated backups và encryption.

