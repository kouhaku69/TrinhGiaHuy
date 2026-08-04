---
title: "Blog 1"
date: "2025-07-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Develop and monitor a Spark application using existing data in Amazon S3 with Amazon SageMaker Unified Studio

by Amit Maindola and Abhilash Nagilla | on July 9, 2025 | in [Amazon SageMaker Data & AI Governance](https://aws.amazon.com/blogs/big-data/category/analytics/amazon-sagemaker-data-ai-governance/), [Amazon SageMaker Lakehouse](https://aws.amazon.com/blogs/big-data/category/analytics/amazon-sagemaker-lakehouse/), [Amazon SageMaker Unified Studio](https://aws.amazon.com/blogs/big-data/category/analytics/amazon-sagemaker-unified-studio/), [Analytics](https://aws.amazon.com/blogs/big-data/category/analytics/), [Technical How-to](https://aws.amazon.com/blogs/big-data/category/post-types/technical-how-to/)

---

Organizations face significant challenges managing their big data analytics workloads. Data teams struggle with fragmented development environments, complex resource management, inconsistent monitoring, and cumbersome manual scheduling processes. These issues lead to lengthy development cycles, inefficient resource utilization, reactive troubleshooting, and difficult-to-maintain data pipelines. These challenges are especially critical for enterprises processing terabytes of data daily for business intelligence (BI), reporting, and machine learning (ML). Such organizations need unified solutions that streamline their entire analytics workflow.

The next generation of [Amazon SageMaker](https://aws.amazon.com/sagemaker/) combined with [Amazon EMR](https://aws.amazon.com/emr/) in [Amazon SageMaker Unified Studio](https://aws.amazon.com/sagemaker/unified-studio/) addresses these pain points through an integrated development environment (IDE) where data workers can develop, test, and refine Spark applications in a consistent environment. [Amazon EMR Serverless](https://aws.amazon.com/emr/serverless/) reduces the cluster management burden by dynamically provisioning resources based on workload requirements, and integrated monitoring tools help teams quickly identify performance bottlenecks.

Integration with [Apache Airflow](https://airflow.apache.org/) through [Amazon Managed Workflows for Apache Airflow](https://aws.amazon.com/managed-workflows-for-apache-airflow/) (Amazon MWAA) provides robust scheduling capabilities, and the pay-only-for-resources-used model delivers significant cost savings.

In this post, we demonstrate how to develop and monitor a Spark application using existing data in [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3) with SageMaker Unified Studio.

---

## Solution overview

This solution uses SageMaker Unified Studio to execute and monitor Spark applications, highlighting integration capabilities. We demonstrate the following key steps:

1. Create an EMR Serverless compute environment for interactive applications using SageMaker Unified Studio
2. Create and configure Spark applications
3. Use [TPC-DS](https://www.tpc.org/tpcds/) data to build and run Spark applications using [Jupyter](https://jupyter.org/) notebooks in SageMaker Unified Studio
4. Monitor application performance and schedule periodic runs with Amazon MWAA integration  
5. Analyze results in SageMaker Unified Studio to optimize workflows

---

## Prerequisites

To follow this tutorial, you need the following requirements:

- **An AWS account** – If you don't have an account yet, you can [create one](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)
- **A SageMaker Unified Studio domain** – For instructions, see [Create an Amazon SageMaker Unified Studio domain – quick setup](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/create-domain-sagemaker-unified-studio-quick.html)
- **A demo project** – Create a demo project in your SageMaker Unified Studio domain. For instructions, see [Create a project](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/getting-started-create-a-project.html). For this example, we choose a profile with **All capabilities** in the project configuration section

---
## Adding EMR Serverless to compute

Complete the following steps to create an EMR Serverless compute environment for building Spark applications:

1. In SageMaker Unified Studio, open the project you created as a prerequisite and choose **Compute**
2. Choose **Data processing**, then choose **Add compute**
3. Choose **Create new compute resources**, then choose **Next**
![blog1](/images/3-Blog/BDB-5127-EMR-Add-Compute-1-New.png)
4. Choose **EMR Serverless**, then choose **Next**
![blog1](/images/3-Blog/BDB-5127-EMR-Serverless-2-New.png)
5. Enter a name for **Compute name**
6. For **Release label**, choose **emr-7.5.0**
7. For **Permission mode**, choose **Compatibility**
8. Choose **Add compute**

The EMR Serverless application initialization process takes a few minutes. After creation is complete, you can view the compute in SageMaker Unified Studio.
![blog1](/images/3-Blog/BDB-5127-Compute-3-1.png)

The above steps illustrate how you set up an Amazon EMR Serverless application in SageMaker Unified Studio to run interactive PySpark workloads. In the following steps, we will build and monitor the Spark application in the interactive JupyterLab workspace.

---

## Developing, monitoring, and debugging Spark applications in Jupyter notebooks

In this section, we build a Spark application using the TPC-DS dataset in SageMaker Unified Studio. With [Amazon SageMaker Data Processing](https://aws.amazon.com/sagemaker/data-processing/), you can focus on transforming and analyzing data without having to manage compute capacity or open-source applications, saving time and reducing costs.

SageMaker Data Processing provides a unified development experience from Amazon EMR, [AWS Glue](https://aws.amazon.com/glue), [Amazon Redshift](http://aws.amazon.com/redshift), [Amazon Athena](http://aws.amazon.com/athena), and Amazon MWAA in the same notebook and query interface. You can automatically provision capacity on [Amazon Elastic Compute Cloud](http://aws.amazon.com/ec2) (Amazon EC2) or EMR Serverless. Autoscaling rules manage changing compute demands to optimize performance and execution time.

### Implementation steps:

1. After completing the previous preparation steps, go to SageMaker Studio and open your project
2. Choose **Build**, then choose **JupyterLab**
The notebook takes about 30 seconds to initialize and connect to the workspace
3. Under the **Notebook** category, choose **Python 3 (ipykernel)**
4. In the first cell, next to **Local Python**, select the dropdown menu and choose **PySpark**
5. Choose the dropdown menu next to **Project.Spark** and select the **EMR-S Compute** compute
6. Run the following code to develop your Spark application. This example reads a 3 TB TPC-DS dataset in Parquet format from a public S3 bucket:
```python
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/store/").createOrReplaceTempView("store")
```

When the Spark session starts and execution logs begin to appear, you can explore the Spark UI and driver logs to debug and troubleshoot your Spark program.
![blog1](/images/3-Blog/BDB-5127-UI-Driver-4.png)
The following screenshot shows an example of the Spark user interface.
![blog1](/images/3-Blog/BDB-5127-SparkUI-5.png)
The following screenshot shows an example of driver logs.
![blog1](/images/3-Blog/BDB-5127-Spark-Driver-6.png)
The following screenshot shows the Executors tab, providing access to driver and executor logs.
![blog1](/images/3-Blog/BDB-5127-Executors-7.png)

7. Use the following code to read additional TPC-DS datasets. You can create temporary views and use the Spark UI to view the files being read. Refer to the appendix at the end of this article for details on how to use TPC-DS datasets in your buckets.
```python
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/item/").createOrReplaceTempView("item")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/store_sales/").createOrReplaceTempView("store_sales")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/date_dim/").createOrReplaceTempView("date_dim")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/customer/").createOrReplaceTempView("customer")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/catalog_sales/").createOrReplaceTempView("catalog_sales")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/web_sales/").createOrReplaceTempView("web_sales")

```
In each notebook cell, you can open Spark Job Progress to view the stages of jobs sent to EMR Serverless for a specific cell. You can see the completion time for each stage. If errors occur, you can check the logs, making troubleshooting more seamless.
![blog1](/images/3-Blog/BDB-5127-Spark-Job-Progress-8.png)
Since the files are partitioned based on date key columns, you can observe that Spark runs tasks in parallel to read the data.
![blog1](/images/3-Blog/BDB-5127-SparkJobs-9.png)
8. Next, get counts by date keys on data partitioned by time key using the following code:
```python
select count(1), ss_sold_date_sk from store_sales group by ss_sold_date_sk order by ss_sold_date_sk
```
![blog1](/images/3-Blog/BDB-5127-Notebook-Block-10.png)

## Monitoring jobs in Spark UI

In the **Jobs** tab of the Spark UI, you can view a list of completed or running jobs, with the following information:

- Action that triggered the job
- Execution time (e.g., 41 seconds, but times will vary)
- Number of stages and tasks — in this example, 2 stages and 3,428 tasks

You can select a job to see more details, especially about the stages. Our job has two stages; a new stage is created each time there is a shuffle. We have one stage to read data from each initial dataset, and one stage for aggregation.
![blog1](/images/3-Blog/BDB-5127-JobRun-11.gif)
In the next example, we run some TPC-DS SQL queries used for performance evaluation and benchmarking:
```sql
with frequent_ss_items as
 (select substr(i_item_desc,1,30) itemdesc,i_item_sk item_sk,d_date solddate,count(*) cnt
  from store_sales, date_dim, item
  where ss_sold_date_sk = d_date_sk
    and ss_item_sk = i_item_sk
    and d_year in (2000, 2000+1, 2000+2,2000+3)
  group by substr(i_item_desc,1,30),i_item_sk,d_date
  having count(*) >4),
 max_store_sales as
 (select max(csales) tpcds_cmax
  from (select c_customer_sk,sum(ss_quantity*ss_sales_price) csales
        from store_sales, customer, date_dim
        where ss_customer_sk = c_customer_sk
         and ss_sold_date_sk = d_date_sk
         and d_year in (2000, 2000+1, 2000+2,2000+3)
        group by c_customer_sk) x),
 best_ss_customer as
 (select c_customer_sk,sum(ss_quantity*ss_sales_price) ssales
  from store_sales, customer
  where ss_customer_sk = c_customer_sk
  group by c_customer_sk
  having sum(ss_quantity*ss_sales_price) > (95/100.0) *
    (select * from max_store_sales))
 select sum(sales)
 from (select cs_quantity*cs_list_price sales
       from catalog_sales, date_dim
       where d_year = 2000
         and d_moy = 2
         and cs_sold_date_sk = d_date_sk
         and cs_item_sk in (select item_sk from frequent_ss_items)
         and cs_bill_customer_sk in (select c_customer_sk from best_ss_customer)
      union all
      (select ws_quantity*ws_list_price sales
       from web_sales, date_dim
       where d_year = 2000
         and d_moy = 2
         and ws_sold_date_sk = d_date_sk
         and ws_item_sk in (select item_sk from frequent_ss_items)
         and ws_bill_customer_sk in (select c_customer_sk from best_ss_customer))) x
```
You can monitor Spark jobs in SageMaker Unified Studio in two ways. Jupyter notebooks provide basic monitoring, showing real-time job status and execution progress. For more detailed analysis, use the Spark UI. You can check specific stages, tasks, and execution plans. The Spark UI is particularly useful for troubleshooting performance issues and optimizing queries. You can track the expected number of stages, running tasks, and detailed duration of each task. This comprehensive view helps you understand resource usage and track job progress at a detailed level.

![blog1](/images/3-Blog/BDB-5127-Spark-TPCDS-12.gif)
In this section, we explained how you can use EMR Serverless compute in SageMaker Unified Studio to build interactive Spark applications. Through the Spark UI, interactive applications provide detailed task-level status, I/O and shuffle information, as well as links to corresponding task logs directly from the notebook, enabling a seamless debugging experience.

---

## Cleanup

To avoid ongoing charges in your AWS account, delete the resources you created in this tutorial:

1. Delete connections
2. Delete EMR jobs
3. Delete EMR output S3 buckets
4. Delete Amazon MWAA resources such as workflows and environments

---

## Conclusion

In this post, we demonstrated how the next generation of SageMaker, combined with EMR Serverless, provides a powerful solution for developing, monitoring, and scheduling Spark applications using data in Amazon S3. The integrated experience significantly reduces complexity by providing a unified development environment, automatic resource management, and comprehensive monitoring capabilities through the Spark UI, while maintaining cost efficiency with the pay-as-you-use model. For enterprises, this means faster time to insights, improved team collaboration, and reduced operational overhead, allowing data teams to focus on analytics rather than infrastructure management.

To get started, explore the [Amazon SageMaker Unified Studio User Guide](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/what-is-sagemaker-unified-studio.html), set up a project in your AWS environment, and discover how this solution can transform your organization's data analytics capabilities.

---

## Appendix
In the following sections, we discuss how to run scheduled workloads and provide details about the TPC-DS dataset for building Spark applications using EMR Serverless.

### Running scheduled workloads

In this section, we deploy JupyterLab notebooks and create workflows using Amazon MWAA. You can use workflows to orchestrate notebooks, querybooks, and many other things in your project repository. With workflows, you can define a set of tasks organized as a directed acyclic graph (DAG) that can run on a schedule you define. Implementation steps:

1. In SageMaker Unified Studio, choose **Build**, and under **Orchestration**, choose **Workflows**
![blog1](/images/3-Blog/BDB-5127-Workflow-13.png)
2. Choose **Create Workflow in Editor**
You will be directed to the JupyterLab notebook with a new DAG named `untitled.py` created in the `/src/workflows/dag` folder
3. Rename this notebook to `tpcds_data_queries.py`

4. You can reuse the existing template with the following updates:

a. Update line 17 with the schedule you want your code to run on
b. Update line 26 with your NOTEBOOK_PATH. This path should be in `src/<notebook_name>.ipynb`. Note that the dag_id name is auto-generated; you can name it as per your requirement.
![blog1](/images/3-Blog/BDB-5127-Spark-TPCDS-DagNotebk-14.jpg)
5. Choose **File** and **Save notebook**
To test, you can trigger a manual workload run
6. In SageMaker Unified Studio, choose **Build**, then under **Orchestration**, choose **Workflows**.
7. Choose your workflow, then choose **Run**.
You can monitor job success on the Runs tab.
![blog1](/images/3-Blog/BDB-5127-AirflowRun-15.png)
To debug notebook jobs by accessing the Spark UI in the Airflow job console, you must use [EMR Serverless Airflow Operators](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/using-airflow.html) to submit jobs. The link is available on the **Details** tab of the query.
This option has key limitations: it is not available for Amazon EMR on EC2, and SageMaker notebook job operators do not work.

You can configure the operator to create one-time links to the application UI and Spark output logs by passing `enable_application_ui_links=True` as a parameter. After the job starts running, these links are available on the Details tab of the corresponding task. If `enable_application_ui_links=False`, the links will appear but in a grayed-out state.

Make sure you have `emr-serverless:GetDashboardForJobRun` in [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) to create dashboard links.

Open the Airflow user interface for your task. The Spark user interface and history server dashboard options will appear on the Details tab, as shown in the following screenshot.

![blog1](/images/3-Blog/BDB-5127-AirflowUI-16.jpeg)

The screenshot illustrates the Jobs tab of the Spark UI.

![blog1](/images/3-Blog/BDB-5127-Airflow-SparkUI-17.jpeg)
---

## About the authors

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/Amit-Maindola.jpg" alt="Amit Maindola" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Amit Maindola</strong> is a Senior Data Architect focused on data engineering, analytics, and AI/ML at Amazon Web Services. He helps customers in their digital transformation journey and enables them to build highly scalable, robust, and secure cloud-based analytical solutions on AWS to gain timely insights and make critical business decisions.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/image045.jpg" alt="Abhilash Nagilla" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Abhilash Nagilla</strong> is a senior specialist solutions architect at Amazon Web Services (AWS), supporting public sector customers on their cloud journey with a focus on AWS data and AI services. Outside of work, Abhilash enjoys learning new technologies, watching movies, and traveling to new places.</p>
  </div>
</div>