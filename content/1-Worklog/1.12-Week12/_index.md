---
title: "Week 12 Worklog"
date: "2026-08-10"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objectives:

* Practice advanced Amazon Athena: Athena Spark and Athena Federated Query.
* Amazon RDS PostgreSQL: upgrades, performance monitoring, backup/recovery, scalability, high availability.
* Connect an application to RDS PostgreSQL using Secrets Manager.
* Get familiar with Machine Learning using Amazon SageMaker: feature engineering, train/tune/deploy a model.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Amazon Athena Workshop** <br>&emsp;+ Learned that Athena is a serverless analytics service built on Trino/Presto and Spark <br>&emsp;+ Practiced Athena Spark <br>&emsp;+ Practiced Athena Federated Query, connecting to an RDS PostgreSQL data source | 10/08/2026 | 10/08/2026 | <https://000106.awsstudygroup.com/> |
| 2   | - **Lab: AWS RDS PostgreSQL Foundation** <br>&emsp;+ Upgraded the minor/engine version of RDS PostgreSQL, verified the upgrade result <br>&emsp;+ Monitored performance with CloudWatch Logs/Alerts, generated load for testing and optimized load-test speed <br>&emsp;+ Configured automated backups, manual snapshots, Point-in-Time Restore, and AWS Backup <br>&emsp;+ Created a Read Replica for read scalability, performed vertical scaling, migrated to a Multi-AZ DB Cluster <br>&emsp;+ Created a Custom Parameter Group, configured Multi-AZ for high availability, and tested failover | 11/08/2026 | 11/08/2026 | <https://000115.awsstudygroup.com/> |
| 3   | - **Lab: AWS RDS PostgreSQL for Developers** <br>&emsp;+ Set up the environment, used AWS Secrets Manager to store connection information <br>&emsp;+ Wrote a simple database connection code sample and code that auto-fetches credentials to connect <br>&emsp;+ Deployed a user management application (AWS FCJ User Management System) connecting to RDS PostgreSQL | 12/08/2026 | 12/08/2026 | <https://000116.awsstudygroup.com/> |
| 4   | - **Lab: Amazon SageMaker Immersion Day** <br>&emsp;+ Created a SageMaker Studio, downloaded a sample dataset, analyzed and processed data correlations with Data Wrangler <br>&emsp;+ Transformed the data, stored it in Feature Store, exported the data to S3 <br>&emsp;+ Trained, tuned, and deployed an XGBoost model, evaluated its performance, and tried automatic model tuning | 13/08/2026 | 13/08/2026 | <https://000200.awsstudygroup.com/> |
| 5   | - **Practice & Review, Internship Wrap-up:** <br>&emsp;+ Reviewed Athena Spark and Athena Federated Query <br>&emsp;+ Reviewed RDS PostgreSQL operations (upgrades, backups, Multi-AZ) and connecting applications with Secrets Manager <br>&emsp;+ Reviewed the feature engineering and train/tune/deploy workflow on SageMaker <br>&emsp;+ Wrapped up the entire 12-week internship, from basic infrastructure to AWS's specialized services | 14/08/2026 | 14/08/2026 | <https://000106.awsstudygroup.com/>, <https://000115.awsstudygroup.com/>, <https://000116.awsstudygroup.com/>, <https://000200.awsstudygroup.com/> |

### 🏆 **Week 12 Achievements**

**1. Advanced Amazon Athena**

* Practiced Athena Spark and Athena Federated Query connecting to RDS PostgreSQL

**2. Operating Amazon RDS PostgreSQL**

* Upgraded the engine, monitored performance, configured backup/recovery and Point-in-Time Restore
* Scaled with Read Replicas and a Multi-AZ Cluster, tested failover for high availability

**3. Connecting Applications to RDS PostgreSQL**

* Used Secrets Manager to store connection information, deployed a real user management application

**4. Machine Learning with Amazon SageMaker**

* Practiced feature engineering with Data Wrangler, stored features with Feature Store
* Trained, tuned, and deployed an XGBoost model on SageMaker, evaluated model performance

### Week 12 Conclusion

Week 12 was the final week of the internship, focusing on Amazon RDS PostgreSQL from two angles — operations (upgrades, backups, high availability) and application development (connectivity, Secrets Manager) — alongside an advanced Athena lab at the start of the week. The SageMaker lab closed out the entire program with a complete Machine Learning exercise, from data preparation with Data Wrangler to training, tuning, and deploying an XGBoost model. The last day of the week served both as a review of the week's content and as a wrap-up of the full 12-week internship.
