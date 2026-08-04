---
title: "Week 5 Worklog"
date: "2026-06-22"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Manage identity centrally with IAM Identity Center (SSO), restrict permissions with Permission Boundaries and IAM Conditions.
* Monitor security compliance with Security Hub, control S3 access through VPC Endpoints, block web attacks with WAF.
* Encrypt data with KMS, discover sensitive data with Macie, manage secrets with Secrets Manager.
* Manage Security Groups centrally with Firewall Manager, detect threats with GuardDuty, automate patching with EC2 Image Builder.
* Authenticate users across platforms with Cognito, apply S3 security best practices.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: IAM Identity Center (AWS SSO)** <br>&emsp;+ Created an AWS account in AWS Organizations, configured an Organization Unit <br>&emsp;+ Invited a member account into the Organization, granted access through Permission Sets <br>&emsp;+ Accessed via the AWS CLI, restricted access by time, used Customer Managed Policies <br>- **Lab: IAM Permission Boundary** <br>&emsp;+ Created a Restriction Policy to act as a Permission Boundary <br>&emsp;+ Created a restricted IAM User, verified the user's actual effective permissions <br>- **Lab: IAM Role & Condition** <br>&emsp;+ Created separate IAM Groups/Users for EC2 and RDS <br>&emsp;+ Created an Admin Role, configured switch role <br>&emsp;+ Restricted switch role by IP address and by time window | 22/06/2026 | 22/06/2026 | <https://000012.awsstudygroup.com/>, <https://000030.awsstudygroup.com/>, <https://000044.awsstudygroup.com/> |
| 2   | - **Lab: AWS Security Hub** <br>&emsp;+ Reviewed the security standards applied to the account <br>&emsp;+ Enabled Security Hub, reviewed the security score for each standard <br>- **Lab: Secure Access to S3 via VPC Endpoints** <br>&emsp;+ Created a Gateway Endpoint to access S3 from the VPC and tested the connection <br>&emsp;+ Created an Interface Endpoint to access S3 from on-premises, simulated on-premises DNS <br>&emsp;+ Configured a VPC Endpoint Policy to restrict access <br>- **Lab: AWS WAF** <br>&emsp;+ Prepared an S3 bucket and deployed a sample web app <br>&emsp;+ Created a Web ACL with managed rules, a custom rule, and an advanced custom rule <br>&emsp;+ Tested the new rules and enabled request logging | 23/06/2026 | 23/06/2026 | <https://000018.awsstudygroup.com/>, <https://000111.awsstudygroup.com/>, <https://000026.awsstudygroup.com/> |
| 3   | - **Lab: Encrypting Data at Rest with AWS KMS** <br>&emsp;+ Created a Policy/Role and a Group/User, created a KMS key <br>&emsp;+ Created an S3 bucket, uploaded data encrypted with the KMS key <br>&emsp;+ Created CloudTrail logging, queried logs with Athena, tried sharing the encrypted data <br>- **Lab: Discovering Sensitive Data with Amazon Macie** <br>&emsp;+ Created an S3 bucket, enabled Macie <br>&emsp;+ Created a Custom Data Identifier, created a Macie job to scan the data <br>&emsp;+ Reviewed the results and findings on sensitive data / misconfigured buckets <br>- **Lab: AWS Secrets Manager with RDS and Fargate** <br>&emsp;+ Prepared the infrastructure (VPC, Bastion Host, private RDS) <br>&emsp;+ Accessed RDS using credentials stored in Secrets Manager, practiced Secret Rotation <br>&emsp;+ Accessed RDS from an application running on Fargate | 24/06/2026 | 24/06/2026 | <https://000033.awsstudygroup.com/>, <https://000090.awsstudygroup.com/>, <https://000096.awsstudygroup.com/> |
| 4   | - **Lab: Managing Security Groups with AWS Firewall Manager** <br>&emsp;+ Reviewed the role of Security Groups in controlling remote access (RDP, SSH) <br>&emsp;+ Configured AWS Firewall Manager to audit Security Groups centrally across accounts <br>&emsp;+ Applied a policy to restrict Security Groups <br>- **Lab: Amazon GuardDuty** <br>&emsp;+ Learned how GuardDuty works <br>&emsp;+ Simulated 3 scenarios: a compromised EC2 instance, compromised IAM credentials, and IAM role credential exfiltration <br>&emsp;+ Automated the response with EventBridge + Lambda for some scenarios <br>- **Lab: Automated Patching with EC2 Image Builder & Systems Manager** <br>&emsp;+ Built the base infrastructure and the application infrastructure <br>&emsp;+ Created an AMI Builder Pipeline with EC2 Image Builder <br>&emsp;+ Automated the build process with an SSM Automation Document, deployed the new AMI through a CloudFormation AutoScalingReplacingUpdate | 25/06/2026 | 25/06/2026 | <https://000097.awsstudygroup.com/>, <https://000098.awsstudygroup.com/>, <https://000099.awsstudygroup.com/> |
| 5   | - **Lab: Amazon Cognito Cross-Site** <br>&emsp;+ Distinguished between a Cognito User Pool and Identity Pool <br>&emsp;+ Prepared the infrastructure and reviewed the sample code <br>&emsp;+ Deployed and tested Cognito authentication across multiple sites/applications <br>- **Lab: S3 Security Best Practices** <br>&emsp;+ Prepared the infrastructure through CloudFormation, created an access key <br>&emsp;+ Required HTTPS and SSE-S3 encryption, blocked public ACLs, and configured S3 Block Public Access <br>&emsp;+ Restricted access through an S3 VPC Endpoint, used AWS Config to detect public buckets, and used Access Analyzer for S3 | 26/06/2026 | 26/06/2026 | <https://000141.awsstudygroup.com/>, <https://000069.awsstudygroup.com/> |

### 🏆 **Week 5 Achievements**

**1. Identity Management & Access Control**

* Configured IAM Identity Center for centralized access management across multiple AWS accounts
* Applied Permission Boundaries to cap the maximum permissions of a user
* Configured IAM Roles with conditions restricting access by IP and by time

**2. Compliance Monitoring & Web Application Protection**

* Enabled Security Hub to view compliance scores against security standards
* Configured VPC Endpoints (Gateway, Interface) to access S3 without going over the public internet
* Configured AWS WAF with managed rules and custom rules to block web attacks

**3. Data Protection**

* Encrypted S3 data with KMS, queried access logs with CloudTrail and Athena
* Used Macie to discover sensitive data (PII, financial data) in S3
* Managed and automatically rotated secrets with Secrets Manager for RDS and Fargate

**4. Threat Detection & Response**

* Audited and restricted Security Groups centrally with Firewall Manager
* Simulated and handled compromise scenarios with GuardDuty, EventBridge, and Lambda
* Automated OS patching with EC2 Image Builder and Systems Manager

**5. Authentication & S3 Security**

* Deployed a Cognito User Pool/Identity Pool for cross-site authentication
* Applied S3 security best practices: requiring HTTPS, SSE-S3 encryption, blocking public access, using Access Analyzer

### Week 5 Conclusion

Week 5 focused on the Security track within the Optimize category, moving from identity management (SSO, Permission Boundaries, Role Conditions) to data protection (KMS, Macie, Secrets Manager) and threat detection (GuardDuty, Firewall Manager). Most of these labs build on concepts covered earlier (IAM policies, VPC, CloudFormation), so the hardest part was chaining services together into a complete incident-response flow — for example, combining GuardDuty with EventBridge and Lambda for automated response. This week's content maps directly to the compliance requirements many organizations expect when running systems on AWS.
