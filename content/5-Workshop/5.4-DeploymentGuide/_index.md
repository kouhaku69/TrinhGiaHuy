---
title: "AWS configuration and deployment"
date: "2026-08-07"
weight: 4
chapter: false
pre: "<b> 5.4. </b>"
---

# Configure AWS and deploy the application

This section turns the architecture into concrete AWS Console procedures. Each lesson explains the objective, values to enter, source-code dependency, validation steps, and required evidence.

#### Implementation order

1. [Create the VPC, subnets, route table, and security groups](5.4.1-Networking/)
2. [Create the IAM role, EC2 instance, and Elastic IP](5.4.2-IAM-EC2/)
3. [Create Amazon RDS for PostgreSQL](5.4.3-RDS/)
4. [Store application secrets in AWS Secrets Manager](5.4.4-SecretsManager/)
5. [Create and configure Amazon S3](5.4.5-S3/)
6. [Create the Amazon Cognito User Pool and App Client](5.4.6-Cognito/)
7. [Enable Amazon Bedrock model access](5.4.7-Bedrock/)
8. [Deploy the frontend and backend with Docker](5.4.8-Docker/)
9. [Create the Amazon CloudFront distribution](5.4.9-CloudFront/)
10. [Configure Amazon CloudWatch](5.4.10-CloudWatch/)
11. [Test, accept, and clean up the workshop](5.4.11-Test-Cleanup/)

#### Verified deployment values

| Item | Value |
|---|---|
| Region | `ap-southeast-1` (Singapore) |
| EC2 | `i-03642ee2788132cb3`, `t3.medium`, `ap-southeast-1a` |
| Elastic IP | `54.251.119.230` |
| CloudFront | `d3pn12mzrv3aqy.cloudfront.net` |
| Frontend container | `balan-frontend`, `80:80` |
| Backend container | `balan-backend`, `5000:5000`, `/health` health check |
| CloudWatch log group | `/balancoffee/backend` |

{{% notice warning %}}
Never add access keys, passwords, tokens, secret values, or real connection strings to Markdown or screenshots. Sensitive values belong in Secrets Manager or an untracked `.env` file.
{{% /notice %}}
