---
title: "Configure the VPC and security groups"
date: "2026-08-07"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b>"
---

# Create the VPC, subnets, route table, and security groups

## 1. Objective

Create one public subnet for EC2 and two private subnets in separate Availability Zones for the RDS DB subnet group.

## 2. Recommended address plan

| Resource | Recommended name | CIDR/AZ |
|---|---|---|
| VPC | balan-coffee-vpc | 10.0.0.0/16 |
| Public subnet A | balan-public-a | 10.0.1.0/24, ap-southeast-1a |
| Private DB subnet A | balan-db-private-a | 10.0.11.0/24, ap-southeast-1a |
| Private DB subnet B | balan-db-private-b | 10.0.12.0/24, ap-southeast-1b |
| Internet Gateway | balan-coffee-igw | Attached to the VPC |

These are lab recommendations. If the deployed resources use different values, retain and document the actual values.

## 3. Create the VPC and subnets

1. Open **VPC → Your VPCs → Create VPC**.
2. Choose **VPC only** and enter the name and CIDR.
3. Enable **DNS resolution** and **DNS hostnames**.
4. Open **Subnets → Create subnet** and create the three subnets.
5. Enable automatic public IPv4 assignment on the public subnet when required.

## 4. Internet Gateway and route table

1. Create balan-coffee-igw and attach it to the VPC.
2. Create balan-public-rt and associate it with balan-public-a.
3. Add the routes:

| Destination | Target |
|---|---|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | balan-coffee-igw |

The private DB subnets do not require an Internet route for EC2-to-RDS traffic.

## 5. Security groups

### balan-coffee-ec2-sg

| Type | Port | Source | Note |
|---|---:|---|---|
| HTTP | 80 | 0.0.0.0/0 during validation | Allows direct EIP and CloudFront origin tests |
| SSH | 22 | YOUR_PUBLIC_IP/32 | Only when Session Manager is unavailable |

Do not add an Internet ingress rule for port 5000. Nginx reaches the backend through the Docker network.

After direct EIP testing is no longer needed, replace the HTTP source with the CloudFront origin-facing managed prefix list.

### balan-coffee-rds-sg

| Type | Port | Source |
|---|---:|---|
| PostgreSQL | 5432 | Security group balan-coffee-ec2-sg |

Never use 0.0.0.0/0 for PostgreSQL.

## 6. Validation

+ The public subnet has a default route to the IGW.
+ Port 5000 is not exposed by the EC2 security group.
+ The RDS security group accepts 5432 only from the EC2 security group.
+ The private DB subnets are in different Availability Zones.

**Evidence:** VPC resource map, public route table, and inbound rules of both security groups.

