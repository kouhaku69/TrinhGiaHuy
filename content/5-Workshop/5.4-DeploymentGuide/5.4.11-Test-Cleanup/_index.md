---
title: "Test and clean up"
date: "2026-08-07"
weight: 11
chapter: false
pre: "<b> 5.4.11. </b>"
---

# Test, accept, and clean up the workshop

| Layer | Test | Expected result |
|---|---|---|
| EC2 local | localhost and /health | HTTP 200, PostgreSQL Connected |
| Elastic IP | http://54.251.119.230/ | Frontend loads |
| CloudFront | HTTPS root, /health, /api/products | HTTP 200; API/health not cached |
| Cognito | Sign-up through forgot-password | No CORS failure |
| S3 | Upload/view/delete | Correct prefix and access model |
| Bedrock | Vietnamese and English chatbot tests | Valid response |
| CloudWatch | Backend logs/dashboard | New events and metrics |

~~~bash
docker compose ps
curl -f http://localhost/health
curl -f http://54.251.119.230/health
curl -f https://d3pn12mzrv3aqy.cloudfront.net/health
~~~

Acceptance requires healthy containers, a connected database, working CloudFront HTTPS and APIs, corrected login CORS, private RDS access, no public port 5000, no credentials in Git/screenshots, and current CloudWatch telemetry.

## Clean-up order

1. Disable and delete CloudFront if no longer needed.
2. Run docker compose down.
3. Create an RDS final snapshot when required, then delete RDS.
4. Terminate EC2 and release the Elastic IP.
5. Empty and delete approved S3 buckets.
6. Schedule secret deletion according to retention requirements.
7. Delete Cognito, CloudWatch resources, security groups, route tables, subnets, IGW, and VPC.
8. Remove the IAM policy and role only after dependencies are gone.

{{% notice warning %}}
Deleting RDS, S3, Cognito, or secrets can permanently remove data. Confirm the exact resource, backup, and owner before deletion.
{{% /notice %}}

