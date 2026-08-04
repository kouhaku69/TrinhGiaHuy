---
title: "Week 2 Worklog"
date: "2026-06-01"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Get a solid grasp of AWS IAM: User, Group, Role, Policy, and how to grant applications access securely.
* Get familiar with compute services: EC2, Lightsail, Lightsail Container, EC2 Auto Scaling.
* Use operational tools (Cloud9, AWS CLI) and start working with the data layer: RDS, DynamoDB, ElastiCache.
* Monitor systems with CloudWatch and extend hybrid networking with Route 53 Resolver.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: AWS Identity and Access Management (IAM)** <br>&emsp;+ Distinguished between IAM Group, IAM User, IAM Policy, and IAM Role <br>&emsp;+ Created an Admin Group and Admin User, and logged in as a test <br>&emsp;+ Created an Admin Role and an Operator User, and configured switch role <br>&emsp;+ Cleaned up resources <br>- **Lab: Getting Familiar with Amazon EC2** <br>&emsp;+ Prepared a separate VPC and Security Group for Linux and Windows <br>&emsp;+ Launched a Windows Server 2025 instance and an Amazon Linux instance, and connected to each <br>&emsp;+ Changed instance type, created an EBS Snapshot, created a Custom AMI, and launched an instance from it <br>&emsp;+ Learned how to recover access when a key pair is lost <br>- **Lab: Granting Application Access with an IAM Role on EC2** <br>&emsp;+ Prepared an EC2 instance and an S3 bucket <br>&emsp;+ Tried using an Access Key to access S3, then switched to an IAM Role attached to EC2 <br>&emsp;+ Compared the security level of both approaches and cleaned up resources | 01/06/2026 | 01/06/2026 | <https://000002.awsstudygroup.com/>, <https://000004.awsstudygroup.com/>, <https://000048.awsstudygroup.com/> |
| 2   | - **Lab: AWS Cloud9** <br>&emsp;+ Created a Cloud9 instance <br>&emsp;+ Practiced basic operations: using the command line, editing text files, going back to the Dashboard <br>&emsp;+ Used the AWS CLI directly inside Cloud9, and cleaned up resources <br>- **Lab: Amazon S3 Static Website Hosting** <br>&emsp;+ Learned the concepts of bucket, object, and region <br>&emsp;+ Created a bucket, uploaded sample source code, and enabled static website hosting <br>&emsp;+ Configured the public access block and public permissions for objects, then tested the website <br>- **Lab: Amazon RDS** <br>&emsp;+ Prepared the VPC, Security Groups for EC2 and RDS, and a DB Subnet Group <br>&emsp;+ Created an EC2 instance and an RDS database instance, and deployed an application connecting to RDS <br>&emsp;+ Practiced Backup and Restore, and cleaned up resources | 02/06/2026 | 02/06/2026 | <https://000049.awsstudygroup.com/>, <https://000057.awsstudygroup.com/>, <https://000005.awsstudygroup.com/> |
| 3   | - **Lab: Amazon Lightsail — Cost Optimization** <br>&emsp;+ Deployed a database and 3 open-source applications: WordPress, PrestaShop, Akaunting <br>&emsp;+ Configured networking and security for each application <br>&emsp;+ Created snapshots, upgraded to a larger instance, and set up alarms for monitoring <br>- **Lab: Amazon Lightsail Container** <br>&emsp;+ Created a Container Service and tried deploying a public image <br>&emsp;+ Built and pushed a custom image with Docker, then deployed it, and cleaned up resources <br>- **Lab: EC2 Auto Scaling & Load Balancer** <br>&emsp;+ Prepared the network infrastructure, EC2, RDS, and the base web server <br>&emsp;+ Created a Launch Template and an Application Load Balancer <br>&emsp;+ Created an Auto Scaling Group, and tested manual, scheduled, and dynamic scaling | 03/06/2026 | 03/06/2026 | <https://000045.awsstudygroup.com/>, <https://000046.awsstudygroup.com/>, <https://000006.awsstudygroup.com/> |
| 4   | - **Lab: AWS CloudWatch** <br>&emsp;+ Viewed and analyzed CloudWatch Metrics (search expressions, math expressions, dynamic labels) <br>&emsp;+ Worked with CloudWatch Logs, Logs Insights, and Metric Filters <br>&emsp;+ Created a CloudWatch Alarm and Dashboard for monitoring <br>- **Lab: Hybrid DNS with Route 53 Resolver** <br>&emsp;+ Prepared a Key Pair, CloudFormation Template, and Security Group <br>&emsp;+ Connected to the RDGW and deployed Microsoft AD <br>&emsp;+ Created Route 53 Outbound/Inbound Endpoints and Resolver Rules, then tested the results <br>- **Lab: Getting Started with the AWS CLI** <br>&emsp;+ Installed and configured the AWS CLI <br>&emsp;+ Worked with S3, SNS, IAM, and VPC through the CLI <br>&emsp;+ Created an EC2 instance via the CLI and practiced troubleshooting common errors | 04/06/2026 | 04/06/2026 | <https://000008.awsstudygroup.com/>, <https://000010.awsstudygroup.com/>, <https://000011.awsstudygroup.com/> |
| 5   | - **Lab: Amazon DynamoDB** <br>&emsp;+ Learned about Core Components, Primary Key, Secondary Index, Read Consistency, and Capacity Mode <br>&emsp;+ Practiced creating a table and writing/reading/updating/querying data through the Console and CloudShell <br>&emsp;+ Practiced with the AWS SDK (Python): CRUD operations, loading sample data, query/scan <br>- **Lab: Amazon ElastiCache (Redis)** <br>&emsp;+ Created a Subnet Group and a Redis cluster (cluster mode disabled/enabled) <br>&emsp;+ Connected to the cluster node and granted access <br>&emsp;+ Used the AWS SDK to set/get strings, hashes, publish/subscribe, and read/write a stream | 05/06/2026 | 05/06/2026 | <https://000060.awsstudygroup.com/>, <https://000061.awsstudygroup.com/> |

