---
title: "Blog 3"
date: "2025-07-10"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Build a scalable AI video generator using Amazon SageMaker AI and CogVideoX

by Nick Biso, Jinzhao Feng, Katherine Feng, and Natasha Tchir | on JUN 19 2025 | in [Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/sagemaker/), [Amazon SageMaker AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/sagemaker/amazon-sagemaker-ai/), [Artificial Intelligence](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/),[Intermediate (200)](https://aws.amazon.com/blogs/machine-learning/category/learning-levels/intermediate-200/)

---
In recent years, the rapid advancement of artificial intelligence and machine learning (AI/ML) technologies has revolutionized various aspects of digital content creation. One particularly exciting development is the emergence of video generation capabilities, which offer unprecedented opportunities for companies across diverse industries. This technology allows for the creation of short video clips that can be seamlessly combined to produce longer, more complex videos. The potential applications of this innovation are vast and far-reaching, promising to transform how businesses communicate, market, and engage with their audiences. Video generation technology presents a myriad of use cases for companies looking to enhance their visual content strategies. For instance, ecommerce businesses can use this technology to create dynamic product demonstrations, showcasing items from multiple angles and in various contexts without the need for extensive physical photoshoots. In the realm of education and training, organizations can generate instructional videos tailored to specific learning objectives, quickly updating content as needed without re-filming entire sequences. Marketing teams can craft personalized video advertisements at scale, targeting different demographics with customized messaging and visuals. Furthermore, the entertainment industry stands to benefit greatly, with the ability to rapidly prototype scenes, visualize concepts, and even assist in the creation of animated content. The flexibility offered by combining these generated clips into longer videos opens up even more possibilities. Companies can create modular content that can be quickly rearranged and repurposed for different displays, audiences, or campaigns. This adaptability not only saves time and resources, but also allows for more agile and responsive content strategies. As we delve deeper into the potential of video generation technology, it becomes clear that its value extends far beyond mere convenience, offering a transformative tool that can drive innovation, efficiency, and engagement across the corporate landscape.

In this post, we explore how to implement a robust AWS-based solution for video generation that uses the CogVideoX model and [Amazon SageMaker AI](https://aws.amazon.com/sagemaker-ai).

---

## Solution overview

Our architecture delivers a highly scalable and secure video generation solution using AWS managed services. The data management layer implements three purpose-specific [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3) buckets—for input videos, processed outputs, and access logging—each configured with appropriate encryption and lifecycle policies to support data security throughout its lifecycle.

For compute resources, we use [AWS Fargate](https://aws.amazon.com/fargate[) for [Amazon Elastic Container Service](http://aws.amazon.com/ecs) (Amazon ECS) to host the [Streamlit](https://streamlit.io/) web application, providing serverless container management with automatic scaling capabilities. Traffic is efficiently distributed through an [Application Load Balancer](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/). The AI processing pipeline uses SageMaker AI processing jobs to handle video generation tasks, decoupling intensive computation from the web interface for cost optimization and enhanced maintainability. User prompts are refined through [Amazon Bedrock](https://aws.amazon.com/bedrock/), which feeds into the [CogVideoX-5b](https://huggingface.co/THUDM/CogVideoX-5b) model for high-quality video generation, creating an end-to-end solution that balances performance, security, and cost-efficiency.

The following diagram illustrates the solution architecture.

![blog 3](/images/3-Blog/ML-17715-architecture-diagram.png)

## Solution overview
[CogVideoX](https://arxiv.org/abs/2408.06072) is an open source, state-of-the-art text-to-video generation model capable of producing 10-second continuous videos at 16 frames per second with a resolution of 768×1360 pixels. The model effectively translates text prompts into coherent video narratives, addressing common limitations in previous video generation systems.

The model uses three key innovations:

 1. A 3D Variational Autoencoder (VAE) that compresses videos along both spatial and temporal dimensions, improving compression efficiency and video quality
 2. An expert transformer with adaptive LayerNorm that enhances text-to-video alignment through deeper fusion between modalities
 3. Progressive training and multi-resolution frame pack techniques that enable the creation of longer, coherent videos with significant motion elements
CogVideoX also benefits from an effective text-to-video data processing pipeline with various preprocessing strategies and a specialized video captioning method, contributing to higher generation quality and better semantic alignment. The model’s weights are publicly available, making it accessible for implementation in various business applications, such as product demonstrations and marketing content. The following diagram shows the architecture of the model.

![blog 3](/images/3-Blog/ML-17715-model-architecture-3.png)

---

## Prompt enhancement
To improve the quality of video generation, the solution provides an option to enhance user-provided prompts. This is done by instructing a [large language model](https://aws.amazon.com/what-is/large-language-model/) (LLM), in this case [Anthropic’s Claude](https://aws.amazon.com/bedrock/claude/), to take a user’s initial prompt and expand upon it with additional details, creating a more comprehensive description for video creation. The prompt consists of three parts:

 1. Role section – Defines the AI’s purpose in enhancing prompts for video generation
 2. Task section – Specifies the instructions needed to be performed with the original prompt
 3. Prompt section – Where the user’s original input is inserted
By adding more descriptive elements to the original prompt, this system aims to provide richer, more detailed instructions to video generation models, potentially resulting in more accurate and visually appealing video outputs. We use the following prompt template for this solution:

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

# Prerequisites

Before you deploy the solution, make sure you have the following prerequisites:

 1. **The AWS CDK Toolkit** – Install the [AWS CDK Toolkit](https://docs.aws.amazon.com/cdk/v2/guide/cli.html) globally using npm:
`npm install -g aws-cdk`
 This provides the core functionality for deploying infrastructure as code to AWS.
 2. **Docker Desktop** – This is required for local development and testing. It makes sure container images can be built and tested locally before deployment.
 3. **The AWS CLI** – The [AWS Command Line Interface](http://aws.amazon.com/cli) (AWS CLI) must be installed and configured with appropriate credentials. This requires an AWS account with necessary permissions. Configure the AWS CLI using `aws configure` with your access key and secret.
 4. **Python Environment** – You must have Python 3.11+ installed on your system. We recommend using a virtual environment for isolation. This is required for both the AWS CDK infrastructure and Streamlit application.
 5. **Active AWS account** – You will need to raise a service quota request for SageMaker to ml.g5.4xlarge for processing jobs.

# Deploy the solution

This solution has been tested in the `us-east-1` AWS Region. Complete the following steps to deploy:

 1. Create and activate a virtual environment:
``` Bash
python -m venv .`
venv source .venv/bin/activate
```
 2. Install infrastructure dependencies:
```bash
cd infrastructure
pip install -r requirements.txt
```
 3. Bootstrap the AWS CDK (if not already done in your AWS account):
``` bash
cdk bootstrap
```
 4. Deploy the infrastructure:
``` bash
cdk deploy -c allowed_ips='["'$(curl -s ifconfig.me)'/32"]'
```
To access the Streamlit UI, choose the link for StreamlitURL in the AWS CDK output logs after deployment is successful. The following screenshot shows the Streamlit UI accessible through the URL.

![blog 3](/images/3-Blog/ML-17715-UI.png)

## Basic video generation
Complete the following steps to generate a video:

 1. Input your natural language prompt into the text box at the top of the page.
 2. Copy this prompt to the text box at the bottom.
 3. Choose **Generate Video** to create a video using this basic prompt.
The following is the output from the simple prompt `“A bee on a flower.”`

![blog 3](/images/3-Blog/ML-17715-video-1-1749587371475.gif)

## Enhanced video generation
For higher-quality results, complete the following steps:

 1. Enter your initial prompt in the top text box.
 2. Choose Enhance Prompt to send your prompt to Amazon Bedrock.
 3. Wait for Amazon Bedrock to expand your prompt into a more descriptive version.
 4. Review the enhanced prompt that appears in the lower text box.
 5. Edit the prompt further if desired.
 6. Choose Generate Video to initiate the processing job with CogVideoX.

When processing is complete, your video will appear on the page with a download option.The following is an example of an enhanced prompt and output:
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

## Add an image to your prompt

If you want to include an image with your text prompt, complete the following steps:

 1.Complete the text prompt and optional enhancement steps.
 2. Choose **Include an Image.**
 3. Upload the photo you want to use.
 4. With both text and image now prepared, choose **Generate Video** to start the processing job.

The following is an example of the previous enhanced prompt with an included image.

![blog 3](/images/3-Blog/ML-17715-video-3-image-1024x682.jpeg)

![blog 3](/images/3-Blog/ML-17715-video-3-1749587457635.gif)

To view more samples, check out the [CogVideoX gallery.](https://github.com/THUDM/CogVideo?tab=readme-ov-file#gallery)

## Clean up

To avoid incurring ongoing charges, clean up the resources you created as part of this post:

`cdk destroy`

## Considerations

Although our current architecture serves as an effective proof of concept, several enhancements are recommended for a production environment. Considerations include implementing an API Gateway with AWS Lambda backed REST endpoints for improved interface and authentication, introducing a queue-based architecture using [Amazon Simple Queue Service](https://aws.amazon.com/sqs/) (Amazon SQS) for better job management and reliability, and enhancing error handling and monitoring capabilities.

## Conclusion

Video generation technology has emerged as a transformative force in digital content creation, as demonstrated by our comprehensive AWS-based solution using the CogVideoX model. By combining powerful AWS services like Fargate, SageMaker, and Amazon Bedrock with an innovative prompt enhancement system, we’ve created a scalable and secure pipeline capable of producing high-quality video clips. The architecture’s ability to handle both text-to-video and image-to-video generation, coupled with its user-friendly Streamlit interface, makes it an invaluable tool for businesses across sectors—from ecommerce product demonstrations to personalized marketing campaigns. As showcased in our sample videos, the technology delivers impressive results that open new avenues for creative expression and efficient content production at scale. This solution represents not just a technological advancement, but a glimpse into the future of visual storytelling and digital communication.

To learn more about CogVideoX, refer to [CogVideoX on Hugging Face](https://huggingface.co/THUDM/CogVideoX-5b). Try out the solution for yourself, and share your feedback in the comments.

---
## About the Authors
<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/nick-biso.jpg" alt="Yudho Ahmad Diponegoro" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Nick Biso</strong>is a Machine Learning Engineer at AWS Professional Services. He solves complex organizational and technical challenges using data science and engineering. In addition, he builds and deploys AI/ML models on the AWS Cloud. His passion extends to his proclivity for travel and diverse cultural experiences.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/natasha-tchir.jpeg" alt="Yudho Ahmad Diponegoro" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Natasha Tchir</strong> is a Cloud Consultant at the Generative AI Innovation Center, specializing in machine learning. With a strong background in ML, she now focuses on the development of generative AI proof-of-concept solutions, driving innovation and applied research within the GenAIIC.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/ML-17715-katherine-feng.jpeg" alt="Le Vy" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Katherine Feng</strong> is a Cloud Consultant at AWS Professional Services within the Data and ML team. She has extensive experience building full-stack applications for AI/ML use cases and LLM-driven solutions.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 30px;">
  <img src="/images/3-Blog/ML-17715-jinzhao-feng.png" alt="Loke Jun Kai" style="width: 150px; height: 150px; object-fit: cover; margin-right: 20px; border-radius: 8px;">
  <div>
    <p><strong>Jinzhao Feng</strong> is a Machine Learning Engineer at AWS Professional Services. He focuses on architecting and implementing large-scale generative AI and classic ML pipeline solutions. He is specialized in FMOps, LLMOps, and distributed training.</p>
  </div>
</div>