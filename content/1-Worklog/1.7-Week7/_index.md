---
title: "Week 7 Worklog"
date: "2026-07-06"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Finish the Performance track under Optimize: CI/CD for containers on EKS, hybrid storage (Storage Gateway, FSx), advanced DynamoDB design, workflow orchestration with Step Functions, storage performance measurement.
* Move into Cost Optimization: Savings Plan/Reserved Instance, cost visualization, cost analysis with Glue and Athena.
* Start the Modernize DevAx series (Monolith to Microservices): lift-and-shift a Java monolith, automated CI/CD, building a microservice with Lambda, splitting data out to DynamoDB, event-driven architecture with SQS/SNS/Kinesis.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: CI/CD for Amazon EKS with AWS CodePipeline** <br>&emsp;+ Created an IAM Role for the pipeline, modified aws-auth to grant CI/CD permissions <br>&emsp;+ Forked the sample repository, generated a GitHub access token <br>&emsp;+ Set up CodePipeline, triggered a new release, and tracked the automated deployment to the cluster <br>- **Lab: AWS Storage Gateway** <br>&emsp;+ Created an S3 bucket and an EC2 instance running Storage Gateway <br>&emsp;+ Created a Storage Gateway and File Shares <br>&emsp;+ Mounted the File Share from a simulated on-premises machine <br>- **Lab: Amazon FSx for Windows File Server** <br>&emsp;+ Built the environment with CloudFormation, created SSD and HDD Multi-AZ file systems <br>&emsp;+ Created new file shares, tested performance, enabled data deduplication and shadow copies <br>&emsp;+ Managed user sessions, storage quotas, and scaled throughput and storage capacity | 06/07/2026 | 06/07/2026 | <https://000152.awsstudygroup.com/>, <https://000024.awsstudygroup.com/>, <https://000025.awsstudygroup.com/> |
| 2   | - **Lab: Advanced Design Patterns for Amazon DynamoDB** <br>&emsp;+ Created a table and loaded sample data, measured capacity units and partitioning behavior <br>&emsp;+ Practiced Sequential Scan and Parallel Scan <br>&emsp;+ Built Global Secondary Indexes (write sharding, key overloading, sparse indexes), tried Composite Keys and Adjacency Lists <br>&emsp;+ Combined DynamoDB Streams with Lambda to replicate data to a replica table <br>- **Lab: Getting Started with AWS Step Functions** <br>&emsp;+ Deployed two sample Lambda functions, created the first state machine using a Task state <br>&emsp;+ Added a Choice state for branching logic and a Parallel state for concurrent execution <br>&emsp;+ Used waitForTaskToken to pause/resume the workflow, handled errors with Retry and Catch <br>- **Lab: Storage Performance Lab** <br>&emsp;+ Measured and optimized S3 throughput (prefixes, sync, small file operations, copy operations) <br>&emsp;+ Measured IOPS, I/O size, sync frequency, and multi-threading on EFS <br>&emsp;+ Compared performance across EFS storage classes and performance modes | 07/07/2026 | 07/07/2026 | <https://000039.awsstudygroup.com/>, <https://000047.awsstudygroup.com/>, <https://000068.awsstudygroup.com/> |
| 3   | - **Lab: Savings Plan, Reserved Instance, and Reserved DB Instance** <br>&emsp;+ Reviewed the Savings Plan types and compared them with Reserved Instances <br>&emsp;+ Reviewed Savings Plan recommendations, purchased a sample Savings Plan <br>&emsp;+ Reviewed Reserved Instance types and Reserved DB Instances for RDS <br>- **Lab: Cost Visualization** <br>&emsp;+ Viewed cost and usage by service and by account <br>&emsp;+ Viewed Savings Plan and Reserved Instance coverage, viewed elasticity <br>&emsp;+ Built a custom EC2 report, analyzed cost through Cost Explorer, and reviewed data transfer costs <br>- **Lab: Cost and Performance Analysis with AWS Glue and Amazon Athena** <br>&emsp;+ Prepared and built a database using a Glue Crawler <br>&emsp;+ Queried Cost & Usage Report data with Athena <br>&emsp;+ Analyzed cost by tag, cost allocation, and usage | 08/07/2026 | 08/07/2026 | <https://000042.awsstudygroup.com/>, <https://000034.awsstudygroup.com/>, <https://000040.awsstudygroup.com/> |
| 4   | - **Lab: Migrating the Monolith (TravelBuddy)** <br>&emsp;+ Created a Key Pair and CloudFormation stack, connected to the Windows instance and configured the database <br>&emsp;+ Ran the Java monolith application locally through Eclipse IDE <br>&emsp;+ Deployed the application to Elastic Beanstalk, updated the application, and queried the API <br>- **Lab: Configure App Auto-Release** <br>&emsp;+ Created an AWS CodeStar project, connected Eclipse IDE to CodeCommit <br>&emsp;+ Replaced the sample source code, deployed through CodePipeline, and diagnosed a deployment error <br>&emsp;+ Deployed a Windows Service to EC2 with CodeDeploy, monitored the service <br>- **Lab: Create a Microservice with Lambda** <br>&emsp;+ Created and tested a Lambda function locally, then uploaded it to AWS Lambda <br>&emsp;+ Wrote a function to process images: generating thumbnails for JPEG files and deleting non-image files, wired to an S3 trigger <br>&emsp;+ Packaged and automated the function deployment with SAM/CloudFormation, orchestrated through CodeStar | 09/07/2026 | 09/07/2026 | <https://000050.awsstudygroup.com/>, <https://000051.awsstudygroup.com/>, <https://000052.awsstudygroup.com/> |
| 5   | - **Lab: Refactor Your Data & Workflows** <br>&emsp;+ Created a new DynamoDB table and Global Secondary Index for a trip-search microservice <br>&emsp;+ Orchestrated the microservice through CodeStar, updated the target region and IAM policies for the API <br>&emsp;+ Built a calculator microservice using AWS Step Functions calling Lambda <br>- **Lab: Microservices Messaging & Eventing** <br>&emsp;+ Compared messaging patterns: SQS pub/sub (single and multiple subscribers, FIFO), SNS fan-out to multiple SQS queues <br>&emsp;+ Practiced a Kinesis publisher sending data to an SQS subscriber <br>&emsp;+ Implemented message streaming with Kinesis, verified data in ElasticSearch/Kibana | 10/07/2026 | 10/07/2026 | <https://000053.awsstudygroup.com/>, <https://000054.awsstudygroup.com/> |

