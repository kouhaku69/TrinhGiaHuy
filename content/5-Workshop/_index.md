---
title: "Workshop"
date: "2026-08-07"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploy Balan Coffee & Roastery on AWS with Docker and Amazon EC2

#### Overview

This workshop provides an end-to-end guide for designing, configuring, deploying, and validating **Balan Coffee & Roastery** on AWS. It is based on `trinpce192008/AWS_Workshop`, branch `aws-workshop-v2`, and follows the step-by-step style of `workshop-template/content/5-Workshop`.

The active request path is:

**User → HTTPS → Amazon CloudFront → EC2 public DNS/Elastic IP `54.251.119.230` → Amazon EC2 → Docker frontend/Nginx → Docker backend**

The backend uses Amazon RDS for PostgreSQL and integrates with Amazon S3, Amazon Cognito, Amazon Bedrock, AWS Secrets Manager, and Amazon CloudWatch.

#### Workshop content

1. [Solution Design Document](5.1-SolutionDesignDocument/)
2. [Solution Architecture](5.2-SolutionArchitecture/)
3. [Environment Preparation](5.3-EnvironmentSetup/)
4. [AWS Configuration and Deployment](5.4-DeploymentGuide/)
5. [Monitoring Guide](5.5-MonitoringGuide/)
6. [Architecture Decisions](5.6-ArchitectureDecisions/)

{{% notice info %}}
The primary endpoint is [CloudFront](https://d3pn12mzrv3aqy.cloudfront.net). The [Elastic IP](http://54.251.119.230/) is the stable origin address and supports direct diagnostics. Resource names not present in the repository or supplied evidence are clearly marked as **recommended values** and must be replaced with real IDs before final submission.
{{% /notice %}}
