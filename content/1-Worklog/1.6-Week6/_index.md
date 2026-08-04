---
title: "Week 6 Worklog"
date: "2026-06-29"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Build reliability into the infrastructure: AWS Backup, VPC Peering, Transit Gateway.
* Build event-driven architecture with SNS/SQS, share data through EBS Multi-Attach.
* Deploy high availability for databases on Windows: Windows Failover Cluster, SQL Server HA (2019, 2022).
* Package and deploy applications with containers: Docker, ECS, CDK; set up CI/CD for containers and EC2.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Deploying AWS Backup** <br>&emsp;+ Created an S3 bucket, deployed the infrastructure for backup <br>&emsp;+ Created a Backup Plan for EBS, RDS, DynamoDB, and EFS <br>&emsp;+ Configured notifications through SNS, tested Restore, cleaned up resources <br>- **Lab: VPC Peering** <br>&emsp;+ Reviewed how to connect two VPCs directly without going over the internet <br>&emsp;+ Configured Route Tables and Network ACLs for the peering connection <br>- **Lab: AWS Transit Gateway** <br>&emsp;+ Compared VPC Peering and Transit Gateway as the number of VPCs grows <br>&emsp;+ Connected 4 VPCs through a single Transit Gateway <br>&emsp;+ Created Transit Gateway Attachments and Route Tables, added routes to VPC Route Tables | 29/06/2026 | 29/06/2026 | <https://000013.awsstudygroup.com/>, <https://000019.awsstudygroup.com/>, <https://000020.awsstudygroup.com/> |
| 2   | - **Lab: Event-driven Architecture with SNS and SQS** <br>&emsp;+ Deployed the infrastructure and a sample event generator <br>&emsp;+ Practiced a basic pub/sub pattern <br>&emsp;+ Configured message filtering and advanced message filtering to route messages by attribute <br>- **Lab: Sharing Data Across VPCs with EBS Multi-Attach (NVMe Reservation)** <br>&emsp;+ Created 2 VPCs (Prod, Test) and an EC2 instance in each <br>&emsp;+ Created a single EBS volume and attached it to both instances <br>&emsp;+ Installed PostgreSQL, used NVMe Reservation to control access to the shared volume <br>- **Lab: MySQL HA Cluster with EBS Multi-Attach** <br>&emsp;+ Created a VPC, EC2 instances, a shared EBS volume, configured LVM <br>&emsp;+ Installed MySQL, configured a Network Load Balancer, deployed WordPress connecting through the NLB <br>&emsp;+ Created a Response Plan in Incident Manager, a CloudWatch Alarm, and tested Failover | 30/06/2026 | 30/06/2026 | <https://000077.awsstudygroup.com/>, <https://100000.awsstudygroup.com/>, <https://100001.awsstudygroup.com/> |
| 3   | - **Lab: Windows Failover Cluster with EBS Multi-Attach** <br>&emsp;+ Created a VPC, Active Directory, and the cluster node EC2 instances <br>&emsp;+ Created a shared EBS volume, joined the domain, installed the Windows Feature for clustering <br>&emsp;+ Created the Failover Cluster, configured networking and storage for the cluster <br>- **Lab: SQL Server 2019 on Windows Failover Cluster** <br>&emsp;+ Installed SQL Server 2019 on node 1, installed SSMS <br>&emsp;+ Added node 2 to the cluster, installed SSMS on node 2 <br>&emsp;+ Verified the cluster installation <br>- **Lab: MSSQL Cluster with Windows Failover Cluster (2022)** <br>&emsp;+ Repeated the WSFC build process with SQL Server 2022 <br>&emsp;+ Installed SQL Server, added node 2, verified the results | 01/07/2026 | 01/07/2026 | <https://100002.awsstudygroup.com/>, <https://100003.awsstudygroup.com/>, <https://100004.awsstudygroup.com/> |
| 4   | - **Lab: Deploying Applications with Docker** <br>&emsp;+ Deployed the application locally first, then prepared a VPC/Security Group/IAM Role for ECR <br>&emsp;+ Created an RDS instance, configured EC2 to run the container <br>&emsp;+ Deployed using a Docker image and using Docker Compose, pushed the image to ECR/Docker Hub <br>- **Lab: Deploying Applications on Amazon ECS** <br>&emsp;+ Prepared the infrastructure, created an ECS Cluster and Task Definition <br>&emsp;+ Configured an Application Load Balancer, created an ECS Service <br>&emsp;+ Verified the deployment <br>- **Lab: Deploying Spring Boot on ECS Fargate with AWS CDK** <br>&emsp;+ Created an ECR repository, a VPC, and a NAT Gateway with CDK <br>&emsp;+ Created an ECS Cluster, Service, API Gateway, and a DynamoDB table with CDK <br>&emsp;+ Instrumented the service with AWS X-Ray to trace requests | 02/07/2026 | 02/07/2026 | <https://000015.awsstudygroup.com/>, <https://000016.awsstudygroup.com/>, <https://000118.awsstudygroup.com/> |
| 5   | - **Lab: CI/CD for Containerized Applications on ECS** <br>&emsp;+ Set up CI/CD with a GitLab Runner, with GitHub Actions, and with CodeBuild <br>&emsp;+ Monitored the application with Container Insights (CloudWatch) <br>&emsp;+ Routed logs with Firelens, stored logs in S3 <br>- **Lab: Deploying Applications to EC2 with AWS CodePipeline** <br>&emsp;+ Prepared the infrastructure, an S3 bucket, a Git connection, and an IAM Role/Instance Profile <br>&emsp;+ Configured the CodeDeploy Agent, CodeCommit, CodeBuild, and CodeDeploy <br>&emsp;+ Assembled everything into a complete CodePipeline, troubleshot common issues | 03/07/2026 | 03/07/2026 | <https://000017.awsstudygroup.com/>, <https://000023.awsstudygroup.com/> |

### 🏆 **Week 6 Achievements**

**1. Reliability & Network Connectivity**

* Automated backup/restore for EBS, RDS, DynamoDB, and EFS with AWS Backup
* Compared and deployed VPC Peering and Transit Gateway for multi-VPC connectivity models

**2. Event-driven Architecture & Storage Sharing**

* Built a pub/sub flow with message filtering using SNS/SQS
* Used EBS Multi-Attach to share a volume across multiple EC2 instances, applied NVMe Reservation to control access
* Built a MySQL HA cluster with an NLB, Incident Manager, and a CloudWatch Alarm

**3. High Availability for SQL Server on Windows**

* Built a Windows Failover Cluster using EBS Multi-Attach
* Installed SQL Server 2019 and 2022 on WSFC, verified the cluster was working

**4. Containerization & CI/CD**

* Packaged an application with Docker, pushed the image to ECR/Docker Hub
* Deployed an application on ECS through the Console and through AWS CDK (with API Gateway, DynamoDB, X-Ray)
* Set up CI/CD for containers (GitLab, GitHub Actions, CodeBuild) and for EC2 (CodePipeline, CodeCommit, CodeDeploy)
* Monitored the containerized application with Container Insights and routed logs with Firelens

### Week 6 Conclusion

Week 6 had two distinct parts: the Reliability track (Backup, VPC Peering, Transit Gateway, SNS/SQS, EBS Multi-Attach, Windows Failover Cluster) and the start of the Performance track with containers (Docker, ECS, CI/CD). The Windows Failover Cluster with SQL Server labs took the most time, since they require several steps to prepare Active Directory and configure cluster storage. The container and CI/CD section introduced a different deployment approach compared to the earlier EC2/VPC labs, and CDK kept showing up as the main tool for defining ECS infrastructure as code.
