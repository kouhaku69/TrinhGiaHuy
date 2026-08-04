---
title: "Week 10 Worklog"
date: "2026-07-27"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Finish the Container category: EKS Blueprints with CDK, CI/CD for EKS with CodePipeline, deploying Red Hat OpenShift Service on AWS (ROSA).
* Start the Data & Analytics category: building a data lake on AWS with Glue, Athena, and QuickSight.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Introduction to EKS Blueprints** <br>&emsp;+ Created a VPC, EC2 instance, and IAM Role, installed the required tools <br>&emsp;+ Created EKS Blueprints and a CDK project, built a cluster deployment pipeline <br>&emsp;+ Configured team access to the cluster through IaC, installed an add-on (Cluster Autoscaler), deployed a workload with ArgoCD | 27/07/2026 | 27/07/2026 | <https://000065.awsstudygroup.com/> |
| 2   | - **Lab: CI/CD on EKS with CodePipeline and GitHub** <br>&emsp;+ Created a Cloud9 workspace, installed Kubernetes tools, configured an IAM Role, created an EKS Cluster with eksctl <br>&emsp;+ Deployed a sample application to the cluster <br>&emsp;+ Created a CodePipeline (S3, service roles for CodePipeline/CodeBuild, configured RBAC), forked the source code, and verified the CI/CD flow | 28/07/2026 | 28/07/2026 | <https://000062.awsstudygroup.com/> |
| 3   | - **Lab: Red Hat OpenShift Service on AWS (ROSA)** <br>&emsp;+ Enabled ROSA, created an access key, installed and authenticated ROSA <br>&emsp;+ Created an OpenShift cluster on AWS, deployed an application to the cluster <br>&emsp;+ Set up basic CI/CD for ROSA using CodeCommit, CodeBuild, and CodePipeline | 29/07/2026 | 29/07/2026 | <https://000071.awsstudygroup.com/> |
| 4   | - **Lab: Data Lake on AWS** <br>&emsp;+ Created an IAM Role/Policy, created an S3 bucket and a Kinesis Delivery Stream to ingest sample data <br>&emsp;+ Created a Data Catalog with a Glue Crawler, verified the ingested data <br>&emsp;+ Analyzed the data with Athena and visualized it with QuickSight | 30/07/2026 | 30/07/2026 | <https://000035.awsstudygroup.com/> |
| 5   | - **Practice & Review:** <br>&emsp;+ Reviewed the EKS Blueprints architecture, managing teams/add-ons through IaC, and deploying workloads with ArgoCD <br>&emsp;+ Practiced the CI/CD flow for EKS with CodePipeline and the ROSA cluster again <br>&emsp;+ Reviewed the basic steps of building a data lake on AWS (ingest, catalog, query, visualize) | 31/07/2026 | 31/07/2026 | <https://000065.awsstudygroup.com/>, <https://000062.awsstudygroup.com/>, <https://000071.awsstudygroup.com/>, <https://000035.awsstudygroup.com/> |

### 🏆 **Week 10 Achievements**

**1. Completed the Container Category**

* Deployed an EKS cluster using the Blueprint model with CDK, managed teams and add-ons through Infrastructure as Code, deployed workloads with ArgoCD
* Built CI/CD for an application running on EKS using CodePipeline and GitHub
* Deployed and operated a Red Hat OpenShift (ROSA) cluster on AWS, set up basic CI/CD

**2. Started a Data Lake on AWS**

* Ingested sample data with a Kinesis Delivery Stream, created a Data Catalog with a Glue Crawler
* Queried the data with Athena, visualized it with QuickSight

### Week 10 Conclusion

Week 10 finished the entire Container category with three back-to-back labs on EKS Blueprints, CI/CD for EKS, and ROSA — all three centered on managing a Kubernetes cluster through Infrastructure as Code, differing mainly in the orchestration platform (plain EKS versus OpenShift). The data lake lab at the end of the week opened the Data & Analytics category, introducing the basic ingest-catalog-query-visualize flow that will be repeated and expanded in the coming weeks. The last day of the week was set aside to review everything covered, to make sure it was solid before moving into the next week.
