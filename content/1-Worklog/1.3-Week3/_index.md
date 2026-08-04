---
title: "Week 3 Worklog"
date: "2026-06-08"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Go deeper into AWS networking: advanced VPC, Transit Gateway, CloudFront and Lambda@Edge.
* Get familiar with Windows on AWS: WorkSpaces and AWS Managed Microsoft AD.
* Practice system migration: VM Import/Export, Database Migration (SCT/DMS), Disaster Recovery.
* Optimize cost and monitoring: Lambda automation, Grafana, CloudWatch, Tags & Resource Groups.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: AWS Networking and Content Delivery** <br>&emsp;+ Reviewed VPC components in depth: Subnet, Route Table, ENI, EIP, VPC Endpoint, Internet Gateway, NAT Gateway, Security Group vs. NACL <br>&emsp;+ Deployed Transit Gateway and a Site-to-Site VPN through a Cisco CSR router (accessed via Cloud9) <br>&emsp;+ Set up Route 53 DNS Endpoints and Internal Hosted Zones, VPC Endpoints for AWS services, and VPC Endpoint Services (PrivateLink) <br>&emsp;+ Practiced VPC Peering and Transit Gateway Network Manager <br>- **Lab: CloudFront with S3 Origin** <br>&emsp;+ Created an S3 bucket and uploaded a sample index.html file <br>&emsp;+ Configured CloudFront to distribute content from S3, then cleaned up resources <br>- **Lab: Advanced AWS CloudFront** <br>&emsp;+ Created a CloudFront Distribution with EC2 as the origin, tested the application <br>&emsp;+ Configured Distribution Invalidations, a Custom Error Page, Origin Group, Response Headers, and Cache Behavior <br>&emsp;+ Wrote and deployed a Lambda@Edge function, reviewed Metrics and Logs | 08/06/2026 | 08/06/2026 | <https://000092.awsstudygroup.com/>, <https://000094.awsstudygroup.com/>, <https://000130.awsstudygroup.com/> |
| 2   | - **Lab: Windows on AWS — Amazon WorkSpaces** <br>&emsp;+ Prepared and deployed Amazon WorkSpaces <br>&emsp;+ Accessed WorkSpaces through the browser and through the WorkSpaces Client <br>&emsp;+ Cleaned up resources <br>- **Lab: Windows on AWS — AWS Managed Microsoft AD** <br>&emsp;+ Deployed AWS Managed Directory Service <br>&emsp;+ Deployed an EC2 instance joined to the domain, tested communication between servers <br>&emsp;+ Cleaned up resources <br>- **Lab: Building Highly Available Web Applications** <br>&emsp;+ Reviewed standard HA architecture concepts: multi-AZ deployment, Load Balancer, Auto Scaling, RDS Multi-AZ <br>&emsp;+ (Note: the lab page's detailed content could not be loaded, so only the concepts were reviewed) | 09/06/2026 | 09/06/2026 | <https://000093.awsstudygroup.com/>, <https://000095.awsstudygroup.com/>, <https://000101.awsstudygroup.com/> |
| 3   | - **Lab: VM Import/Export** <br>&emsp;+ Prepared VMware Workstation, exported a VM from the on-premises environment <br>&emsp;+ Uploaded and imported the VM into AWS, created an AMI, and launched an instance from it <br>&emsp;+ Configured an S3 bucket ACL to export the instance/AMI back to on-premises <br>- **Lab: Database Schema Conversion & Migration (SCT/DMS)** <br>&emsp;+ Prepared a Key Pair and environment, connected to an Oracle/SQL Server source <br>&emsp;+ Converted the schema using the AWS Schema Conversion Tool, configured the target database (RDS SQL Server, Aurora MySQL/PostgreSQL) <br>&emsp;+ Created a Replication Instance, DMS Endpoint, and Migration Task; tried DMS Serverless and monitored scaling through CloudWatch <br>&emsp;+ Handled common issues such as Memory Pressure and Table Errors <br>- **Lab: AWS Elastic Disaster Recovery** <br>&emsp;+ Prepared a simulated on-premises infrastructure, connected to the Bastion Host <br>&emsp;+ Configured DRS, installed the Agent, and set up the Launch Template <br>&emsp;+ Performed a Failover and cleaned up resources | 10/06/2026 | 10/06/2026 | <https://000014.awsstudygroup.com/>, <https://000043.awsstudygroup.com/>, <https://000100.awsstudygroup.com/> |
| 4   | - **Lab: Optimizing EC2 Costs with Lambda** <br>&emsp;+ Prepared a VPC, Security Group, EC2 instance, and a Slack incoming webhook <br>&emsp;+ Created a tag for the instance, created an IAM Role for Lambda <br>&emsp;+ Wrote Lambda functions to automatically start/stop the instance, and checked the results <br>- **Lab: Monitoring with Grafana** <br>&emsp;+ Prepared a VPC, EC2 instance, and an IAM User/Role <br>&emsp;+ Installed Grafana on EC2 <br>&emsp;+ Built a dashboard to monitor AWS resources <br>- **Lab: CloudWatch Advanced Workshop** <br>&emsp;+ Went deeper into CloudWatch Metrics (search/math expressions, dynamic labels) <br>&emsp;+ Advanced Logs Insights, Metric Filters, Alarms, and Dashboards | 11/06/2026 | 11/06/2026 | <https://000022.awsstudygroup.com/>, <https://000029.awsstudygroup.com/>, <https://000036.awsstudygroup.com/> |
| 5   | - **Lab: Managing Resources with Tags & Resource Groups** <br>&emsp;+ Tagged EC2 instances through the Console and the CLI <br>&emsp;+ Filtered resources by tag <br>&emsp;+ Created a Resource Group based on tags or on a CloudFormation stack <br>- **Lab: Controlling EC2 Access with Resource Tags via IAM** <br>&emsp;+ Created an IAM Policy with tag-based conditions, created an IAM Role dedicated to the EC2 Administrator <br>&emsp;+ Switched roles and verified permissions to create/edit EC2 instances across multiple Regions (Tokyo, North Virginia) based on whether the required tag was present | 12/06/2026 | 12/06/2026 | <https://000027.awsstudygroup.com/>, <https://000028.awsstudygroup.com/> |

### 🏆 **Week 3 Achievements**

**1. Advanced Networking & Content Delivery**

* Reviewed VPC components in depth (ENI, EIP, VPC Endpoint, NAT/IGW) and deployed Transit Gateway combined with Site-to-Site VPN
* Built CloudFront Distributions with both S3 and EC2 origins, wrote a Lambda@Edge function to process requests

**2. Windows on AWS**

* Deployed Amazon WorkSpaces and accessed it through multiple methods
* Deployed AWS Managed Microsoft AD and verified connectivity between domain-joined servers

**3. System Migration**

* Practiced VM Import/Export to move virtual machines between on-premises and AWS in both directions
* Converted schemas and migrated data using AWS SCT/DMS, including DMS Serverless
* Simulated Disaster Recovery with AWS Elastic Disaster Recovery and performed a Failover

**4. Cost Optimization & Monitoring**

* Automated EC2 start/stop with Lambda to reduce costs
* Monitored resources with Grafana and advanced CloudWatch features (Logs Insights, Metric Filters)

**5. Resource Management & Access Control**

* Used tags and Resource Groups to manage resources systematically
* Applied tag-based IAM policy conditions to restrict the EC2 Administrator's permissions by Region

### Week 3 Conclusion

Week 3 shifted focus to larger-scale operational challenges: advanced networking with Transit Gateway/CloudFront, the Windows on AWS ecosystem, and especially system migration (VM, database, disaster recovery) — a skill set that matters a lot when working with customers moving their infrastructure to the cloud. Database Migration with SCT/DMS was the heaviest topic, and it's worth practicing again to fully internalize the schema conversion flow and how to troubleshoot migration errors. Combining tags with IAM policy conditions to restrict permissions by Region is also a governance technique worth applying to future multi-environment projects.
