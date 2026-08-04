---
title: "Week 4 Worklog"
date: "2026-06-15"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Operate systems remotely using Systems Manager and Session Manager.
* Deploy infrastructure as code with CloudFormation and AWS CDK.
* Optimize cost and resources: EC2 Resource Optimization, Service Quotas, IAM usage restrictions.
* Automate the snapshot lifecycle and detect anomalies in backups.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: AWS Systems Manager** <br>&emsp;+ Prepared a VPC, a Windows EC2 instance, and an IAM Role for the SSM Agent <br>&emsp;+ Used Patch Manager for automated patching <br>&emsp;+ Used Run Command to run commands across multiple servers at once <br>- **Lab: Systems Manager — Session Manager** <br>&emsp;+ Prepared a VPC with public/private subnets, a public EC2 instance, and a private instance <br>&emsp;+ Connected to the private instance through VPC Endpoints (ssm, ssmmessages, ec2messages) without a bastion host <br>&emsp;+ Managed session logs written to S3 and practiced Port Forwarding <br>- **Lab: AWS CloudFormation** <br>&emsp;+ Wrote a basic CloudFormation Template in Cloud9 <br>&emsp;+ Worked with Custom Resources using Lambda, Mappings and StackSets, and Drift Detection | 15/06/2026 | 15/06/2026 | <https://000031.awsstudygroup.com/>, <https://000058.awsstudygroup.com/>, <https://000037.awsstudygroup.com/> |
| 2   | - **Lab: AWS CDK Essentials** <br>&emsp;+ Learned what CDK is and how it relates to CloudFormation <br>&emsp;+ Created a workspace and configured the Cloud9 environment <br>&emsp;+ Wrote and updated a first CDK Template, deploying EC2 through user data <br>- **Lab: AWS CDK Advanced** <br>&emsp;+ Used CDK to build an architecture with API Gateway, ALB, ECS, and Lambda <br>&emsp;+ Combined Lambda with S3 <br>&emsp;+ Created a Nested Stack with CDK <br>- **Lab: Infrastructure as Code Workshop Series** <br>&emsp;+ Reviewed IaC concepts and compared common frameworks <br>&emsp;+ Created a Lambda Function, a VPC, and EC2 through code <br>&emsp;+ Deployed a Three-Tier architecture (Web/Application/Database) using a CloudFormation Stack and checked access at each tier | 16/06/2026 | 16/06/2026 | <https://000038.awsstudygroup.com/>, <https://000076.awsstudygroup.com/>, <https://000102.awsstudygroup.com/> |
| 3   | - **Lab: Right-Sizing Amazon EC2** <br>&emsp;+ Got familiar with Amazon CloudWatch, created an IAM Role for the CloudWatch Agent <br>&emsp;+ Installed the CloudWatch Agent to collect memory metrics <br>&emsp;+ Reviewed recommendations from EC2 Resource Optimization and AWS Compute Optimizer <br>- **Lab: Monitoring Network Infrastructure with VPC Flow Logs** <br>&emsp;+ Created and enabled VPC Flow Logs, sending data to CloudWatch Logs <br>&emsp;+ Analyzed traffic to determine whether a Security Group was too restrictive or too permissive <br>- **Lab: Delegating Access to the Billing Console** <br>&emsp;+ Created an IAM User Group, enabled Billing access <br>&emsp;+ Created and attached an IAM Policy allowing cost viewing/management, and tested the access | 17/06/2026 | 17/06/2026 | <https://000032.awsstudygroup.com/>, <https://000074.awsstudygroup.com/>, <https://000075.awsstudygroup.com/> |
| 4   | - **Lab: Managing Service Quotas** <br>&emsp;+ Learned about Service Quotas — the default limits for each AWS service <br>&emsp;+ Practiced submitting a quota increase request <br>- **Lab: Cost & Usage Management with IAM** <br>&emsp;+ Created an IAM Group/User, wrote a policy restricting access by Region <br>&emsp;+ Restricted by EC2 instance family, by instance size, and by EBS volume type <br>&emsp;+ Verified each restriction policy took effect <br>- **Lab: Automatically Archiving EBS Snapshots with Data Lifecycle Manager** <br>&emsp;+ Created an EC2 instance with a sample snapshot <br>&emsp;+ Set up a single policy schedule and multiple policy schedules <br>&emsp;+ Checked the results of the automated archive/deletion of snapshots | 18/06/2026 | 18/06/2026 | <https://000063.awsstudygroup.com/>, <https://000064.awsstudygroup.com/>, <https://000088.awsstudygroup.com/> |
| 5   | - **Lab: AWS Backup Anomaly Detection for EBS** <br>&emsp;+ Prepared an S3 bucket, an EBS volume, and infrastructure through CloudFormation <br>&emsp;+ Created a backup and studied the anomaly-detection pipeline (AWS Backup → EventBridge → Lambda → DynamoDB → CloudWatch → SNS) <br>&emsp;+ Tracked alerts triggered when the number of changed blocks between snapshots crossed the threshold <br>- **Lab: AWS Toolkit for VS Code — Amazon Q & CodeWhisperer** <br>&emsp;+ Installed the AWS Toolkit for VS Code and connected an AWS account <br>&emsp;+ Used AWS Explorer to work with AWS services directly inside the IDE <br>&emsp;+ Used Amazon Q for Q&A and debugging code, and Amazon CodeWhisperer for code suggestions and security scanning | 19/06/2026 | 19/06/2026 | <https://000089.awsstudygroup.com/>, <https://000087.awsstudygroup.com/> |

### 🏆 **Week 4 Achievements**

**1. Remote Operations**

* Used Systems Manager Patch Manager and Run Command for patching and running commands at scale
* Connected to private EC2 instances through Session Manager and VPC Endpoints, without opening SSH/RDP ports externally

**2. Infrastructure as Code**

* Wrote CloudFormation Templates from basic to advanced (Custom Resources, StackSets, Drift Detection)
* Used AWS CDK to define infrastructure as code, building an ECS/ALB/API Gateway/Lambda architecture and a Nested Stack
* Deployed a Three-Tier architecture using CloudFormation

**3. Cost & Resource Optimization**

* Right-sized EC2 based on CloudWatch Agent metrics and Compute Optimizer recommendations
* Monitored network traffic with VPC Flow Logs, delegated Billing Console access through IAM
* Restricted usage with IAM policies by Region, EC2 family, instance size, and EBS volume type
* Managed Service Quotas and submitted quota increase requests

**4. Automation & Data Protection**

* Automated the EBS Snapshot lifecycle with Data Lifecycle Manager
* Studied the anomaly-detection pipeline in AWS Backup (EventBridge, Lambda, DynamoDB, CloudWatch, SNS)

**5. Developer Tooling**

* Installed the AWS Toolkit for VS Code, used Amazon Q and CodeWhisperer while writing code

### Week 4 Conclusion

Week 4 covered two main areas: remote system operations (Systems Manager, Session Manager) and Infrastructure as Code (CloudFormation, CDK). CDK generates CloudFormation underneath, so learning CloudFormation first makes CDK easier to follow. The labs on Service Quotas, IAM usage restrictions, and Data Lifecycle Manager are cost-governance tasks that come up repeatedly when operating an AWS account over the long term.
