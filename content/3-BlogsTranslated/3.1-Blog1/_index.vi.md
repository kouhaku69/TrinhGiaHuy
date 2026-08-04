---
title: "Blog 1"
date: "2025-07-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Phát triển và giám sát ứng dụng Spark sử dụng dữ liệu hiện có trong Amazon S3 với Amazon SageMaker Unified Studio

Tác giả:Amit Maindola và Abhilash Nagilla | Ngày: 09 tháng 7 năm 2025 | Danh mục: [Amazon SageMaker Data & AI Governance](https://aws.amazon.com/blogs/big-data/category/analytics/amazon-sagemaker-data-ai-governance/), [Amazon SageMaker Lakehouse](https://aws.amazon.com/blogs/big-data/category/analytics/amazon-sagemaker-lakehouse/), [Amazon SageMaker Unified Studio](https://aws.amazon.com/blogs/big-data/category/analytics/amazon-sagemaker-unified-studio/), [Analytics](https://aws.amazon.com/blogs/big-data/category/analytics/), [Technical How-to](https://aws.amazon.com/blogs/big-data/category/post-types/technical-how-to/)

---

Các tổ chức thường đối mặt với thách thức đáng kể trong quản lý khối lượng công việc phân tích dữ liệu lớn (big data analytics). Nhóm dữ liệu gặp khó khăn với môi trường phát triển phân mảnh, quản lý tài nguyên phức tạp, giám sát không nhất quán và quy trình lập lịch thủ công rườm rà. Những vấn đề này dẫn đến chu kỳ phát triển kéo dài, sử dụng tài nguyên không hiệu quả, khắc phục sự cố mang tính phản ứng và pipeline dữ liệu khó duy trì. Những thách thức này đặc biệt nghiêm trọng đối với các doanh nghiệp xử lý hàng terabyte dữ liệu hàng ngày cho business intelligence (BI), báo cáo và machine learning (ML). Những tổ chức như vậy cần các giải pháp thống nhất để hợp lý hóa toàn bộ quy trình phân tích.

Thế hệ tiếp theo của [Amazon SageMaker](https://aws.amazon.com/sagemaker/) kết hợp với [Amazon EMR](https://aws.amazon.com/emr/) trong [Amazon SageMaker Unified Studio](https://aws.amazon.com/sagemaker/unified-studio/) giải quyết những điểm đau này thông qua một môi trường phát triển tích hợp (IDE) nơi các nhân viên làm việc dữ liệu có thể phát triển, thử nghiệm và tinh chỉnh ứng dụng Spark trong một môi trường nhất quán. [Amazon EMR Serverless](https://aws.amazon.com/emr/serverless/) giảm bớt gánh nặng quản lý cụm bằng cách cấp phát tài nguyên động dựa vào yêu cầu khối lượng công việc, và các công cụ giám sát tích hợp giúp các nhóm nhanh chóng xác định điểm nghẽn hiệu năng.

Việc tích hợp với [Apache Airflow](https://airflow.apache.org/) thông qua [Amazon Managed Workflows for Apache Airflow](https://aws.amazon.com/managed-workflows-for-apache-airflow/) (Amazon MWAA) mang lại khả năng lập lịch mạnh mẽ, và mô hình trả phí cho tài nguyên sử dụng ("pay-only-for-resources-used") đem lại tiết kiệm chi phí đáng kể.

Trong bài viết này, chúng tôi minh họa cách phát triển và giám sát một ứng dụng Spark sử dụng dữ liệu hiện có trong [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3)** bằng SageMaker Unified Studio.

---

## Tổng quan giải pháp

Giải pháp này sử dụng SageMaker Unified Studio để thực thi và giám sát ứng dụng Spark, làm nổi bật các khả năng tích hợp. Chúng tôi trình bày các bước chính sau đây:

1. Tạo môi trường compute EMR Serverless cho ứng dụng tương tác bằng SageMaker Unified Studio
2. Tạo và cấu hình ứng dụng Spark
3. Sử dụng dữ liệu [TPC-DS](https://www.tpc.org/tpcds/) để xây dựng và chạy ứng dụng Spark bằng notebook [Jupyter](https://jupyter.org/) trong SageMaker Unified Studio
4. Giám sát hiệu năng ứng dụng và lập lịch chạy định kỳ với tích hợp Amazon MWAA
5. Phân tích kết quả trong SageMaker Unified Studio để tối ưu hóa quy trình làm việc

---

## Yêu cầu chuẩn bị

Để theo dõi bài hướng dẫn này, bạn cần các yêu cầu sau:

- **Một tài khoản AWS** – Nếu bạn chưa có tài khoản, bạn có thể [create one](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)
- **Một domain SageMaker Unified Studio** – Để biết hướng dẫn, xem [Create an Amazon SageMaker Unified Studio domain – quick setup](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/create-domain-sagemaker-unified-studio-quick.html)
- **Một dự án demo** – Tạo một dự án demo trong domain SageMaker Unified Studio của bạn. Để biết hướng dẫn, hãy xem [Create a project](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/getting-started-create-a-project.html). Đối với ví dụ này, chúng tôi chọn profile có **All capabilities** trong phần cấu hình dự án

---

## Thêm EMR Serverless vào compute

Hoàn thành các bước sau để tạo môi trường compute EMR Serverless để xây dựng ứng dụng Spark:

1. Trong SageMaker Unified Studio, mở dự án bạn đã tạo làm yêu cầu trước và chọn **Compute**
2. Chọn **Data processing**, sau đó chọn **Add compute**
3. Chọn **Create new compute resources**, rồi chọn **Next**
![blog1](/images/3-Blog/BDB-5127-EMR-Add-Compute-1-New.png)
4. Chọn **EMR Serverless**, rồi chọn **Next**
![blog1](/images/3-Blog/BDB-5127-EMR-Serverless-2-New.png)
5. Nhập tên cho **Compute name**
6. Với **Release label**, chọn **emr-7.5.0**
7. Với **Permission mode**, chọn **Compatibility**
8. Chọn **Add compute**

Quá trình khởi tạo ứng dụng EMR Serverless mất vài phút. Sau khi tạo xong, bạn có thể xem compute trong SageMaker Unified Studio.
![blog1](/images/3-Blog/BDB-5127-Compute-3-1.png)

Các bước trên minh họa cách bạn thiết lập một ứng dụng Amazon EMR Serverless trong SageMaker Unified Studio để chạy các workloads PySpark tương tác. Trong các bước tiếp theo, chúng tôi sẽ xây dựng và theo dõi ứng dụng Spark trong không gian làm việc JupyterLab tương tác.

---

## Phát triển, giám sát và gỡ lỗi ứng dụng Spark trong notebook Jupyter

Trong phần này, chúng tôi xây dựng ứng dụng Spark sử dụng dataset TPC-DS trong SageMaker Unified Studio. Với [Amazon SageMaker Data Processing](https://aws.amazon.com/sagemaker/data-processing/), bạn có thể tập trung vào chuyển đổi và phân tích dữ liệu mà không cần quản lý công suất compute hay ứng dụng nguồn mở, tiết kiệm thời gian và giảm chi phí. 

SageMaker Data Processing cung cấp trải nghiệm phát triển thống nhất từ Amazon EMR, [AWS Glue](https://aws.amazon.com/glue), [Amazon Redshift](http://aws.amazon.com/redshift), [Amazon Athena](http://aws.amazon.com/athena), và Amazon MWAA trong cùng một notebook và giao diện truy vấn. Bạn có thể tự động cấp phát công suất trên [Amazon Elastic Compute Cloud](http://aws.amazon.com/ec2) (Amazon EC2) hoặc EMR Serverless. Các quy tắc autoscaling quản lý thay đổi nhu cầu compute để tối ưu hiệu suất và thời gian thực thi.

### Các bước thực hiện:

1. Sau khi hoàn thành các bước chuẩn bị trước đó, vào SageMaker Studio và mở dự án của bạn
2. Chọn **Build**, sau đó chọn **JupyterLab**
Notebook mất khoảng 30 giây để khởi tạo và kết nối vào không gian làm việc
3. Dưới danh mục **Notebook**, chọn **Python 3 (ipykernel)**
4. Trong cell đầu tiên, kế bên **Local Python**, chọn menu thả xuống và chọn **PySpark**
5. Chọn menu thả xuống kế bên **Project.Spark** và chọn compute **EMR-S Compute**
6.Chạy mã sau để phát triển ứng dụng Spark của bạn. Ví dụ này đọc dataset TPC-DS 3 TB ở định dạng Parquet từ một bucket S3 công khai:
```python
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/store/").createOrReplaceTempView("store")
```

Khi phiên Spark bắt đầu và logs thực thi bắt đầu xuất hiện, bạn có thể khám phá giao diện Spark UI và logs driver để gỡ lỗi và khắc phục chương trình Spark.
![blog1](/images/3-Blog/BDB-5127-UI-Driver-4.png)
Ảnh chụp màn hình sau đây hiển thị ví dụ về giao diện người dùng Spark.
![blog1](/images/3-Blog/BDB-5127-SparkUI-5.png)
Ảnh chụp màn hình sau đây hiển thị ví dụ về nhật ký trình điều khiển. 
![blog1](/images/3-Blog/BDB-5127-Spark-Driver-6.png)
Ảnh chụp màn hình sau đây hiển thị tab Executors, cung cấp quyền truy cập vào nhật ký trình điều khiển và trình thực thi.
![blog1](/images/3-Blog/BDB-5127-Executors-7.png)

7. Sử dụng mã sau để đọc thêm một số tập dữ liệu TPC-DS. Bạn có thể tạo chế độ xem tạm thời và sử dụng Spark UI để xem các tệp đang được đọc. Tham khảo phần phụ lục ở cuối bài viết này để biết chi tiết về cách sử dụng tập dữ liệu TPC-DS trong các bucket của bạn.
```python
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/item/").createOrReplaceTempView("item")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/store_sales/").createOrReplaceTempView("store_sales")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/date_dim/").createOrReplaceTempView("date_dim")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/customer/").createOrReplaceTempView("customer")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/catalog_sales/").createOrReplaceTempView("catalog_sales")
spark.read.parquet("s3://blogpost-sparkoneks-us-east-1/blog/BLOG_TPCDS-TEST-3T-partitioned/web_sales/").createOrReplaceTempView("web_sales")

```
Trong mỗi cell của notebook, bạn có thể mở Spark Job Progress để xem các giai đoạn của job gửi đến EMR Serverless cho một cell cụ thể. Bạn có thể xem thời gian hoàn thành mỗi giai đoạn. Nếu xảy ra lỗi, bạn có thể kiểm tra logs, giúp việc khắc phục sự cố liền mạch hơn.
![blog1](/images/3-Blog/BDB-5127-Spark-Job-Progress-8.png)
Vì các file được phân vùng dựa trên cột khóa ngày, bạn có thể quan sát rằng Spark chạy các tác vụ song song để đọc dữ liệu.
![blog1](/images/3-Blog/BDB-5127-SparkJobs-9.png)
8. Tiếp theo, lấy count theo các khóa ngày trên dữ liệu được phân vùng theo khóa thời gian bằng mã:
```python
select count(1), ss_sold_date_sk from store_sales group by ss_sold_date_sk order by ss_sold_date_sk
```
![blog1](/images/3-Blog/BDB-5127-Notebook-Block-10.png)

## Giám sát công việc trong Spark UI

Trong tab **Jobs** của Spark UI, bạn có thể xem danh sách các job hoàn thành hoặc đang chạy, với các thông tin sau:

- Hành động kích hoạt job
- Thời gian thực hiện (ví dụ: 41 giây, nhưng thời gian sẽ khác nhau)
- Số giai đoạn (stages) và tác vụ (tasks) — trong ví dụ này là 2 stages và 3.428 tasks

Bạn có thể chọn job để xem chi tiết hơn, đặc biệt về các giai đoạn. Job của chúng tôi có hai giai đoạn; một stage mới được tạo mỗi khi có shuffle. Chúng tôi có một stage để đọc dữ liệu mỗi dataset ban đầu, và một stage cho phép tổng hợp (aggregation).
![blog1](/images/3-Blog/BDB-5127-JobRun-11.gif)
Trong ví dụ tiếp theo, chúng tôi chạy một số câu SQL TPC-DS dùng để đánh giá hiệu năng và benchmark:
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
Bạn có thể giám sát công việc Spark trong SageMaker Unified Studio bằng hai cách. Notebook Jupyter cung cấp giám sát cơ bản, hiển thị trạng thái job real-time và tiến độ thực thi. Để có phân tích chi tiết hơn, sử dụng Spark UI. Bạn có thể kiểm tra các giai đoạn cụ thể, tasks, và kế hoạch thực thi. Spark UI đặc biệt hữu ích để khắc phục vấn đề hiệu năng và tối ưu truy vấn. Bạn có thể theo dõi số lượng giai đoạn dự kiến, tasks chạy, và chi tiết thời lượng mỗi task.Cái nhìn toàn diện này giúp bạn hiểu sử dụng tài nguyên và theo dõi tiến độ job ở mức chi tiết.

![blog1](/images/3-Blog/BDB-5127-Spark-TPCDS-12.gif)
Trong phần này, chúng tôi đã giải thích cách bạn có thể dùng compute EMR Serverless trong SageMaker Unified Studio để xây dựng ứng dụng Spark tương tác. Qua Spark UI, ứng dụng tương tác cung cấp trạng thái mức tác vụ chi tiết, thông tin I/O và shuffle, cũng như liên kết đến logs tương ứng của tasks trực tiếp từ notebook, giúp trải nghiệm gỡ lỗi liền mạch.

---

## Dọn dẹp

Để tránh phát sinh chi phí liên tục trong tài khoản AWS của bạn, hãy xóa các tài nguyên bạn đã tạo trong bài hướng dẫn:

1. Xóa connection
2. Xóa job EMR
3. Xóa các S3 buckets đầu ra của EMR
4. Xóa các tài nguyên Amazon MWAA như workflows và environments
---

## Kết luận

Trong bài viết này, chúng tôi đã minh họa cách thế hệ tiếp theo của SageMaker, kết hợp với EMR Serverless, cung cấp giải pháp mạnh mẽ để phát triển, giám sát và lập lịch các ứng dụng Spark sử dụng dữ liệu trong Amazon S3. Trải nghiệm tích hợp giúp giảm đáng kể sự phức tạp bằng cách cung cấp môi trường phát triển thống nhất, quản lý tài nguyên tự động, và khả năng giám sát toàn diện thông qua Spark UI, trong khi vẫn duy trì hiệu quả chi phí theo mô hình trả theo sử dụng. Đối với doanh nghiệp, điều này có nghĩa là thời gian để có insight nhanh hơn, cải thiện sự hợp tác nhóm, và giảm gánh nặng vận hành, để các nhóm dữ liệu có thể tập trung vào phân tích thay vì quản lý hạ tầng.

Để bắt đầu, hãy khám phá [Amazon SageMaker Unified Studio User Guide](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/what-is-sagemaker-unified-studio.html), thiết lập một dự án trong môi trường AWS của bạn, và khám phá cách giải pháp này có thể biến đổi khả năng phân tích dữ liệu của tổ chức bạn.

---

## Phụ lục
Trong các phần sau, chúng tôi thảo luận cách chạy workload theo lịch và cung cấp chi tiết về dataset TPC-DS để xây dựng ứng dụng Spark sử dụng EMR Serverless.

### Chạy workload theo lịch

Trong phần này, chúng tôi triển khai notebook JupyterLab và tạo workflow sử dụng Amazon MWAA. Bạn có thể dùng workflows để điều phối notebooks, querybooks và nhiều thứ khác trong kho dự án của bạn. Với workflows, bạn có thể định nghĩa một tập các tác vụ tổ chức dưới dạng directed acyclic graph (DAG) có thể chạy theo lịch mà bạn định nghĩa.Các bước thực hiện:

1. Trong SageMaker Unified Studio, chọn **Build**, và dưới **Orchestration**, chọn **Workflows**
![blog1](/images/3-Blog/BDB-5127-Workflow-13.png)
2. Chọn **Create Workflow in Editor**
 Bạn sẽ được chuyển đến notebook JupyterLab với DAG mới tên `untitled.py` được tạo trong thư mục `/src/workflows/dag`
3. Đổi tên notebook này thành `tpcds_data_queries.py`

4.Bạn có thể tái sử dụng template hiện có với các cập nhật sau:

a. Cập nhật dòng 17 với lịch bạn muốn mã của bạn chạy
b. CCập nhật dòng 26 với  NOTEBOOK_PATH của bạn. Đường dẫn này nên nằm trong src/<notebook_name>.ipynb. Lưu ý tên dag_id  được tạo tự động; bạn có thể đặt tên theo yêu cầu của bạn.
![blog1](/images/3-Blog/BDB-5127-Spark-TPCDS-DagNotebk-14.jpg)
5. Chọn **File** và **Save notebook**
 Để kiểm thử, bạn có thể kích hoạt chạy thủ công workload
6. Trong SageMaker Unified Studio, chọn **Build**, rồi dưới **Orchestration**, chọn **Workflows**.
7. Chọn workflow của bạn, rồi chọn **Run**.
 Bạn có thể giám sát thành công của job trên tab Runs.
 ![blog1](/images/3-Blog/BDB-5127-AirflowRun-15.png)
Để gỡ lỗi job notebook bằng cách truy cập Spark UI trong console Airflow job, bạn phải sử dụng [EMR Serverless Airflow Operators](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/using-airflow.html) để gửi job. Liên kết có sẵn trên tab **Details** của truy vấn.
Tùy chọn này có các hạn chế chính: nó không khả dụng cho Amazon EMR trên EC2, và các operator notebook job SageMaker không hoạt động.

Bạn có thể cấu hình operator để tạo liên kết một lần đến UI ứng dụng và logs đầu ra Spark bằng cách truyền `enable_application_ui_links=True` làm tham số. Sau khi job bắt đầu chạy, các liên kết này có sẵn trên tab Details của task tương ứng. Nếu `enable_application_ui_links=False`, thì liên kết sẽ xuất hiện nhưng ở trạng thái xám.

Hãy đảm bảo bạn có `emr-serverless:GetDashboardForJobRun ` trong  [AWS Identity and Access Management](https://aws.amazon.com/iam/)(IAM)  để tạo liên kết dashboard.

Mở giao diện người dùng Airflow cho tác vụ của bạn. Giao diện người dùng Spark và các tùy chọn bảng điều khiển máy chủ lịch sử sẽ hiển thị trên tab Details, như trong ảnh chụp màn hình sau.

![blog1](/images/3-Blog/BDB-5127-AirflowUI-16.jpeg)

Ảnh chụp màn hình minh họa tab Jobs của Spark UI được trình bày.

![blog1](/images/3-Blog/BDB-5127-Airflow-SparkUI-17.jpeg)
---

## Về các tác giả

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/Amit-Maindola.jpg" alt="Amit Maindola" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Amit Maindola</strong> là Senior Data Architect tập trung vào data engineering, analytics và AI/ML tại Amazon Web Services. Ông giúp khách hàng trong quá trình chuyển đổi số và cho phép họ xây dựng các giải pháp phân tích đám mây quy mô cao, mạnh mẽ và an toàn trên AWS để thu được insight kịp thời và ra quyết định kinh doanh quan trọng.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/image045.jpg" alt="Abhilash Nagilla" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Abhilash Nagilla</strong> là senior specialist solutions architect tại Amazon Web Services (AWS), hỗ trợ các khách hàng khu vực công trên hành trình đám mây của họ với trọng tâm là các dịch vụ dữ liệu và AI của AWS. Ngoài công việc, Abhilash thích tìm hiểu công nghệ mới, xem phim và đi du lịch đến những nơi mới.</p>
  </div>
</div>