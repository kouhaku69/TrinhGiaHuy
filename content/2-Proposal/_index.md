---
title: "Proposal"
date: "2025-12-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Balan Coffee & Roastery – Modernizing the Online Coffee Platform on AWS

### **1. Executive Summary**

Balan Coffee & Roastery is a project to modernize the existing coffee sales website by deploying it on the AWS platform, aiming to improve operability, security, and system observability.

The application is deployed on a single **Amazon EC2** instance using **Docker Compose** to run the frontend and backend containers together, combined with **Amazon CloudFront** as the content delivery layer for all incoming user requests.

The backend connects to fully managed AWS services including **Amazon RDS (PostgreSQL)**, **Amazon S3**, **Amazon Cognito**, **Amazon SES**, **Amazon Bedrock**, and **AWS Secrets Manager**, ensuring that data, authentication, email delivery, and AI features are processed securely and separately from the application layer.

Operational visibility is ensured through **Amazon CloudWatch** combined with the **CloudWatch Agent** installed directly on the EC2 instance.

### **2. Problem Statement**

**Problem:**

- The current Balan Coffee system is built and operated in a traditional way, without leveraging AWS managed services, making it difficult to scale, back up data, and monitor system health.
- Sensitive information (API keys, database connection details) configured manually or stored directly in source code carries a high security risk.
- The customer experience is limited due to the lack of smart product recommendations based on user behavior or preferences.

**Solution:**

- Containerize the frontend and backend using **Docker Compose**, deployed on **Amazon EC2**, simplifying operations within the scope of the workshop while retaining full control over the infrastructure.
- Use **Amazon RDS (PostgreSQL)** in a private subnet to ensure relational data is managed, automatically backed up, and isolated from direct Internet access.
- Integrate **Amazon Cognito** for user authentication, **AWS Secrets Manager** to securely manage sensitive configuration information, **Amazon S3** to store product files/images, **Amazon SES** to send transactional emails, and **Amazon Bedrock** to build an AI-powered chatbot/product recommendation feature.
- The entire system is centrally monitored via **Amazon CloudWatch**.

**Benefits and Return on Investment (ROI)**

The solution helps Balan Coffee & Roastery transition from a manually operated model to a clearly structured model on AWS, separating the application, data, and security layers. Using managed services (RDS, S3, Cognito, SES, Bedrock) reduces the amount of manual operational work, while infrastructure costs at the workshop stage mostly fall within AWS's free tier/low-cost limits (small EC2 instance, single-AZ RDS, low S3 storage). This is a foundation that can gradually scale up to a full production architecture (ECS, Auto Scaling, Load Balancer) without needing to be redesigned from scratch.

### **3. Solution Architecture**

The Balan Coffee & Roastery platform is built on a container deployment model on **Amazon EC2**, combined with AWS managed services to handle data, authentication, storage, email delivery, and AI features. The architecture ensures a clear separation between the content delivery layer, the application layer (public subnet), and the data layer (private subnet).

Main processing flow: users access the system via **HTTPS**, requests are distributed by **Amazon CloudFront** to the **Internet Gateway**, then forwarded to **Amazon EC2** located in the VPC's public subnet. On EC2, **Docker** runs the **Frontend** and **Backend** containers simultaneously. The backend communicates with **Amazon RDS (PostgreSQL)** located in the private subnet (TCP port 5432, controlled by a Security Group), and also calls **Amazon S3** (file/image storage), **Amazon Bedrock** (AI product recommendations/chatbot), **Amazon SES** (email delivery), and **Amazon Cognito** (user authentication).

All EC2 activity is collected by the **CloudWatch Agent** (system metrics, application logs) and sent to **Amazon CloudWatch** for display on a Dashboard and to set up alerts.

The overall architecture is described in detail in the diagram below:

![Balan Coffee & Roastery Solution Architecture](/images/2-Proposal/architecture.jpg)

### AWS Services Used

1.  **Amazon EC2:** Compute server, running the application via Docker.
2.  **Docker (Docker Compose):** Packages and runs the frontend and backend containers together.
3.  **Amazon CloudFront:** Global content delivery, HTTPS termination, and improved page-load performance.
4.  **Amazon RDS (PostgreSQL):** Relational database, storing all business data (products, orders, users).
5.  **Amazon S3:** Object storage (product images, static files).
6.  **Amazon Cognito:** User authentication and authorization.
7.  **Amazon SES:** Sends transactional/notification emails.
8.  **Amazon Bedrock:** Provides AI capability for the chatbot and product recommendation feature.
9.  **AWS Secrets Manager:** Securely stores and manages sensitive configuration information (environment variables, connection keys).
10. **Amazon CloudWatch (and CloudWatch Agent):** Collects metrics and logs, and displays a system monitoring dashboard.
11. **Amazon VPC (Public/Private Subnet, Internet Gateway, Security Group):** Network design that isolates the data layer from direct Internet access.

### **Component Design**

1.  **User Interface Layer (Frontend):**

    - Runs as a Docker container on EC2, serving the web interface to customers.
    - Distributed globally via **Amazon CloudFront**, ensuring fast load times and always using HTTPS.

2.  **Business Logic Layer (Backend):**

    - Runs as a separate Docker container on the same EC2 instance, handling all business logic (products, orders, authentication, AI integration).
    - Accesses sensitive configuration information (RDS connection, Bedrock/SES API keys, etc.) through **AWS Secrets Manager**, rather than storing it directly in source code.

