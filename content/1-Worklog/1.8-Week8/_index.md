---
title: "Week 8 Worklog"
date: "2026-07-13"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Finish the DevAx series (Monolith to Microservices): authentication for a Single Page Application, integrating AWS AI services (Polly, Rekognition, Lex).
* Complete the entire Serverless Book Store series: building a Lambda function that processes images and writes to DynamoDB, a front-end calling API Gateway, deploying with AWS SAM, authenticating with Cognito, setting up SSL/custom domains, processing orders with SQS/SNS, CI/CD with CodePipeline, monitoring with CloudWatch/X-Ray, and getting familiar with AppSync/GraphQL.
* Start the Document Management System series: creating a DynamoDB table and Lambda functions to manage documents, using Amplify for authentication and file storage.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Authentication for a Single Page Application** <br>&emsp;+ Created a DynamoDB table, manually built and deployed a serverless microservice <br>&emsp;+ Created and exposed an API through API Gateway, deployed through CodeStar/CI-CD <br>&emsp;+ Added authentication to the SPA with a Cognito User Pool, configured sign-up/sign-in, measured application performance with X-Ray <br>- **Lab: Experiencing Amazon AI Services** <br>&emsp;+ Used Amazon Polly to convert text to speech through the Console, CLI, and Java SDK, generated speech marks <br>&emsp;+ Used Amazon Rekognition for object detection and facial recognition in images <br>&emsp;+ Built a chatbot with Amazon Lex, wired a Lambda function to handle conversation for the TravelBuddy application <br>- **Lab: Getting Started with AWS Lambda for the Serverless Bookstore application** <br>&emsp;+ Created a Lambda function that processes images when triggered by an S3 upload event <br>&emsp;+ Created an IAM Policy for Lambda to access S3, tested the function <br>&emsp;+ Created a DynamoDB table and wrote data from Lambda | 13/07/2026 | 13/07/2026 | <https://000055.awsstudygroup.com/>, <https://000056.awsstudygroup.com/>, <https://000078.awsstudygroup.com/> |
| 2   | - **Lab: Building a Front-end that Calls API Gateway** <br>&emsp;+ Deployed the front-end, created a DynamoDB table for application data <br>&emsp;+ Wrote Lambda functions to write/list/delete data <br>&emsp;+ Configured methods and CORS on API Gateway, tested the API with Postman and with the front-end <br>- **Lab: Deploying a Serverless Application with AWS SAM** <br>&emsp;+ Rebuilt the entire application from the previous lab using SAM syntax (YAML) <br>&emsp;+ Deployed the front-end, Lambda functions (list/write/delete/resize image), and configured API Gateway (GET/POST/DELETE) through SAM <br>&emsp;+ Re-tested the API with Postman and the front-end <br>- **Lab: Authentication with Amazon Cognito for a Serverless Application** <br>&emsp;+ Created a Cognito User Pool <br>&emsp;+ Created an API and Lambda function requiring authentication <br>&emsp;+ Tested the sign-in/sign-up flow on the front-end | 14/07/2026 | 14/07/2026 | <https://000079.awsstudygroup.com/>, <https://000080.awsstudygroup.com/>, <https://000081.awsstudygroup.com/> |
| 3   | - **Lab: Setting up SSL for a Serverless Application** <br>&emsp;+ Created a domain and Hosted Zone on Route 53 <br>&emsp;+ Requested an SSL certificate from AWS Certificate Manager <br>&emsp;+ Created a CloudFront distribution serving the application over HTTPS with a custom domain <br>- **Lab: Processing Orders with SQS and SNS** <br>&emsp;+ Created an SQS queue and an SNS topic <br>&emsp;+ Created a DynamoDB table to store orders and Lambda functions for checkout/management/handling/deleting orders <br>&emsp;+ Tested the flow: an order is placed into the queue, SNS notifies the admin, the admin handles or deletes the order <br>- **Lab: CI/CD for a Serverless Application with AWS CodePipeline** <br>&emsp;+ Created a Git repository and pipeline for the backend (SAM) <br>&emsp;+ Created a separate Git repository and pipeline for the front-end <br>&emsp;+ Verified automatic build/deploy on new code pushes | 15/07/2026 | 15/07/2026 | <https://000082.awsstudygroup.com/>, <https://000083.awsstudygroup.com/>, <https://000084.awsstudygroup.com/> |
| 4   | - **Lab: Monitoring a Serverless Application with CloudWatch and X-Ray** <br>&emsp;+ Debugged a Lambda function using CloudWatch Logs <br>&emsp;+ Created a custom metric and a CloudWatch Alarm for alerting <br>&emsp;+ Traced requests across the application with AWS X-Ray <br>- **Lab: Getting Started with AWS AppSync** <br>&emsp;+ Learned how AppSync works together with GraphQL <br>&emsp;+ Configured DynamoDB resolvers: writing, reading, updating, deleting, scanning, and querying data through GraphQL <br>&emsp;+ Created and queried a nested complex object | 16/07/2026 | 16/07/2026 | <https://000085.awsstudygroup.com/>, <https://000086.awsstudygroup.com/> |
| 5   | - **Lab: Building the Foundation for a Document Management System** <br>&emsp;+ Created a DynamoDB table to store file information <br>&emsp;+ Wrote Lambda functions to list, upload, and delete documents <br>&emsp;+ Tested each Lambda function <br>- **Lab: Using Amplify for Authentication and Storage** <br>&emsp;+ Configured Amplify Authentication based on Cognito <br>&emsp;+ Configured Amplify Storage to upload/manage files on S3 <br>&emsp;+ Set up access levels (private/protected/public) for each file type | 17/07/2026 | 17/07/2026 | <https://000133.awsstudygroup.com/>, <https://000134.awsstudygroup.com/> |

### 🏆 **Week 8 Achievements**

**1. Completed the DevAx Series**

* Added Cognito authentication to a Single Page Application, measured performance with X-Ray
* Integrated AI services (Polly, Rekognition, Lex) into the TravelBuddy application, built a conversational chatbot

**2. Completed the Entire Serverless Book Store Series**

* Built a backend with Lambda, S3, DynamoDB, and a front-end calling API Gateway
* Rebuilt the entire application with AWS SAM, added Cognito authentication
* Set up a custom domain and SSL with Route 53, ACM, and CloudFront
* Processed orders with SQS/SNS, set up separate CI/CD pipelines for backend and front-end with CodePipeline
* Monitored the application with CloudWatch/X-Ray, got familiar with AppSync and GraphQL

**3. Started the Document Management System Series**

* Built the foundation for storing document metadata with DynamoDB and Lambda
* Configured Amplify for user authentication and file storage management

### Week 8 Conclusion

Week 8 had two parts: finishing the DevAx series with SPA authentication and AI service integration, then moving into the Serverless Book Store series, which goes from a simple Lambda/DynamoDB application to a full system with SAM, Cognito, a custom domain/SSL, SQS/SNS, CI/CD, and monitoring. This series is highly iterative — each step adds a layer on top of the basic serverless architecture built at the start of the week, so the hardest part was keeping the Lambda functions, API Gateway, and front-end in sync across each step. The last two labs of the week opened the Document Management System series, reusing almost the same Lambda/DynamoDB pattern but applied to a file-management problem with Amplify.
