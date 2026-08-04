---
title: "Blog 2"
date: "2025-07-10"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Dân chủ hóa dữ liệu để ra quyết định kịp thời với text-to-SQL tại Parcel Perform

Tác giả: Yudho Ahmad Diponegoro, Le Vy, và Jun Kai Loke | Ngày: 09 tháng 7 năm 2025 | Danh mục: [Amazon Athena](https://aws.amazon.com/blogs/machine-learning/category/analytics/amazon-athena/), [Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Amazon Bedrock Knowledge Bases](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/amazon-bedrock-knowledge-bases/), [Business Intelligence](https://aws.amazon.com/blogs/machine-learning/category/business-intelligence/), [Customer Solutions](https://aws.amazon.com/blogs/machine-learning/category/post-types/customer-solutions/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), [Intermediate (200)](https://aws.amazon.com/blogs/machine-learning/category/learning-levels/intermediate-200/), [Supply Chain](https://aws.amazon.com/blogs/machine-learning/category/supply-chain/)

*Bài viết này được đồng thực hiện với Le Vy từ Parcel Perform.*

---

Truy cập dữ liệu chính xác thường là yếu tố thực sự khác biệt giữa những quyết định xuất sắc và những quyết định kịp thời. Điều này càng trở nên quan trọng hơn đối với những quyết định và hành động hướng tới khách hàng. Một AI hiện đại được triển khai đúng cách có thể giúp tổ chức của bạn đơn giản hóa truy cập dữ liệu để đưa ra các quyết định chính xác và kịp thời cho nhóm kinh doanh hướng đến khách hàng, trong khi giảm thiểu công việc nặng "không phân biệt" mà nhóm dữ liệu của bạn phải làm. Trong bài viết này, chúng tôi chia sẻ cách [Parcel Perform](https://www.parcelperform.com/), một nền tảng AI Delivery Experience hàng đầu cho các doanh nghiệp thương mại điện tử trên toàn cầu, đã triển khai một giải pháp như vậy.

Việc theo dõi giao hàng sau mua một cách chính xác có thể rất quan trọng đối với nhiều thương nhân thương mại điện tử. Parcel Perform cung cấp một trải nghiệm dữ liệu và giao hàng thông minh, được điều khiển bởi AI, từ đầu đến cuối, và hệ thống phần mềm như dịch vụ (SaaS) cho các thương nhân thương mại điện tử. Hệ thống sử dụng các dịch vụ AWS và AI hiện đại để xử lý hàng trăm triệu dữ liệu chuyển động giao hàng mỗi ngày, và cung cấp khả năng theo dõi thống nhất trên các đơn vị giao hàng cho các thương nhân, với điểm nhấn là độ chính xác và sự đơn giản.

Nhóm kinh doanh tại Parcel Perform thường cần truy cập dữ liệu để trả lời các câu hỏi liên quan đến giao hàng của các thương nhân, như "Tuần trước chúng ta có thấy sự tăng đột biến trong việc trễ giao hàng không? Nếu có, ở những trung tâm vận chuyển nào được quan sát điều này, và nguyên nhân chính của vấn đề là gì?" Trước đây, nhóm dữ liệu phải tự tạo truy vấn và chạy truy vấn đó để lấy dữ liệu. Với khả năng text-to-SQL mới được hỗ trợ bởi generative AI trong Parcel Perform, nhóm kinh doanh có thể tự phục vụ nhu cầu dữ liệu của họ bằng cách sử dụng giao diện trợ lý AI. Trong bài viết này, chúng tôi thảo luận cách Parcel Perform kết hợp generative AI, lưu trữ dữ liệu và truy cập dữ liệu thông qua các dịch vụ AWS để đưa ra quyết định kịp thời.

---

## Kiến trúc phân tích dữ liệu

Giải pháp bắt đầu từ việc ingest (nhập), lưu trữ và truy cập dữ liệu. Parcel Perform áp dụng kiến trúc phân tích dữ liệu như trong sơ đồ dưới đây.

![Data Analytics Architecture](/images/3-Blog/ML-18476-data-architecture.png)

Một loại dữ liệu then chốt trong ứng dụng giám sát kiện hàng của Parcel Perform là dữ liệu sự kiện kiện hàng (parcel event data), có thể lên đến hàng tỷ dòng. Điều này bao gồm thay đổi trạng thái gửi hàng, thay đổi vị trí, và nhiều hơn nữa. Dữ liệu hàng ngày từ nhiều đơn vị kinh doanh được đổ về các cơ sở dữ liệu quan hệ relational databases được lưu trữ trên [Amazon Relational Database Service](https://aws.amazon.com/rds/) (Amazon RDS).

Mặc dù cơ sở dữ liệu quan hệ phù hợp cho việc ingest nhanh và tiêu thụ từ ứng dụng, một stack phân tích riêng biệt là cần thiết để xử lý các phép phân tích ở quy mô lớn và hiệu năng cao mà không làm gián đoạn ứng dụng chính. Những nhu cầu phân tích này bao gồm trả lời các truy vấn tổng hợp từ những câu hỏi như "Có bao nhiêu kiện hàng bị trễ tuần trước?"

Parcel Perform sử dụng [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) kết hợp một query engine do [Amazon Athena](https://aws.amazon.com/athena/) cung cấp để đáp ứng nhu cầu phân tích của họ. Với cách tiếp cận này, Parcel Perform được hưởng lợi từ lưu trữ chi phí thấp trong khi vẫn có thể chạy các truy vấn SQL khi cần thiết, và Athena tính phí dựa trên mức sử dụng.

Dữ liệu trong Amazon S3 được lưu ở định dạng [Apache Iceberg](https://iceberg.apache.org/) cho phép cập nhật dữ liệu — điều này có ích trong trường hợp dữ liệu sự kiện kiện hàng đôi khi được cập nhật. Nó cũng hỗ trợ phân vùng (partitioning) để cải thiện hiệu suất. [Amazon S3 Tables](https://aws.amazon.com/s3/features/tables/), ra mắt vào cuối năm 2024, là tính năng quản lý các bảng Iceberg, và cũng có thể là một lựa chọn cho bạn.

Parcel Perform sử dụng một cluster [Apache Kafka](https://kafka.apache.org/) được quản lý bởi [Amazon Managed Streaming for Apache Kafka](https://aws.amazon.com/msk/) (Amazon MSK) như luồng để chuyển dữ liệu từ nguồn đến bucket S3. [Amazon MSK Connect](https://aws.amazon.com/msk/features/msk-connect/) với connector Debezium phát dữ liệu bằng change data capture (CDC) từ Amazon RDS sang Amazon MSK.

[Apache Flink](https://flink.apache.org/), chạy trên [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS), xử lý các luồng dữ liệu từ Amazon MSK. Nó ghi dữ liệu này vào bucket S3 theo định dạng Iceberg, và cập nhật schema dữ liệu trong [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html). Schema dữ liệu này cho phép Athena truy vấn đúng dữ liệu trong bucket S3.

Bây giờ bạn đã hiểu cách dữ liệu được ingest và lưu trữ, chúng ta sẽ xem cách dữ liệu được tiêu thụ thông qua trợ lý phục vụ dữ liệu sử dụng generative AI cho các nhóm kinh doanh tại Parcel Perform.

---

## AI agent có thể truy vấn dữ liệu

Người dùng của AI agent phục vụ dữ liệu tại Parcel Perform là các thành viên nhóm kinh doanh hướng đến khách hàng, những người thường xuyên truy vấn dữ liệu sự kiện kiện hàng để trả lời các câu hỏi từ các thương nhân thương mại điện tử về giao hàng và hỗ trợ họ một cách chủ động. Ảnh chụp màn hình dưới đây cho thấy trải nghiệm UI của trợ lý AI, được hỗ trợ bởi text-to-SQL với generative AI.

![AI Assistant UI](/images/3-Blog/ML-18476-ai-assistant-screenshot.png)

Tính năng này đã giúp nhóm Parcel Perform và khách hàng của họ tiết kiệm thời gian, phần mà chúng tôi sẽ thảo luận sau trong bài viết này. Trong phần tiếp theo, chúng tôi trình bày kiến trúc hỗ trợ tính năng này.

---

## Kiến trúc AI agent text-to-SQL

Kiến trúc trợ lý AI phục vụ dữ liệu tại Parcel Perform được thể hiện trong sơ đồ sau:

![Text-to-SQL Architecture](/images/3-Blog/ML-18476-ai-assistant-architecture.png)

UI của trợ lý AI được hỗ trợ bởi một ứng dụng được xây dựng với framework [FastAPI](https://fastapi.tiangolo.com/), chạy trên Amazon EKS. Nó cũng được đặt phía trước bởi một [Application Load Balancer](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/) để cho phép khả năng mở rộng theo chiều ngang nếu cần.

Ứng dụng sử dụng [LangGraph](https://www.langchain.com/langgraph) để điều phối workflow của các lời gọi mô hình ngôn ngữ lớn (LLM), việc sử dụng các công cụ, và checkpointing bộ nhớ. Đồ thị sử dụng nhiều công cụ, bao gồm những công cụ từ [SQLDatabase Toolkit](https://python.langchain.com/docs/integrations/tools/sql_database/) để tự động lấy schema dữ liệu thông qua Athena. Đồ thị cũng sử dụng một [Amazon Bedrock Knowledge Bases retriever](https://python.langchain.com/docs/integrations/retrievers/bedrock/) để truy xuất thông tin kinh doanh từ một knowledge base. Parcel Perform sử dụng các mô hình [Anthropic’s Claude models in Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) để tạo SQL.

Mặc dù vai trò của Athena như là engine truy vấn để truy vấn dữ liệu sự kiện kiện hàng trên Amazon S3 là rõ ràng, Parcel Perform vẫn cần một knowledge base. Trong trường hợp sử dụng này, việc tạo SQL hoạt động tốt hơn khi LLM có thêm thông tin ngữ cảnh kinh doanh để giúp diễn giải các trường dữ liệu và chuyển ngữ thuật ngữ logistics thành biểu diễn dữ liệu. Điều này được minh họa tốt hơn qua hai ví dụ sau:

1. Hoạt động trong hồ dữ liệu của Parcel Perform sử dụng các mã cụ thể: `c` cho create và `u` cho update. Khi phân tích dữ liệu, Parcel Perform đôi khi cần tập trung chỉ vào các bản ghi tạo ban đầu, nơi operation code bằng `c`. Vì logic kinh doanh này có thể không có trong dữ liệu huấn luyện của LLM chung, Parcel Perform định nghĩa rõ ràng điều này trong ngữ cảnh kinh doanh của họ.

2. Trong thuật ngữ logistics, transit time có các quy ước ngành cụ thể. Nó được đo bằng ngày, và giao hàng trong cùng ngày được ghi là `transit_time = 0`. Mặc dù điều này trực quan đối với chuyên gia logistics, một LLM có thể dịch sai yêu cầu như "Lấy tất cả các chuyến gửi với giao hàng cùng ngày" bằng cách sử dụng `WHERE transit_time = 1` thay vì `WHERE transit_time = 0` trong câu SQL được tạo ra.

Vì vậy, mỗi câu hỏi đến sẽ đi qua một workflow Retrieval Augmented Generation (RAG) để tìm các thông tin ngữ cảnh kinh doanh đã lưu có thể liên quan, nhằm làm giàu ngữ cảnh. Cơ chế này giúp cung cấp các quy tắc và cách diễn giải cụ thể mà ngay cả những LLM tiên tiến cũng có thể không suy ra được từ dữ liệu huấn luyện chung.

Parcel Perform sử dụng [Amazon Bedrock Knowledge Bases](https://aws.amazon.com/bedrock/knowledge-bases/) như một giải pháp được quản lý cho workflow RAG. Họ ingest thông tin ngữ cảnh kinh doanh bằng cách tải file lên Amazon S3. Amazon Bedrock Knowledge Bases xử lý các file, chia chúng thành các chunk, sử dụng embedding models để tạo vector, và lưu các vector vào một vector database để chúng có thể được tìm kiếm. Các bước này được quản lý toàn bộ bởi Amazon Bedrock Knowledge Bases. Parcel Perform lưu các vector trong [Amazon OpenSearch Serverless](https://aws.amazon.com/opensearch-service/features/serverless/) như vector database được lựa chọn để đơn giản hóa quản lý hạ tầng.

Amazon Bedrock Knowledge Bases cung cấp [Retrieve API](https://aws.amazon.com/opensearch-service/features/serverless/), nhận đầu vào (chẳng hạn như câu hỏi từ trợ lý AI), chuyển nó thành embedding vector, tìm kiếm các chunk thông tin kinh doanh liên quan trong vector database, và trả lại các chunk tài liệu phù hợp nhất. Nó được tích hợp với trình thu thập Cơ sở kiến thức Amazon Bedrock của [LangChain](https://www.langchain.com/) bằng cách [calling the invoke method.](https://python.langchain.com/api_reference/aws/retrievers/langchain_aws.retrievers.bedrock.AmazonKnowledgeBasesRetriever.html#langchain_aws.retrievers.bedrock.AmazonKnowledgeBasesRetriever.invoke)

Bước tiếp theo liên quan đến việc gọi một AI agent với thông tin ngữ cảnh kinh doanh được cung cấp và prompt tạo SQL. Prompt này được lấy cảm hứng từ [a prompt in LangChain Hub](https://smith.langchain.com/hub/langchain-ai/sql-agent-system-prompt). Dưới đây là đoạn mã mẫu của prompt:

```
You are an agent designed to interact with a SQL database.
Given an input question, create a syntactically correct {dialect} query to run, then look at the results of the query and return the answer.
Unless the user specifies a specific number of examples they wish to obtain, always limit your query to at most {top_k} results.

Relevant context:
{rag_context}

You can order the results by a relevant column to return the most interesting examples in the database.
Never query for all the columns from a specific table, only ask for the relevant columns given the question.
You have access to tools for interacting with the database.
- Only use the below tools. Only use the information returned by the below tools to construct your final answer.
- DO NOT make any DML statements (INSERT, UPDATE, DELETE, DROP etc.) to the database.
- To start querying for final answer you should ALWAYS look at the tables in the database to see what you can query. Do NOT skip this step.
- Then you should query the schema of the most relevant tables
```

Prompt mẫu là một phần của hướng dẫn ban đầu cho agent. Schema dữ liệu được chèn tự động bởi các công cụ từ SQLDatabase Toolkit trong bước sau của workflow agent. Các bước sau xảy ra sau khi người dùng nhập câu hỏi vào UI trợ lý AI:

1. Câu hỏi kích hoạt một lần chạy biểu đồ LangGraph.

2. Các quá trình sau diễn ra song song:
   a. Đồ thị lấy schema cơ sở dữ liệu từ Athena thông qua SQLDatabase Toolkit.
   b. Đồ thị truyền câu hỏi đến Amazon Bedrock Knowledge Bases retriever và nhận danh sách thông tin kinh doanh liên quan đến câu hỏi.

3. Đồ thị gọi một LLM sử dụng Amazon Bedrock bằng cách truyền câu hỏi, ngữ cảnh đối thoại, schema dữ liệu và thông tin ngữ cảnh kinh doanh. Kết quả là SQL được sinh ra.

4. Đồ thị sử dụng SQLDatabase Toolkit một lần nữa để chạy SQL thông qua Athena và nhận kết quả dữ liệu.

5. Kết quả dữ liệu được truyền vào một LLM để sinh phản hồi cuối cùng dựa trên câu hỏi ban đầu. [Amazon Bedrock Guardrails](https://aws.amazon.com/bedrock/guardrails/) được sử dụng như biện pháp bảo vệ để tránh các đầu vào và phản hồi không phù hợp.

6. Phản hồi cuối cùng được trả về cho người dùng qua UI trợ lý AI.

Sơ đồ dưới đây minh họa các bước này.

![Workflow Steps](/images/3-Blog/ML-18476-ai-assistant-architecture-numbered.png)

Việc triển khai này cho thấy cách Parcel Perform biến các yêu cầu thô thành dữ liệu có thể hành động để hỗ trợ quyết định kịp thời. Bảo mật cũng được triển khai ở nhiều thành phần. Về mạng, các pod EKS được đặt trong các subnet riêng (private subnets) trong [Amazon Virtual Private Cloud](http://aws.amazon.com/vpc) (Amazon VPC) để tăng cường bảo mật mạng cho ứng dụng trợ lý AI. Trợ lý AI này được đặt sau một lớp backend yêu cầu xác thực. Về bảo mật dữ liệu, các dữ liệu nhạy cảm được mask khi lưu trữ (at rest) trong bucket S3.

Parcel Perform cũng giới hạn quyền của role [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) được sử dụng để truy cập bucket S3 sao cho chỉ có thể truy cập các bảng nhất định.

Trong các phần tiếp theo, chúng tôi thảo luận cách Parcel Perform tiếp cận việc xây dựng giải pháp chuyển đổi dữ liệu này.

---

## Từ ý tưởng đến sản xuất

Parcel Perform bắt đầu với ý tưởng giải phóng nhóm dữ liệu của họ khỏi việc thủ công phục vụ các yêu cầu từ nhóm kinh doanh, đồng thời cải thiện tính kịp thời của khả năng truy cập dữ liệu để hỗ trợ nhóm kinh doanh trong quyết định của họ.

Với sự hỗ trợ của đội Kiến trúc sư Giải pháp AWS, Parcel Perform hoàn thành một proof of concept sử dụng các dịch vụ AWS và một Jupyter notebook trong [Amazon SageMaker Studio](https://jupyter.org/). Sau khi bước đầu thành công, Parcel Perform tích hợp giải pháp này với công cụ điều phối họ chọn, LangGraph.

Trước khi đưa vào sản xuất, Parcel Perform tiến hành thử nghiệm kỹ lưỡng để xác minh kết quả có nhất quán không. Họ thêm [LangSmith Tracing](https://docs.smith.langchain.com/observability) để ghi lại các bước và kết quả của AI agent để đánh giá hiệu suất của nó.

Nhóm Parcel Perform phát hiện ra những thách thức trong hành trình của họ, mà chúng tôi sẽ thảo luận trong phần sau. Họ thực hiện prompt engineering để giải quyết những thách thức đó. Cuối cùng, AI agent được tích hợp vào sản xuất để được nhóm kinh doanh sử dụng. Sau đó, Parcel Perform thu thập phản hồi người dùng nội bộ và giám sát các log từ LangSmith Tracing để xác minh hiệu suất được duy trì.

---

## Những thách thức

Hành trình này không miễn nhiễm với các thách thức.

Đầu tiên, một số thương nhân thương mại điện tử có thể có nhiều bản ghi trong hồ dữ liệu với tên khác nhau. Ví dụ, một thương nhân tên "ABC" có thể có nhiều bản ghi như "ABC Singapore Holdings Pte. Ltd.," "ABC Demo Account," "ABC Test Group," v.v. Đối với câu hỏi như "Tuần trước có kiện hàng giao trễ nào của ABC không?", SQL được sinh có phần `WHERE merchant_name LIKE '%ABC%'`, điều này có thể dẫn đến sự mơ hồ.

Trong giai đoạn proof of concept, vấn đề này đã gây ra sự khớp sai kết quả.

Đối với thách thức này, Parcel Perform dựa vào prompt engineering cẩn thận để hướng dẫn LLM xác định khi nào tên có thể mơ hồ. AI agent sau đó gọi Athena lại để tìm tên phù hợp. LLM quyết định tên thương nhân nào được sử dụng dựa trên nhiều yếu tố, bao gồm đóng góp volume dữ liệu và trạng thái tài khoản trong hồ dữ liệu. Trong tương lai, Parcel Perform dự kiến triển khai kỹ thuật tinh vi hơn bằng cách nhắc người dùng giải quyết sự mơ hồ.

Thách thức thứ hai liên quan đến các câu hỏi không bị ràng buộc có thể tạo ra các truy vấn tốn kém chạy trên lượng lớn dữ liệu và dẫn đến thời gian chờ truy vấn lâu hơn. Một số câu hỏi như vậy có thể không có mệnh đề LIMIT trong truy vấn. Để giải quyết vấn đề này, Parcel Perform hướng dẫn LLM thêm mệnh đề LIMIT với số lượng kết quả tối đa nếu người dùng không chỉ định số lượng kết quả mong muốn.

Trong tương lai, Parcel Perform dự định sử dụng kết quả EXPLAIN của truy vấn để xác định những truy vấn nặng.

**Thách thức thứ ba** liên quan đến việc theo dõi việc sử dụng và chi phí phát sinh của giải pháp này. Với việc bắt đầu nhiều dự án generative AI sử dụng Amazon Bedrock và đôi khi sử dụng cùng một ID mô hình LLM, Parcel Perform phải phân biệt chi phí phát sinh theo dự án. Parcel Perform tạo một [inference profile](https://docs.aws.amazon.com/bedrock/latest/userguide/inference-profiles.html) cho mỗi dự án, gán profile đó với các [tags](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/what-are-tags.html), và bao gồm profile đó trong mỗi lời gọi LLM cho dự án đó. Với cách thiết lập này, Parcel Perform có thể phân tách chi phí theo từng dự án để cải thiện tính minh bạch và theo dõi chi phí.

---

## Tác động

Để trích xuất dữ liệu, nhóm kinh doanh phải làm rõ yêu cầu với nhóm dữ liệu, gửi yêu cầu, kiểm tra tính khả thi, và chờ đợi nguồn lực. Quy trình này kéo dài hơn khi các yêu cầu đến từ khách hàng hoặc các nhóm ở múi giờ khác nhau, mỗi lần làm rõ thêm có thể mất 12–24 giờ do giao tiếp bất đồng bộ. Các yêu cầu đơn giản gửi sớm trong ngày làm việc có thể hoàn thành trong 24 giờ, trong khi các yêu cầu phức tạp hơn hoặc vào thời điểm cao điểm có thể mất từ 3–5 ngày làm việc.

Với trợ lý AI text-to-SQL, quy trình này được đơn giản hóa đáng kể — giảm thiểu việc trao đổi qua lại để làm rõ yêu cầu, loại bỏ phụ thuộc vào năng lực của nhóm dữ liệu và tự động hóa phần diễn giải kết quả.

Đo lường của Parcel Perform cho thấy trợ lý AI text-to-SQL giảm thời gian để có insight trung bình xuống 99%, từ 2,3 ngày xuống còn trung bình 10 phút, tiết kiệm khoảng 3.850 giờ chờ đợi mỗi tháng cho tất cả người yêu cầu trong khi vẫn giữ được độ chính xác dữ liệu.

Người dùng có thể trực tiếp truy vấn dữ liệu mà không cần trung gian, nhận kết quả trong vài phút thay vì vài ngày. Các nhóm ở các múi giờ khác nhau giờ đây có thể truy cập insight bất cứ lúc nào trong ngày, giảm bớt sự khó chịu "chờ châu Á thức dậy" hoặc "bắt kịp EMEA trước khi họ rời đi", dẫn đến khách hàng hài lòng hơn và giải quyết vấn đề nhanh hơn.

Sự chuyển đổi này đã ảnh hưởng sâu sắc đến khả năng và trọng tâm của đội phân tích dữ liệu, giải phóng nhóm dữ liệu để làm việc chiến lược hơn và giúp mọi người đưa ra quyết định nhanh hơn, có thông tin hơn. Trước đây, các nhà phân tích dành khoảng 25% thời gian làm việc cho các yêu cầu trích xuất dữ liệu thông thường — tương đương hơn 260 giờ mỗi tháng cho cả nhóm. Nay, với các truy vấn cơ bản và trung bình được tự động hóa, con số này giảm xuống chỉ còn 10%, giải phóng gần 160 giờ mỗi tháng cho các công việc có tác động cao. Các nhà phân tích giờ đây tập trung vào phân tích dữ liệu phức tạp thay vì dành thời gian cho các tác vụ truy xuất dữ liệu cơ bản.

---

## Kết luận

Giải pháp của Parcel Perform minh chứng cách bạn có thể sử dụng generative AI để nâng cao năng suất và trải nghiệm khách hàng. Parcel Perform đã xây dựng một AI agent text-to-SQL biến câu hỏi của nhóm kinh doanh thành SQL để truy vấn dữ liệu thực. Điều này cải thiện tính kịp thời của khả năng truy cập dữ liệu phục vụ quyết định liên quan đến khách hàng. Hơn nữa, nhóm dữ liệu có thể tránh công việc "nặng không phân biệt" để tập trung vào các nhiệm vụ phân tích dữ liệu phức tạp.

Giải pháp này sử dụng nhiều dịch vụ AWS như Amazon Bedrock và các công cụ như LangGraph. Bạn có thể bắt đầu với một proof of concept và tham khảo ý kiến Kiến trúc sư Giải pháp AWS hoặc hợp tác với [AWS Partners](https://partners.amazonaws.com/). Nếu bạn có câu hỏi, hãy đăng lên [AWS re:Post](https://repost.aws/). Bạn cũng có thể làm cho quá trình phát triển trở nên dễ dàng hơn với sự trợ giúp của [Amazon Q Developer](https://aws.amazon.com/q/developer/).

Khi bạn gặp thách thức, bạn có thể lặp lại để tìm giải pháp, có thể bao gồm prompt engineering hoặc bổ sung bước vào workflow của bạn.

Bảo mật là ưu tiên hàng đầu. Hãy đảm bảo trợ lý AI của bạn có guardrails thích hợp để bảo vệ chống lại prompt threats, các chủ đề không phù hợp, ngôn từ thô tục, rò rỉ dữ liệu và các vấn đề bảo mật khác. Bạn có thể tích hợp Amazon Bedrock Guardrails với ứng dụng generative AI của bạn thông qua API.

**Để tìm hiểu thêm, hãy tham khảo các tài nguyên sau:**
- [Xây dựng giải pháp chuyển đổi văn bản sang SQL mạnh mẽ, tạo ra các truy vấn phức tạp, tự động sửa lỗi và truy vấn các nguồn dữ liệu đa dạng](https://aws.amazon.com/blogs/machine-learning/build-a-robust-text-to-sql-solution-generating-complex-queries-self-correcting-and-querying-diverse-data-sources/)
- [Hội thảo về tác nhân LangGraph với Amazon Bedrock](https://catalog.us-east-1.prod.workshops.aws/workshops/9bc28f51-d7c3-468b-ba41-72667f3273f1/en-US)
- [Xây dựng cơ sở kiến thức bằng cách kết nối với kho dữ liệu có cấu trúc](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-build-structured.html)

---

## Về các tác giả

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/yudho-full2.jpg" alt="Yudho Ahmad Diponegoro" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Yudho Ahmad Diponegoro</strong> là một Senior Solutions Architect tại AWS. Có hơn 10 năm làm việc tại Amazon, ông đã giữ nhiều vai trò từ phát triển phần mềm đến kiến trúc giải pháp. Ông hỗ trợ các startup tại Singapore về kiến trúc trên đám mây. Dù giữ kiến thức rộng về công nghệ và ngành, ông tập trung vào AI và machine learning, nơi ông đã dẫn dắt nhiều startup trong khu vực ASEAN áp dụng machine learning và generative AI tại AWS.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/levy-copy-1.png" alt="Le Vy" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Le Vy</strong> là Trưởng nhóm AI tại Parcel Perform, người thúc đẩy phát triển các ứng dụng AI và khám phá nghiên cứu AI mới. Cô bắt đầu sự nghiệp trong phân tích dữ liệu và đào sâu vào AI qua chương trình thạc sĩ về trí tuệ nhân tạo. Đam mê áp dụng dữ liệu và AI để giải quyết vấn đề kinh doanh thực tế, cô cũng dành thời gian hướng dẫn các kỹ sư trẻ và xây dựng cộng đồng hỗ trợ trong lĩnh vực công nghệ. Trong công việc của mình, Vy tích cực thách thức các chuẩn mực giới trong ngành và cổ vũ học tập suốt đời như chìa khóa đổi mới.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/junkai.png" alt="Loke Jun Kai" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Loke Jun Kai</strong> là GenAI/ML Specialist Solutions Architect tại AWS, phục vụ các khách hàng chiến lược trong khu vực ASEAN. Ông làm việc với các khách hàng từ Start-up đến Enterprise để xây dựng các use case tiên tiến và nền tảng GenAI có thể mở rộng. Đam mê trong lĩnh vực AI, việc nghiên cứu và đọc sách liên tục đã dẫn đến nhiều giải pháp sáng tạo với kết quả kinh doanh cụ thể. Ngoài công việc, ông thích chơi tennis và cờ.</p>
  </div>
</div>