### 🏆 **Week 2 Achievements**

**1. Access Management & Security (IAM)**

* Clearly distinguished IAM Group, User, Role, and Policy; practiced switch role
* Compared Access Key and IAM Role when granting applications access on EC2

**2. Compute & Scaling**

* Launched and managed EC2 instances (Windows Server 2025, Amazon Linux), created a Custom AMI
* Deployed open-source applications on Lightsail and Lightsail Container
* Built an Auto Scaling Group combined with a Load Balancer for a scalable application

**3. Development & Operations Tools**

* Got familiar with AWS Cloud9 as a browser-based IDE
* Became comfortable using the AWS CLI with S3, SNS, IAM, VPC, and EC2

**4. Storage & Database**

* Hosted a static website on Amazon S3
* Deployed Amazon RDS and practiced backup/restore
* Worked with Amazon DynamoDB and Amazon ElastiCache (Redis) through the Console, CLI, and SDK

**5. Monitoring & Networking**

* Used CloudWatch Metrics, Logs, Alarms, and Dashboards to monitor the system
* Set up Hybrid DNS with Route 53 Resolver, connecting AWS with an on-premises Microsoft AD

### Week 2 Conclusion

Week 2 expanded quickly across many foundational services — from security (IAM) to compute (EC2, Lightsail, Auto Scaling), operational tooling (Cloud9, CLI), and the data layer (RDS, DynamoDB, ElastiCache), plus monitoring with CloudWatch and hybrid networking with Route 53. It was a fairly heavy load of material for one week, but working hands-on with each service made it much easier to follow than reading the theory alone. Auto Scaling and Hybrid DNS had the most configuration steps, so those are the two areas worth reviewing again before applying them to a real project.
