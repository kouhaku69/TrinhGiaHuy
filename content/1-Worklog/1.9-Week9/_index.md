---
title: "Week 9 Worklog"
date: "2026-07-20"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Finish the Document Management System series: integrating the front-end with API Gateway, deploying with AWS SAM, configuring CloudFront/SSL, building a search feature with OpenSearch, CI/CD with CodePipeline, monitoring with CloudWatch/X-Ray.
* Serverless Web App Workshop: completing a theme park application with Lambda/API Gateway/SAM, building a complete serverless chat application with user authentication.
* Elastic Beanstalk: deploying an application with two Dev/Production environments and URL swapping, setting up CI/CD with CDK Pipelines.
* Finish the entire Modernize category and start Container: getting familiar with Kubernetes/Amazon EKS, converting a monolith application to microservices with Docker, ECS, and AWS Fargate, deploying WordPress with CodeDeploy.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Integrating the Front-end with API Gateway for the Document Management System** <br>&emsp;+ Deployed the front-end, configured API Gateway <br>&emsp;+ Tested the API with Postman and with the front-end <br>- **Lab: Deploying the Document Management System with AWS SAM** <br>&emsp;+ Deployed Cognito and an S3 bucket, deployed the front-end with SAM <br>&emsp;+ Configured the API and Lambda functions with SAM <br>&emsp;+ Tested the API with the front-end <br>- **Lab: Setting up CloudFront/SSL for the Document Management System** <br>&emsp;+ Created a domain and Hosted Zone <br>&emsp;+ Requested an SSL certificate from ACM <br>&emsp;+ Created a CloudFront distribution serving the application over HTTPS | 20/07/2026 | 20/07/2026 | <https://000135.awsstudygroup.com/>, <https://000136.awsstudygroup.com/>, <https://000137.awsstudygroup.com/> |
| 2   | - **Lab: Building a Search Feature with Amazon OpenSearch** <br>&emsp;+ Created a Lambda function to load data from a DynamoDB Stream into OpenSearch <br>&emsp;+ Created an OpenSearch instance and a search API <br>&emsp;+ Tested searching for documents by name, file type, and tag <br>- **Lab: CI/CD for the Document Management System with CodePipeline** <br>&emsp;+ Created a Git repository and pipeline for the backend (SAM) <br>&emsp;+ Created a separate Git repository and pipeline for the front-end <br>&emsp;+ Verified automatic build/deploy on code push <br>- **Lab: Monitoring the Document Management System with CloudWatch and X-Ray** <br>&emsp;+ Debugged a Lambda function using CloudWatch Logs <br>&emsp;+ Created a custom metric and a CloudWatch Alarm <br>&emsp;+ Traced requests with AWS X-Ray | 21/07/2026 | 21/07/2026 | <https://000138.awsstudygroup.com/>, <https://000139.awsstudygroup.com/>, <https://000140.awsstudygroup.com/> |
| 3   | - **Lab: Serverless with Lambda, API Gateway, and SAM (theme park application)** <br>&emsp;+ Deployed the front-end with AWS Amplify Console, deployed the backend (Lambda, API Gateway, DynamoDB) <br>&emsp;+ Loaded sample data into DynamoDB, tested the configuration <br>&emsp;+ Built a real-time ride wait-time feature and on-ride photo processing (Lambda functions for image processing and compositing) <br>- **Lab: Building a Complete Serverless Chat Application** <br>&emsp;+ Built a static chat page with S3, created an API with Lambda and API Gateway, configured CORS <br>&emsp;+ Moved conversation storage to DynamoDB, split the API into separate Lambda functions <br>&emsp;+ Added user authentication with Cognito (sign-up, sign-in, an authorizer for API Gateway), optimized load speed with CloudFront <br>- **Lab: Deploying an Application with Elastic Beanstalk** <br>&emsp;+ Created a Key Pair and an IAM instance role <br>&emsp;+ Created Development and Production environments in Elastic Beanstalk <br>&emsp;+ Updated the application in the Dev environment and swapped URLs between the two environments | 22/07/2026 | 22/07/2026 | <https://000066.awsstudygroup.com/>, <https://000117.awsstudygroup.com/>, <https://000112.awsstudygroup.com/> |
| 4   | - **Lab: CI/CD for Elastic Beanstalk with AWS CDK Pipelines** <br>&emsp;+ Created a GitHub repository, prepared the environment and a sample web application <br>&emsp;+ Defined the infrastructure and Elastic Beanstalk environment with CDK <br>&emsp;+ Created a CDK Pipeline stack, deployed it, and verified the automated deployment result <br>- **Lab: Introduction to Kubernetes and Amazon EKS** <br>&emsp;+ Reviewed Kubernetes architecture (Control Plane, Data Plane) and Amazon EKS architecture <br>&emsp;+ Prepared the workspace, installed Kubernetes tools, created an IAM Role, and launched a cluster with eksctl <br>&emsp;+ Deployed the Kubernetes Dashboard and a sample microservice application to the cluster, tested scaling the service | 23/07/2026 | 23/07/2026 | <https://000113.awsstudygroup.com/>, <https://000126.awsstudygroup.com/> |
| 5   | - **Lab: Deploying WordPress to EC2 with AWS CodeDeploy** <br>&emsp;+ Created an access key, instance profile, and service role <br>&emsp;+ Launched an EC2 instance, installed the CodeDeploy Agent <br>&emsp;+ Created an S3 bucket and a Deployment Group, and deployed the WordPress application <br>- **Lab: Converting a Monolith to Microservices with Docker, ECS, and AWS Fargate** <br>&emsp;+ Reviewed Docker concepts and container images <br>&emsp;+ Containerized a sample monolith application (Mythical Mysfits) <br>&emsp;+ Deployed the container with AWS Fargate, configured an ALB and ECS Service, and progressively split it into separate microservices | 24/07/2026 | 24/07/2026 | <https://000091.awsstudygroup.com/>, <https://000067.awsstudygroup.com/> |

