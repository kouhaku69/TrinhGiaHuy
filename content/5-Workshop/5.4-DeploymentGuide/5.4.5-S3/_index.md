---
title: "Configure Amazon S3"
date: "2026-08-07"
weight: 5
chapter: false
pre: "<b> 5.4.5. </b>"
---

# Create the application media bucket

## 1. Create the bucket

1. Open **Amazon S3 → Create bucket**.
2. Recommended name: balan-coffee-media-ACCOUNT_ID.
3. Region: ap-southeast-1.
4. Object Ownership: **ACLs disabled**.
5. Default encryption: SSE-S3 or SSE-KMS.
6. Enable versioning when image recovery is required.

The backend stores objects under products/, users/, blogs/, categories/, logos/, banners/, and temp/.

## 2. EC2 role and environment

Grant s3:GetObject, s3:PutObject, and s3:DeleteObject on arn:aws:s3:::MEDIA_BUCKET/*.

~~~dotenv
AWS_S3_BUCKET_NAME=<MEDIA_BUCKET>
AWS_REGION=ap-southeast-1
~~~

## 3. Select the read model

The current source returns direct S3 URLs. Choose and document one model:

| Model | Configuration | Assessment |
|---|---|---|
| Private bucket | Keep Block Public Access; use presigned URLs or a media CloudFront origin | Recommended |
| Public-read prefix | Permit read access only to explicitly public image prefixes | Only for genuinely public assets |

Do not disable all Block Public Access without approval. The current CloudFront distribution uses an **EC2 custom origin**, not S3 OAC.

The current Express upload path does not require S3 CORS. Add CORS only if the browser uploads directly to S3.

## 4. Test

Upload a JPEG/PNG/WebP file below 10 MB, verify the prefix, view it using the selected access model, delete it, and inspect CloudWatch logs.

**Evidence:** bucket Properties, Permissions, encryption, and a non-sensitive sample object.

