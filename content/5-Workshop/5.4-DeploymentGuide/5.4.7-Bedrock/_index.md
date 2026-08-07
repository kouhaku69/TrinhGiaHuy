---
title: "Configure Amazon Bedrock"
date: "2026-08-07"
weight: 7
chapter: false
pre: "<b> 5.4.7. </b>"
---

# Enable Amazon Bedrock

backend/services/bedrockService.js uses the Bedrock Runtime Converse API with:

~~~dotenv
AWS_REGION=ap-southeast-1
BEDROCK_MODEL_ID=apac.amazon.nova-lite-v1:0
~~~

1. Open Amazon Bedrock in the Singapore Region.
2. Open **Model catalog** or **Model access**, depending on the current console.
3. Confirm that Amazon Nova Lite or its inference profile is available to the account.
4. Request access only to the model used by the application.

The EC2 role needs bedrock:InvokeModel and, when streaming is used, bedrock:InvokeModelWithResponseStream. Restrict Resource to the actual model or inference-profile ARN after initial validation.

Test one Vietnamese and one English coffee-consultation prompt. The response must contain valid reply and recommendedIds values. Inspect CloudWatch for AccessDeniedException, ValidationException, or throttling.

Do not send secrets, payment data, or personal data in prompts. Monitor Bedrock requests and cost.