### 🏆 **Week 9 Achievements**

**1. Completed the Document Management System Series**

* Connected the front-end to the backend through API Gateway, redeployed the entire application with AWS SAM
* Set up a custom domain/SSL with Route 53, ACM, and CloudFront
* Built a document search feature with OpenSearch combined with DynamoDB Streams
* Set up separate CI/CD for the backend and front-end, monitored the application with CloudWatch/X-Ray

**2. Serverless Web App Workshop**

* Completed a multi-feature serverless application (static data, real-time wait times, image processing) for a theme park scenario
* Built a serverless chat application from scratch: from static S3 data to a split microservice API with Cognito authentication

**3. Elastic Beanstalk**

* Deployed an application with two Dev/Production environments, practiced swapping URLs to switch versions
* Built a CI/CD pipeline for Elastic Beanstalk using AWS CDK

**4. Finished the Modernize Category, Got Familiar with Kubernetes/EKS**

* Reviewed Kubernetes and Amazon EKS architecture, launched a cluster with eksctl, and deployed a sample microservice application
* Deployed WordPress to EC2 with CodeDeploy, completing the entire Modernize category

**5. Started Container Services**

* Containerized a monolith application with Docker
* Deployed the container with ECS and AWS Fargate, started splitting the application into standalone microservices

### Week 9 Conclusion

Week 9 closed out the entire Modernize category, starting by finishing the Document Management System series (front-end, SAM, CloudFront/SSL, OpenSearch search, CI/CD, monitoring), then moving to two standalone workshops — a theme park application and a serverless chat application — both repeating the Lambda/API Gateway/DynamoDB pattern at a larger scale with added user authentication. The Elastic Beanstalk section showed a different deployment approach compared to serverless, using two parallel environments to reduce risk during updates. The end of the week shifted into Container: getting familiar with Kubernetes/EKS architecture, deploying WordPress with CodeDeploy, and containerizing a sample monolith application to start splitting it into microservices running on ECS/Fargate.