### 🏆 **Week 7 Achievements**

**1. Completed the Performance Track (Optimize)**

* Set up CI/CD for an application running on EKS using CodePipeline
* Deployed hybrid storage with Storage Gateway and FSx for Windows File Server
* Applied advanced DynamoDB design techniques: GSI write sharding, key overloading, sparse indexes, DynamoDB Streams combined with Lambda
* Built a state machine with AWS Step Functions, handling branching, parallel execution, pause/resume, and error handling
* Measured and compared S3 and EFS performance across different configurations

**2. Cost Optimization**

* Compared and applied Savings Plans, Reserved Instances, and Reserved DB Instances
* Visualized cost and coverage of Savings Plans/Reserved Instances through Cost Explorer
* Analyzed the Cost & Usage Report with Glue and Athena, allocated cost by tag

**3. Started the Modernize Monolith-to-Microservices Series**

* Lifted-and-shifted a Java monolith to Elastic Beanstalk, set up CI/CD with CodeStar/CodePipeline/CodeDeploy
* Created a standalone microservice with Lambda, automated deployment with SAM/CloudFormation
* Split application data out of RDS into DynamoDB, built a workflow with Step Functions
* Compared messaging patterns between microservices: SQS, SNS, Kinesis

### Week 7 Conclusion

Week 7 closed out the Performance track under Optimize and opened the Modernize series centered on moving a monolith application to a microservices architecture. The first half of the week covered familiar storage and cost services, with the Step Functions and advanced DynamoDB labs requiring a clearer understanding of data flow than earlier labs. From the middle of the week, the TravelBuddy lab series walked through a complete modernization process: starting from a monolith running on EC2/Elastic Beanstalk, adding CI/CD, then progressively splitting parts out into microservices running on Lambda with data in DynamoDB, and finally choosing a communication mechanism between microservices (SQS, SNS, Kinesis) depending on ordering and latency requirements.
