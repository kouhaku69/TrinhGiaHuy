---
title: "Configure Amazon RDS for PostgreSQL"
date: "2026-08-07"
weight: 3
chapter: false
pre: "<b> 5.4.3. </b>"
---

# Create Amazon RDS for PostgreSQL

## 1. Create the DB subnet group

Create balan-coffee-db-subnet-group in balan-coffee-vpc and add balan-db-private-a and balan-db-private-b. An RDS DB subnet group requires subnets in at least two Availability Zones.

## 2. Create the database

| Setting | Recommended value |
|---|---|
| Creation method | Standard create |
| Engine | PostgreSQL |
| Template | Free tier/Dev-Test as available |
| DB identifier | balan-coffee-postgres-dev |
| Instance class | db.t4g.micro when available |
| Storage | gp3, 20 GiB, storage autoscaling enabled |
| Multi-AZ | No for workshop; Yes for production |
| VPC | balan-coffee-vpc |
| DB subnet group | balan-coffee-db-subnet-group |
| Public access | **No** |
| Security group | balan-coffee-rds-sg |
| Port | 5432 |
| Initial database | balancoffee |
| Encryption | Enabled |
| Automated backups | 7 days recommended |
| Deletion protection | Enabled while in use |

Do not include the master password in the report.

## 3. Test from EC2

~~~bash
nc -zv <RDS_ENDPOINT> 5432
psql "postgresql://<USER>@<RDS_ENDPOINT>:5432/balancoffee?sslmode=require"
~~~

A timeout points to VPC/security-group configuration; an authentication failure points to the database secret.

## 4. Schema and migration

The repository uses PostgreSQL through pg and supports only DATABASE_PROVIDER=postgres. Apply the schema/migration from the deployed revision and validate:

~~~sql
SELECT current_database(), now();
~~~

The /health endpoint must report database: Connected and databaseProvider: postgres.

## 5. Evidence

+ RDS Available status, engine/version, and instance class.
+ VPC, subnet group, and Publicly accessible: No.
+ Port 5432 rule from the EC2 security group.
+ Backup and encryption settings.

