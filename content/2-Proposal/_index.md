---
title: "Proposal"
date: "2025-12-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# TaskHub - Task and Progress Management Platform following the DevSecOps Model on AWS

> **Proposal Document:** [View on Google Docs](https://docs.google.com/document/d/14wEgjSuUkQs6SSVXwNQr4wmjwORjntfDizzC4zmPZIY/edit?tab=t.0)

### **1. Executive Summary**

TaskHub is a task and progress management platform designed to help working groups or small to medium-sized businesses manage work, deadlines, and progress in a visual and secure manner.

The system is developed following the **DevSecOps** model, built entirely on **AWS Serverless**, ensuring scalability, security, and cost optimization.

The development and deployment process uses **AWS CodePipeline** and **CodeBuild** to automate CI/CD and security testing.

### **2. Problem Statement**

**Problem:**

- Small businesses and project teams often struggle with managing workload, tracking progress, and distributing tasks among members.
- Popular management tools like Jira or Asana often have high costs and lack seamless support for DevSecOps processes or the AWS environment.

**Solution:**

- TaskHub uses a Serverless architecture on AWS to build a lightweight, secure, and cost-effective platform.
- The platform is developed using **AWS Lambda**, **API Gateway**, **DynamoDB**, **Cognito**, and **S3/CloudFront**, while integrating **AWS CodePipeline** for CI/CD and dynamic security testing.

**Benefits and Return on Investment (ROI)**

The TaskHub solution brings many practical benefits for development teams and small to medium-sized businesses. The system serves as a central platform for managing tasks, tracking progress, and delegating authority to members effectively. The application of the serverless model on AWS helps minimize operating costs, optimize resources, and increase scalability as usage demand grows. Furthermore, this platform supports building a practical DevSecOps environment, paving the way for research and development teams to expand further projects. According to estimates from the AWS Pricing Calculator, the system's operating cost is only about **$0.66 per month**, equivalent to **$7.92 per year**, while the entire initial infrastructure leverages shared services from AWS, with no additional hardware costs. The expected payback period is achieved within 6-12 months due to significantly reduced manual management work and optimized internal workflow.

### **3. Solution Architecture**

The TaskHub platform is built based on AWS Serverless architecture, ensuring operational scalability, high performance, and cost-effective operation. The system focuses on task management, teamwork, and real-time project progress, while maintaining an automated development and deployment process through the DevSecOps model.

The overall architecture includes key components such as Amazon API Gateway taking responsibility for receiving and distributing user requests, AWS Lambda handling business logic backend and interacting with the Amazon DynamoDB database to store task information, users, and access permissions.

The web interface is hosted via **Amazon S3** and distributed globally by **Amazon CloudFront**, while AWS Cognito ensures authentication and user authorization.

The CI/CD process is automated using **AWS CodePipeline** combined with **AWS CodeBuild**, enabling continuous development deployment and security testing without server management.

The entire architecture is protected by **AWS WAF** and **AWS KMS** to enhance data security and ensure additional DevSecOps compliance. **AWS X-Ray** is used to monitor performance and analyze latency. The overall architecture is described in detail in the diagram below:

![anh1](/images/2-Proposal/image1.png)

### AWS Services Used

1.  **Amazon Route 53:** Highly reliable DNS service, routing traffic.
2.  **AWS WAF (Web Application Firewall):** Advanced protection layer, blocking common attacks.
3.  **Amazon CloudFront:** Global distribution of user interface and static content.
4.  **Amazon S3 (Simple Storage Service):** Static hosting of the entire web interface source code (Next.js build files).
5.  **Amazon Cognito:** User authentication and authorization management.
6.  **Amazon API Gateway:** Middleware communication layer, performing authentication and routing API requests to Lambda.
7.  **AWS Lambda:** Core business logic processing. Integrated to log activities into CloudWatch Logs.
8.  **Amazon DynamoDB:** High-performance NoSQL database. Data is encrypted using **AWS KMS**.
9.  **AWS SNS (Simple Notification Service):** Handles asynchronous notifications.
10. **AWS Secrets Manager:** Secure storage, management, and rotation of secrets.
11. **AWS CodePipeline, CodeBuild, & CodeGuru:**
    - **CodePipeline/CodeBuild:** Building and automating the CI/CD process. CodeBuild runs automated tests and Static Application Security Testing (SAST).
    - **AWS CodeGuru:** Automated source code analysis tool, integrated into the CI/CD process to **provide intelligent recommendations for performance optimization and code quality improvement**, especially important in the Lambda environment.
12. **AWS CloudFormation:** **Infrastructure as Code (IaC)** service for deploying all resources.
13. **AWS CloudWatch Logs & AWS X-Ray:** **CloudWatch Logs** collects logs. **CloudWatch** uses this data to set up alerts. **AWS X-Ray** provides in-depth request tracing capabilities.

### **Component Design**

1.  **User Interface Layer (Frontend):**
    - **Interface:** **Next.js** application built as static files.
    - **Hosting & Distribution:** Static files are securely stored in **Amazon S3** (configured as Origin for CloudFront). This interface is distributed globally by **Amazon CloudFront** with low latency, while being protected by **AWS WAF** (Web Application Firewall) at the Edge layer.

2.  **Business Logic Layer (Backend):**
    - **API Gateway:** **Amazon API Gateway** receives all requests. It is configured with **Cognito Authorizer** to validate user tokens before forwarding requests.
    - **Processing:** **Lambda Functions** are responsible for handling business logic (CRUD tasks, team management, permissions).
    - **Secret Management:** Each Lambda function accesses sensitive information (such as external API keys) through **AWS Secrets Manager**, ensuring secrets are never hard-coded.

3.  **Data Layer (Database):**
    - **Database:** **Amazon DynamoDB** is used to store task data, progress, and user configuration. DynamoDB operates in **On-Demand** mode for automatic scaling and cost optimization.
    - **Data Security:** All data at rest in DynamoDB is encrypted using keys managed by **AWS KMS (Key Management Service)**, meeting the highest security standards.

4.  **Security and Authentication:**
    - **Authentication:** **Amazon Cognito** provides login mechanisms, session management, and Role-Based Access Control (RBAC) for users. Cognito also supports **Multi-Factor Authentication (MFA)** and SSO (Single Sign-On) integration.
    - **Edge Protection:** **AWS WAF** is placed in front of CloudFront to prevent Layer 7 DDoS attacks and other common security vulnerabilities (OWASP Top 10).

5.  **Deployment and Monitoring:**
    - **CI/CD DevSecOps:** Source code is stored on **GitLab** (as per the diagram) and automated via the **AWS CodePipeline/CodeBuild** chain. This process includes running **CodeGuru** to optimize code before deploying infrastructure via **CloudFormation**.
    - **Monitoring & Debugging:** **AWS CloudWatch Logs** collects detailed logs from all services. **AWS CloudWatch** uses these logs to set up automatic alerts for errors or incidents. **AWS X-Ray** provides an overview of transaction flow performance, aiding in debugging and latency optimization.

### **4. Technical Implementation**

The development of the TaskHub project is divided into two main parts—building the AWS serverless infrastructure and developing the task management platform—each including the following key implementation phases:

**Declared Development Phases**

**Phase 1: Design and Modeling (Month 1)**
- **Main Action:** Research Serverless/DevSecOps, select core services (Lambda, DynamoDB, API Gateway). Design detailed architecture diagrams and NoSQL data models.
- **Deliverables:** Architecture Diagram and Data Model Documentation.

**Phase 2: Infrastructure as Code Initialization (Month 2)**
- **Main Action:** Calculate detailed operating costs. Use AWS CDK to build IaC source code for platform services (S3, CloudFront, Cognito), ensuring environment reproducibility.
- **Deliverables:** Base AWS CDK source code and Operating Cost Report.

**Phase 3: DevSecOps Automation Setup (Month 2-3)**
- **Main Action:** Set up a complete CI/CD Pipeline (CodePipeline/CodeBuild). Integrate AWS CodeGuru and SAST tools to automate source code quality and security checks before deployment.
- **Deliverables:** Operational CodePipeline and automated security scanning process.

**Phase 4: Development and Deployment (Month 3-4)**
- **Main Action:** Develop functionality (Lambda Functions with TypeScript) and interface (Next.js). Perform Integration Testing between services. Deploy the official production release via Pipeline.
- **Deliverables:** TaskHub Beta Version (complete CRUD) and Testing Report.

**Technical Requirements**
- **Architecture and Tools:** The entire system is declared and managed using AWS CDK to ensure infrastructure consistency.
- **Technology:** Backend uses TypeScript/Node.js. Frontend uses Next.js (React).
- **Source Code Management:** Source code on GitLab, automated deployment via AWS CodePipeline.
- **Monitoring:** Configure CloudWatch, X-Ray, and CloudWatch Logs for performance monitoring and in-depth debugging.
- **Non-functional Requirements:** The system is located in Singapore (ap-southeast-1) to optimize speed in Vietnam, capable of scaling up to 50 users, and uses AWS KMS for data encryption.

### **5. Timeline & Key Milestones**

#### **Project Timeline**
- **Pre-internship (Month 0):** Prepare plans, research DevSecOps and AWS Serverless services.
- **Month 1:** Set up development environment, initiate AWS infrastructure and CI/CD pipeline.
- **Month 2:** Design architecture, develop core functionality, and automate security testing.
- **Month 3:** Integrate frontend-backend, develop testing, and launch the platform.
- **Post-launch:** Maintenance, performance evaluation, and expansion of advanced features.

### **6. Budget Estimate**

| Resource | Responsibility | Rate (USD) / Hour |
|---|---|---|
| Solution Architects [1 person] | System Architecture Design, API design, Database Schema, Technical Leadership | 6 |
| Engineers [3 person] | Backend Development, Frontend Development, Security Implement | 4 |
| Other (DevOps) [1 person] | CI/CD, Cloud Deployment, Monitoring, Security, Security Configuration | 4 |

| Project Phase | Solution Architects | Engineers | Other (Please specify) | Total Hours |
|---|---|---|---|---|
| System Design & Architecture | 20 | 10 | 0 | 30 |
| Backend Development | 10 | 80 | 0 | 90 |
| Frontend Development | 5 | 60 | 0 | 65 |
| Security & CI/CD Setup | 5 | 30 | 10 | 45 |
| Testing & Deployment | 5 | 30 | 0 | 35 |
| **Total Hours** | **45** | **210** | **10** | **265** |
| **Total Cost (USD)** | **270** | **840** | **40** | **800** |

Cost Contribution distribution between Partner, Customer, AWS:

| Party | Contribution (USD) | % Contribution of Total |
|---|---|---|
| Customer | 0 | 0 |
| Partner | 0 | 0 |
| AWS | 800 | 100 |

### **7. Risk Assessment**

**Risk Matrix**
- Network or AWS service disruption: Medium impact, medium probability.
- CI/CD deployment errors: High impact, low probability.
- Exceeding AWS budget: Medium impact, low probability.
- Security vulnerabilities: High impact, medium probability.
- Performance degradation under load: Medium impact, medium probability.

**Mitigation Strategies**
- Use AWS multi-region and monitor with CloudWatch/X-Ray.
- Test and review source code before deployment via **CodePipeline**.
- Set up cost alerts via AWS Budgets.
- Perform automated security scanning using **CodeBuild** (replacing GitHub Actions).

**Contingency Plan**
- Maintain a staging environment for quick recovery.
- Use CloudFormation and AWS Backup for configuration and data backup.

### **8. Expected Outcomes**

**Technical Improvements**
- **Comprehensive Automation:** Complete transition to automated **DevSecOps** process. New system deployment time takes **under 6 minutes**.
- **Performance Guarantee:** Fast application operation (API response **under 150ms**) and stability (**99.9% Uptime**) thanks to Serverless architecture.
- **Integrated Security:** Automated scanning and remediation of high-level security vulnerabilities during the Code Build process.
- **Scalability Ready:** The platform can scale to serve many users and handle large traffic volumes without structural changes.

**Long-term Value**
- **Technical Asset Creation:** Creation of a complete **AWS CDK/CloudFormation** codebase. This is a cost-optimized Serverless architecture template that can be reused for **other projects** by the team.
- **Robust Platform Foundation:** Establishment of a work management and development environment following industrial standards (DevSecOps), ready for future feature expansion.