3.  **Data Layer (Database):**

    - **Amazon RDS PostgreSQL** is placed in the private subnet, only allowing connections from the backend EC2's Security Group (port 5432), with no exposure to the Internet.
    - Responsible for storing all transaction, product, and user data.

4.  **Security and Authentication:**

    - **Amazon Cognito** provides sign-up/sign-in mechanisms and JWT-based authentication for users.
    - **Security Group** tightly controls traffic between components (only the backend can access RDS).
    - **AWS Secrets Manager** ensures sensitive information is rotated and centrally managed.
    - All user traffic goes through **HTTPS** via CloudFront.

5.  **Monitoring and Operations:**

    - **CloudWatch Agent** installed on EC2 collects infrastructure metrics (CPU, RAM, disk) and application logs.
    - **Amazon CloudWatch** aggregates this data for Dashboard display and can be configured to trigger alerts when anomalies are detected.

### **4. Technical Implementation**

The project is implemented in the following main phases:

**Phase 1: Architecture Design**

- Research the container deployment model on EC2, select suitable AWS services (RDS, S3, Cognito, SES, Bedrock, Secrets Manager).
- Design the VPC network diagram (public/private subnet) and Security Group rules.
- **Deliverable:** Solution Architecture diagram.

**Phase 2: AWS Infrastructure Initialization**

- Create the VPC, subnets, Internet Gateway, and Security Groups.
- Launch EC2, configure Docker/Docker Compose, initialize the RDS PostgreSQL instance and S3 bucket.
- **Deliverable:** AWS infrastructure ready for application deployment.

**Phase 3: Managed Service Integration**

- Integrate Cognito for authentication, Secrets Manager for sensitive configuration, SES for email delivery, and Bedrock for the AI recommendation/chatbot feature.
- **Deliverable:** A complete backend with full AWS integration.

**Phase 4: Deployment and Monitoring**

- Deploy the frontend/backend via Docker Compose on EC2, configure CloudFront.
- Install the CloudWatch Agent, build a monitoring dashboard, and test the entire application flow.
- **Deliverable:** A fully operational system with monitoring, ready for demo.

**Technical Requirements**

- **Infrastructure:** VPC with a public subnet (EC2, Internet Gateway) and a private subnet (RDS), with Security Groups controlling access.
- **Technology:** Docker/Docker Compose for containerization; PostgreSQL for the database.
- **Security:** Cognito for authentication (JWT), Secrets Manager for configuration management, HTTPS via CloudFront.
- **Monitoring:** CloudWatch Agent + CloudWatch (Metrics, Logs, Dashboard).
- **Deployment Region:** ap-southeast-1 (Singapore) to optimize access speed in Vietnam.

### **5. Timeline & Key Milestones**

- **Preparation Phase:** Research the architecture, divide work according to each team member's role.
- **Phase 1:** Design the architecture and network infrastructure (VPC, subnets, Security Groups).
- **Phase 2:** Initialize AWS infrastructure (EC2, RDS, S3) and deploy the base containers.
- **Phase 3:** Integrate managed services (Cognito, Secrets Manager, SES, Bedrock).
- **Phase 4:** Finalize monitoring (CloudWatch), testing, and product demo.

### **6. Team Assignment**

| Member | Scope of Responsibility |
|---|---|
| Tran Minh Quan | AWS infrastructure, deployment (VPC, EC2, Docker), database (RDS), and monitoring (CloudWatch) |
| Nguyen Vo Duy Tuan | User authentication (Cognito) and secret configuration management (Secrets Manager) |
| Pham Nguyen Quang Minh | File storage (S3), AI feature (Bedrock), and backend API integration |
| Trinh Gia Huy | Technical documentation, architecture design, and product demo |

### **7. Risk Assessment**

**Risk Matrix**

- AWS service disruption or outage: Medium impact, low probability.
- Single EC2 instance failure (no Auto Scaling/Load Balancer): High impact, medium probability.
- Misconfigured Security Group exposing the RDS port: High impact, low probability.
- Exceeding the free-tier usage limits for Bedrock/SES: Medium impact, medium probability.
- Data loss due to lack of periodic backups: High impact, low probability.

**Mitigation Strategies**

- Set up CloudWatch alerts when EC2/RDS resources exceed thresholds.
- Regularly review Security Group configurations, only opening necessary ports.
- Enable automated backups for RDS.
- Monitor Bedrock/SES usage via CloudWatch to avoid exceeding limits.

**Contingency Plan**

- Document infrastructure configuration so it can be quickly rebuilt if needed.
- Can be extended to ECS/Auto Scaling when higher availability is required (see Expected Outcomes section).

### **8. Expected Outcomes**

**Technical Improvements**

- **Infrastructure Modernization:** Transition from manual operations to a containerized model on AWS with managed services (RDS, S3, Cognito, SES, Bedrock).
- **Improved Security:** Database isolated in a private subnet, centralized secret management via Secrets Manager, authentication via Cognito.
- **Centralized Monitoring:** The entire system is monitored via CloudWatch, enabling early issue detection.
- **Better Customer Experience:** Integrated AI-powered product recommendation/chatbot feature via Amazon Bedrock.

**Long-term Value**

- **Scalable Foundation:** The current architecture is a stepping stone to scale up to **Amazon ECS, Application Load Balancer, Auto Scaling Group, Route 53, AWS WAF, AWS Certificate Manager**, and a **CI/CD Pipeline** in the future, in line with the direction outlined in the solution architecture document.
- **Technical Asset:** The architecture documentation and infrastructure configuration can be reused as a foundation for similar projects by the team.
