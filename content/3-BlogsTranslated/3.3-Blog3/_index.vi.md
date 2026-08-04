---
title: "Blog 3"
date: "2025-07-10"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Xây dựng trình tạo video AI có thể mở rộng sử dụng Amazon SageMaker AI và CogVideoX

Tác giả: Nick Biso, Jinzhao Feng, Katherine Feng, và Natasha Tchir | Ngày: 19 tháng 6 năm 2025 | Danh mục: [Amazon SageMaker](https://aws.amazon.com/sagemaker/), [Amazon SageMaker AI](https://aws.amazon.com/sagemaker/ai/), [Artificial Intelligence](https://aws.amazon.com/machine-learning/), [Generative AI](https://aws.amazon.com/generative-ai/), [Intermediate (200)](https://aws.amazon.com/training/)

---
Trong những năm gần đây, sự tiến bộ nhanh chóng của công nghệ trí tuệ nhân tạo và machine learning (AI/ML) đã cách mạng hóa nhiều khía cạnh của việc tạo nội dung số. Một phát triển đặc biệt thú vị là sự xuất hiện của khả năng tạo video, mang lại cơ hội chưa từng có cho các công ty trong nhiều ngành công nghiệp khác nhau. Công nghệ này cho phép tạo ra các video clip ngắn có thể được kết hợp một cách liền mạch để sản xuất các video dài hơn, phức tạp hơn. Tiềm năng ứng dụng của sự đổi mới này rất lớn và sâu rộng, hứa hẹn sẽ biến đổi cách các doanh nghiệp giao tiếp, tiếp thị và tương tác với khán giả của họ. Công nghệ tạo video mang lại hàng loạt các trường hợp sử dụng cho các công ty muốn nâng cao chiến lược nội dung hình ảnh của mình. Chẳng hạn, các doanh nghiệp thương mại điện tử có thể sử dụng công nghệ này để tạo ra các bản demo sản phẩm động, trưng bày các mặt hàng từ nhiều góc độ và trong các bối cảnh khác nhau mà không cần phải chụp ảnh vật lý rộng rãi. Trong lĩnh vực giáo dục và đào tạo, các tổ chức có thể tạo ra các video hướng dẫn được tùy chỉnh theo các mục tiêu học tập cụ thể, nhanh chóng cập nhật nội dung khi cần mà không cần quay lại toàn bộ chuỗi. Các nhóm tiếp thị có thể tạo ra các quảng cáo video cá nhân hóa theo quy mô, nhắm đến các nhóm nhân khẩu học khác nhau với thông điệp và hình ảnh tùy chỉnh. Hơn nữa, ngành giải trí sẽ hưởng lợi rất lớn, với khả năng nhanh chóng tạo nguyên mẫu các cảnh quay, hình dung các ý tưởng, và thậm chí hỗ trợ trong việc tạo nội dung hoạt hình. Tính linh hoạt được mang lại bởi việc kết hợp các clip được tạo ra thành các video dài hơn mở ra thậm chí nhiều khả năng hơn nữa. Các công ty có thể tạo nội dung mô-đun có thể được sắp xếp lại và tái sử dụng nhanh chóng cho các hiển thị, khán giả, hoặc chiến dịch khác nhau. Khả năng thích ứng này không chỉ tiết kiệm thời gian và tài nguyên, mà còn cho phép các chiến lược nội dung linh hoạt và phản hồi nhanh hơn. Khi chúng ta đi sâu hơn vào tiềm năng của công nghệ tạo video, rõ ràng là giá trị của nó vượt xa sự tiện lợi đơn thuần, cung cấp một công cụ chuyển đổi có thể thúc đẩy sự đổi mới, hiệu quả và sự tham gia trên toàn cảnh quan doanh nghiệp.

Trong bài viết này, chúng tôi khám phá cách triển khai một giải pháp mạnh mẽ dựa trên AWS cho việc tạo video sử dụng mô hình CogVideoX và [Amazon SageMaker AI](https://aws.amazon.com/sagemaker-ai).

---

## Tổng quan về giải pháp

Kiến trúc của chúng tôi cung cấp một giải pháp tạo video có thể mở rộng cao và bảo mật sử dụng các dịch vụ được quản lý của AWS. Lớp quản lý dữ liệu triển khai ba [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3) bucket có mục đích riêng biệt—cho video đầu vào, đầu ra đã xử lý, và ghi log truy cập—mỗi bucket được cấu hình với các chính sách mã hóa và lifecycle phù hợp để hỗ trợ bảo mật dữ liệu xuyên suốt chu kỳ sống của nó.

Đối với tài nguyên tính toán, chúng tôi sử dụng [AWS Fargate](https://aws.amazon.com/fargate/) cho [Amazon Elastic Container Service](http://aws.amazon.com/ecs) (Amazon ECS) để lưu trữ ứng dụng web [Streamlit](https://streamlit.io/), cung cấp quản lý container serverless với khả năng tự động mở rộng. Traffic được phân phối hiệu quả thông qua một [Application Load Balancer](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/). Pipeline xử lý AI sử dụng các SageMaker AI processing job để xử lý các tác vụ tạo video, tách biệt việc tính toán chuyên sâu khỏi giao diện web để tối ưu chi phí và tăng khả năng bảo trì. Các prompt của người dùng được tinh chỉnh thông qua [Amazon Bedrock](https://aws.amazon.com/bedrock/), sau đó được đưa vào mô hình [CogVideoX-5b](https://huggingface.co/THUDM/CogVideoX-5b) để tạo video chất lượng cao, tạo ra một giải pháp end-to-end cân bằng giữa hiệu suất, bảo mật và hiệu quả chi phí.

Sơ đồ sau đây minh họa kiến trúc giải pháp.

![blog 3](/images/3-Blog/ML-17715-architecture-diagram.png)

## Mô hình CogVideoX
[CogVideoX](https://arxiv.org/abs/2408.06072) là một mô hình tạo video từ văn bản mã nguồn mở, hiện đại nhất có khả năng tạo ra video liên tục 10 giây với tốc độ 16 khung hình/giây ở độ phân giải 768×1360 pixel. Mô hình dịch hiệu quả các prompt văn bản thành các câu chuyện video mạch lạc, giải quyết các hạn chế thường gặp trong các hệ thống tạo video trước đó.

Mô hình sử dụng ba đổi mới chính:

 1. Một bộ mã hóa tự động biến phân 3D (3D Variational Autoencoder - VAE) nén video theo cả chiều không gian và thời gian, cải thiện hiệu quả nén và chất lượng video
 2. Một expert transformer với adaptive LayerNorm tăng cường sự liên kết text-to-video thông qua việc hợp nhất sâu hơn giữa các phương thức
 3. Các kỹ thuật huấn luyện tiến bộ và multi-resolution frame pack cho phép tạo ra các video dài hơn, mạch lạc với các yếu tố chuyển động đáng kể
CogVideoX cũng được hưởng lợi từ một pipeline xử lý dữ liệu text-to-video hiệu quả với nhiều chiến lược tiền xử lý khác nhau và phương pháp tạo chú thích video chuyên biệt, góp phần vào chất lượng tạo ra cao hơn và sự liên kết ngữ nghĩa tốt hơn. Trọng số của mô hình được công khai, làm cho nó có thể tiếp cận được để triển khai trong các ứng dụng kinh doanh khác nhau, chẳng hạn như demo sản phẩm và nội dung tiếp thị. Sơ đồ sau đây cho thấy kiến trúc của mô hình.

![blog 3](/images/3-Blog/ML-17715-model-architecture-3.png)

---

## Cải tiến nhanh chóng
Để cải thiện chất lượng của video được sinh ra, giải pháp cung cấp tùy chọn nâng cao prompt do người dùng cung cấp. Việc này được thực hiện bằng cách yêu cầu một [large language model](https://aws.amazon.com/what-is/large-language-model/) (LLM), trong trường hợp này là [Anthropic’s Claude](https://aws.amazon.com/bedrock/claude/),lấy [prompt](https://aws.amazon.com/what-is/prompt-engineering/) ban đầu của người dùng và mở rộng nó với các chi tiết bổ sung, tạo ra một mô tả đầy đủ hơn cho việc tạo video. Prompt bao gồm ba phần:

 1. Role section – Xác định mục đích của AI trong việc nâng cao prompt cho video generation
 2. Task section – Chỉ định các hướng dẫn cần thực hiện với prompt gốc
 3. Prompt section – Nơi chèn prompt ban đầu của người dùng
Bằng cách bổ sung thêm các yếu tố mô tả vào prompt ban đầu, hệ thống này nhằm cung cấp các hướng dẫn giàu chi tiết hơn cho các mô hình tạo video, có khả năng dẫn đến các kết quả video chính xác và hấp dẫn hơn về mặt hình ảnh. Chúng tôi sử dụng mẫu prompt sau cho giải pháp này:


```
<Role>
Your role is to enhance the user prompt that is given to you by 
providing additional details to the prompt. The end goal is to
covert the user prompt into a short video clip, so it is necessary 
to provide as much information you can.
</Role>
<Task>
You must add details to the user prompt in order to enhance it for
 video generation. You must provide a 1 paragraph response. No 
more and no less. Only include the enhanced prompt in your response. 
Do not include anything else.
</Task>
<Prompt>
{prompt}
</Prompt>
```

# Yêu cầu tiên quyết

Trước khi triển khai giải pháp, hãy đảm bảo bạn có các yêu cầu tiên quyết sau:

 1. **AWS CDK Toolkit** – Cài đặt [AWS CDK Toolkit](https://docs.aws.amazon.com/cdk/v2/guide/cli.html) toàn cục sử dụng npm:
`npm install -g aws-cdk`
 Điều này cung cấp chức năng cốt lõi để triển khai infrastructure as code lên AWS.
 2. **Docker Desktop** – Điều này cần thiết cho phát triển và kiểm thử local. Nó đảm bảo các container image có thể được build và test local trước khi triển khai.
 3. **AWS CLI** – [AWS Command Line Interface](http://aws.amazon.com/cli) (AWS CLI) phải được cài đặt và cấu hình với credentials phù hợp. Điều này cần một tài khoản AWS với quyền cần thiết. Cấu hình AWS CLI sử dụng `aws configure` với access key và secret của bạn.
 4. **Môi trường Python** – Bạn phải có Python 3.11+ được cài đặt trên hệ thống. Chúng tôi khuyến nghị sử dụng virtual environment để cô lập. Điều này cần thiết cho cả AWS CDK infrastructure và ứng dụng Streamlit.
 5. **Tài khoản AWS hoạt động** – Bạn cần tạo yêu cầu service quota cho SageMaker đến ml.g5.4xlarge cho các processing job.

# Triển khai giải pháp

Giải pháp này đã được kiểm thử trong AWS Region `us-east-1`. Hoàn thành các bước sau để triển khai:

 1. Tạo và kích hoạt virtual environment:
``` Bash
python -m venv .`
venv source .venv/bin/activate
```
 2. Cài đặt các dependency của infrastructure:
```bash
cd infrastructure
pip install -r requirements.txt
```
 3. Bootstrap AWS CDK (nếu chưa thực hiện trong tài khoản AWS của bạn):
``` bash
cdk bootstrap
```
 4. Triển khai infrastructure:
``` bash
cdk deploy -c allowed_ips='["'$(curl -s ifconfig.me)'/32"]'
```
Để truy cập Streamlit UI, chọn link cho StreamlitURL trong log đầu ra AWS CDK sau khi triển khai thành công. Ảnh chụp màn hình sau đây hiển thị Streamlit UI có thể truy cập thông qua URL.

![blog 3](/images/3-Blog/ML-17715-UI.png)

## Tạo video cơ bản
Hoàn thành các bước sau để tạo video:

 1. Nhập prompt ngôn ngữ tự nhiên của bạn vào hộp văn bản ở đầu trang.
 2. Sao chép prompt này vào hộp văn bản ở dưới.
 3. Chọn **Generate Video** để tạo video sử dụng prompt cơ bản này.

Sau đây là đầu ra từ prompt đơn giản `"A bee on a flower."`

![blog 3](/images/3-Blog/ML-17715-video-1-1749587371475.gif)

## Tạo video nâng cao
Để đạt kết quả chất lượng cao hơn, hoàn thành các bước sau:

 1. Nhập prompt ban đầu của bạn vào hộp văn bản trên cùng.
 2. Chọn Enhance Prompt để gửi prompt của bạn đến Amazon Bedrock.
 3. Chờ Amazon Bedrock mở rộng prompt của bạn thành phiên bản mô tả chi tiết hơn.
 4. Xem lại enhanced prompt xuất hiện trong hộp văn bản dưới.
 5. Chỉnh sửa thêm prompt nếu muốn.
 6. Chọn Generate Video để bắt đầu processing job với CogVideoX.

Khi xử lý hoàn tất, video của bạn sẽ xuất hiện trên trang với tùy chọn tải xuống. Sau đây là ví dụ về enhanced prompt và đầu ra:
```python
"""
A vibrant yellow and black honeybee gracefully lands on a large, 
blooming sunflower in a lush garden on a warm summer day. The 
bee's fuzzy body and delicate wings are clearly visible as it 
moves methodically across the flower's golden petals, collecting 
pollen. Sunlight filters through the petals, creating a soft, 
warm glow around the scene. The bee's legs are coated in pollen 
as it works diligently, its antennae twitching occasionally. In 
the background, other colorful flowers sway gently in a light 
breeze, while the soft buzzing of nearby bees can be heard
"""

```
![blog 3](/images/3-Blog/ML-17715-video-2-1749587450779.gif)

## Thêm hình ảnh vào prompt của bạn

Nếu bạn muốn bao gồm hình ảnh cùng với text prompt, hoàn thành các bước sau:

 1. Hoàn thành text prompt và các bước cải tiến tùy chọn.
 2. Chọn **Include an Image.**
 3. Tải lên ảnh bạn muốn sử dụng.
 4. Với cả text và image đã chuẩn bị, chọn **Generate Video** để bắt đầu processing job.

Sau đây là ví dụ về enhanced prompt trước đó với hình ảnh được bao gồm.

![blog 3](/images/3-Blog/ML-17715-video-3-image-1024x682.jpeg)

![blog 3](/images/3-Blog/ML-17715-video-3-1749587457635.gif)

Để xem thêm mẫu, hãy kiểm tra [CogVideoX gallery.](https://github.com/THUDM/CogVideo?tab=readme-ov-file#gallery)

## Dọn dẹp

Để tránh phát sinh chi phí liên tục, hãy dọn dẹp các tài nguyên bạn đã tạo trong bài viết này:

`cdk destroy`

## Các cân nhắc

Mặc dù kiến trúc hiện tại của chúng tôi phục vụ như một proof of concept hiệu quả, nhiều cải tiến được khuyến nghị cho môi trường sản xuất. Các cân nhắc bao gồm triển khai API Gateway với các REST endpoint được AWS Lambda hỗ trợ để cải thiện giao diện và xác thực, giới thiệu kiến trúc dựa trên hàng đợi sử dụng [Amazon Simple Queue Service](https://aws.amazon.com/sqs/) (Amazon SQS) để quản lý job tốt hơn và độ tin cậy, và nâng cao khả năng xử lý lỗi và giám sát.

## Conclusion

Công nghệ tạo video đã xuất hiện như một lực lượng chuyển đổi trong việc tạo nội dung số, như được minh họa qua giải pháp toàn diện dựa trên AWS sử dụng mô hình CogVideoX. Bằng cách kết hợp các dịch vụ mạnh mẽ của AWS như Fargate, SageMaker và Amazon Bedrock với hệ thống nâng cao prompt sáng tạo, chúng tôi đã tạo ra một pipeline có khả năng mở rộng và bảo mật, có thể sản xuất các đoạn video chất lượng cao.
Khả năng của kiến trúc trong việc xử lý cả text-to-video và image-to-video, kèm theo giao diện Streamlit thân thiện với người dùng, khiến nó trở thành công cụ vô giá cho các doanh nghiệp trong các lĩnh vực — từ demo sản phẩm thương mại điện tử đến các chiến dịch tiếp thị cá nhân hóa. Như được thể hiện trong các video mẫu, công nghệ mang lại kết quả ấn tượng, mở ra những hướng đi mới trong sáng tạo hình ảnh và sản xuất nội dung quy mô lớn.
Giải pháp này không chỉ đại diện cho bước tiến công nghệ, mà còn là cái nhìn thoáng qua vào tương lai của kể chuyện hình ảnh và truyền thông số.
Để tìm hiểu thêm về CogVideoX, tham khảo
[CogVideoX on Hugging Face](https://huggingface.co/THUDM/CogVideoX-5b). Hãy thử nghiệm giải pháp và chia sẻ phản hồi của bạn trong phần bình luận.


---
## Về các tác giả
<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/nick-biso.jpg" alt="Yudho Ahmad Diponegoro" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Nick Biso</strong> là một Machine Learning Engineer tại AWS Professional Services. Anh giải quyết các thách thức tổ chức và kỹ thuật phức tạp sử dụng khoa học dữ liệu và kỹ thuật. Ngoài ra, anh xây dựng và triển khai các mô hình AI/ML trên AWS Cloud. Niềm đam mê của anh trải dài đến khả năng du lịch và những trải nghiệm văn hóa đa dạng.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/natasha-tchir.jpeg" alt="Yudho Ahmad Diponegoro" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Natasha Tchir</strong> là Cloud Consultant tại Generative AI Innovation Center, chuyên về machine learning. Với nền tảng vững chắc về ML, hiện tại cô tập trung vào việc phát triển các giải pháp proof-of-concept về generative AI, thúc đẩy sự đổi mới và nghiên cứu ứng dụng trong GenAIIC.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/ML-17715-katherine-feng.jpeg" alt="Le Vy" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Katherine Feng</strong> là Cloud Consultant tại AWS Professional Services trong nhóm Data và ML. Cô có kinh nghiệm rộng rãi trong việc xây dựng các ứng dụng full-stack cho các trường hợp sử dụng AI/ML và các giải pháp điều khiển bởi LLM.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/ML-17715-jinzhao-feng.png" alt="Loke Jun Kai" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Jinzhao Feng</strong> là Machine Learning Engineer tại AWS Professional Services. Anh tập trung vào việc thiết kế kiến trúc và triển khai các giải pháp pipeline generative AI và ML cổ điển quy mô lớn. Anh chuyên về FMOps, LLMOps, và distributed training.</p>
  </div>
</div>