---
title: "Week 11 Worklog"
date: "2026-08-03"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Build a data lake on AWS using self-prepared data with Glue DataBrew and Glue ETL.
* Get an overview of AWS data analytics services: ingest, transform, analyze, visualize.
* Build an interactive dashboard with Amazon QuickSight.
* Practice the Data Engineering Immersion Day: streaming, ETL, data lake automation.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - **Lab: Building a Data Lake with Your Own Data** <br>&emsp;+ Downloaded and cleaned a dataset with Glue DataBrew (profiling, transform) <br>&emsp;+ Ingested data with Glue, converted it to Parquet format, created a new Data Catalog <br>&emsp;+ Queried the data with Athena (join, CTAS, views, partitioning) and visualized it with QuickSight | 03/08/2026 | 03/08/2026 | <https://000070.awsstudygroup.com/> |
| 2   | - **Lab: AWS Data Analytics Services Overview** <br>&emsp;+ Ingested data with Kinesis Firehose, cataloged it with a Glue Crawler <br>&emsp;+ Transformed data with Glue (interactive sessions, Glue Studio, DataBrew) and with EMR <br>&emsp;+ Analyzed data with Athena and Kinesis Data Analytics, visualized it with QuickSight, served data through Lambda and Redshift | 04/08/2026 | 04/08/2026 | <https://000072.awsstudygroup.com/> |
| 3   | - **Lab: Getting Started with Amazon QuickSight** <br>&emsp;+ Updated the dataset, built the first dashboard (line chart, KPI, pie chart, pivot table) <br>&emsp;+ Improved the dashboard: formatting, additional charts, a detailed data table <br>&emsp;+ Added interactivity: filters, filter actions, navigation actions, and published the dashboard | 05/08/2026 | 05/08/2026 | <https://000073.awsstudygroup.com/> |
| 4   | - **Lab: Data Engineering Immersion Day** <br>&emsp;+ Detected clickstream anomalies with Amazon Managed Service for Apache Flink, streaming ETL with Glue, Kinesis, and MSK <br>&emsp;+ Ingested data with DMS, transformed data with Glue (data validation, incremental processing with Hudi) <br>&emsp;+ Queried/visualized with Athena, QuickSight, and Athena Federated Query; automated the data lake with Lake Formation | 06/08/2026 | 06/08/2026 | <https://000105.awsstudygroup.com/> |
| 5   | - **Practice & Review:** <br>&emsp;+ Reviewed the process of cleaning and ingesting self-prepared data with Glue DataBrew/Glue ETL <br>&emsp;+ Reviewed the services in a data analytics flow: Kinesis, Glue, EMR, Athena, Redshift <br>&emsp;+ Practiced building a dashboard with QuickSight and the steps of the Data Engineering Immersion Day again | 07/08/2026 | 07/08/2026 | <https://000070.awsstudygroup.com/>, <https://000072.awsstudygroup.com/>, <https://000073.awsstudygroup.com/>, <https://000105.awsstudygroup.com/> |

### 🏆 **Week 11 Achievements**

**1. Built a Data Lake with Self-Prepared Data**

* Cleaned and standardized a dataset with Glue DataBrew, converted it to Parquet format
* Queried data with Athena (join, CTAS, views, partitioning), visualized it with QuickSight

**2. Data Analytics Services Overview**

* Got a full picture of the ingest-transform-analyze-visualize flow with Kinesis, Glue, EMR, Athena, Redshift, Lambda

**3. Advanced Visualization and Data Engineering**

* Built and improved an interactive dashboard with Amazon QuickSight
* Practiced the Data Engineering Immersion Day: clickstream anomaly detection, streaming ETL, Lake Formation

### Week 11 Conclusion

Week 11 continued the Data & Analytics category, going deeper into preparing and cleaning data with Glue DataBrew instead of using pre-built sample data like the previous week. The data analytics overview lab helped tie together all the tools used and still to come in a typical data analytics pipeline. The QuickSight lab focused on visualization skills, while the Data Engineering Immersion Day combined nearly every technique covered during the week (streaming, ETL, Lake Formation) into a single exercise. The last day of the week was set aside to review the key steps before moving into the final week.
