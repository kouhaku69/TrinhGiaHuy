---
title: "Environment Setup"
date: "2026-08-07"
weight: 3
chapter: false
pre: "<b> 5.3. </b>"
---

## Environment Setup

{{% notice info %}}
This page is the pre-deployment checklist. Detailed AWS Console procedures for VPC, IAM/EC2, RDS, Secrets Manager, S3, Cognito, Bedrock, CloudFront, and CloudWatch are provided in [5.4 — AWS configuration and deployment](../5.4-DeploymentGuide/).
{{% /notice %}}

### 1. Prerequisites

+ Access to the private GitHub repository `trinpce192008/AWS_Workshop` and branch `aws-workshop-v2`.
+ An AWS account with permission to create or configure VPC, EC2, Elastic IP, IAM, RDS, S3, Cognito, Bedrock, Secrets Manager, CloudFront, and CloudWatch resources.
+ AWS Region `ap-southeast-1` unless the environment variables and resources are intentionally changed.
+ Docker Engine with the Docker Compose plugin on EC2.
+ A reviewed plan for storing production secret values.

### 2. Record the actual environment

The repository does not define every infrastructure identifier. Complete this table with values from the AWS Console before final submission.

| Item | Repository-confirmed/default value | Actual value |
|---|---|---|
| Region | `ap-southeast-1` | |
| EC2 instance ID/type/AZ | Console evidence | `i-03642ee2788132cb3` / `t3.medium` / `ap-southeast-1a` |
| Elastic IP | Console and endpoint test | `54.251.119.230` |
| CloudFront domain | Deployed distribution | `d3pn12mzrv3aqy.cloudfront.net` |
| VPC and public subnet | Not stored in the repository | |
| EC2 Security Group | Not stored in the repository | |
| RDS identifier | `balan-coffee-postgres-dev` in migration report | |
| RDS database | `balancoffee` | |
| RDS class | `db.t4g.micro` in migration report | |
| S3 bucket | `AWS_S3_BUCKET_NAME` | |
| Cognito User Pool/App Client | Environment variables | |
| Secrets | `balan-coffee/dev/database`, `.../smtp`, `.../cognito`; Auth secret ID must also be recorded | |
| CloudWatch log group | `/balancoffee/backend` | |

### 3. Network preparation

1. Select a VPC in `ap-southeast-1`.
2. Create or select a public subnet with automatic public IPv4 assignment as appropriate.
3. Attach an Internet Gateway to the VPC.
4. Add `0.0.0.0/0 → Internet Gateway` to the public subnet route table.
5. Create an EC2 Security Group:
   + During origin testing, TCP 80 from the administrator/test source; after validation, prefer the CloudFront origin-facing managed prefix list.
   + TCP 22 only from a trusted administrator IP, or omit it when using Session Manager.
   + No public TCP 5000 rule.
6. Create an RDS Security Group that accepts TCP 5432 from the EC2 Security Group.

### 4. EC2 IAM role

Attach an instance profile to EC2. Grant only the actions required by the current implementation:

| Service | Required actions |
|---|---|
| Secrets Manager | `secretsmanager:GetSecretValue` for the four application secret ARNs. |
| S3 | `s3:PutObject`, `s3:DeleteObject`, and any required read action for the selected bucket/prefixes. |
| Cognito IDP | The user-management actions used by sign-up, confirmation, authentication, reset, and admin flows. |
| Bedrock | `bedrock:InvokeModel` for the selected inference profile/model. |
| CloudWatch Logs | `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`, and describe actions needed by the Docker log driver. |
| Systems Manager | Managed instance permissions when Session Manager is used. |

Do not place long-lived `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` values in the production `.env`. The AWS SDK automatically uses the EC2 role when static credentials are absent.

### 5. RDS PostgreSQL preparation

1. Create or select the PostgreSQL instance.
2. Create database `balancoffee`.
3. Apply the schema from `backend/database/postgres/schema.sql` if that file is present in the deployment revision.
4. Keep backups enabled and record the retention period.
5. For the recommended target, place RDS in private subnets and allow port 5432 only from the EC2 Security Group.
6. Store the connection string as JSON in the database secret:

```json
{
  "POSTGRES_URI": "postgresql://USERNAME:PASSWORD@RDS_ENDPOINT:5432/balancoffee?sslmode=no-verify"
}
```

### 6. Application secrets

Create JSON secrets without committing their values:

| Environment selector | Expected JSON keys |
|---|---|
| `DATABASE_SECRET_ID` | `POSTGRES_URI` |
| `SMTP_SECRET_ID` | `EMAIL_HOST`, `EMAIL_PORT`, `EMAIL_USER`, `EMAIL_PASSWORD`, optionally `EMAIL_FROM` |
| `COGNITO_SECRET_ID` | `COGNITO_CLIENT_SECRET` |
| `AUTH_SECRET_ID` | `JWT_SECRET`, `SESSION_SECRET` |

### 7. S3, Cognito, and Bedrock

#### S3

+ Create the image bucket in the selected region.
+ Keep Block Public Access enabled unless the team has approved a public-read design.
+ Add a bucket CORS rule only when browser-direct access is required; the current upload path is backend-to-S3.
+ Set `AWS_S3_BUCKET_NAME` to the exact bucket name.

#### Cognito

+ Create the User Pool and App Client required by the backend.
+ Record `COGNITO_USER_POOL_ID` and `COGNITO_CLIENT_ID`.
+ If the client has a secret, store it in `COGNITO_SECRET_ID`.
+ Align verification and password policies with the application's user flows.

#### Bedrock

+ Confirm model access/inference profile availability for `apac.amazon.nova-lite-v1:0` or set `BEDROCK_MODEL_ID` to an approved alternative.
+ Grant the EC2 role permission only to invoke the selected model.

#### CloudFront

+ Use the EC2 public DNS that resolves to `54.251.119.230` as the custom origin domain.
+ Use HTTP port 80 to the current origin and redirect viewers from HTTP to HTTPS.
+ Use `CachingDisabled` for `/api/*` and `/health`; forward the required cookies, headers, query strings, and HTTP methods for `/api/*`.
+ Use `index.html` as the default root object and configure SPA fallback when needed.

### 8. Configuration files

Create the frontend `.env` before building:

```dotenv
VITE_API_URL=/api
VITE_APP_URL=https://d3pn12mzrv3aqy.cloudfront.net
VITE_APP_NAME=Balan Coffee & Roastery
```

The current `src/config/api.js` intentionally uses an empty production base URL, so browser API calls remain same-origin and are proxied by Nginx even when no absolute API host is supplied.

Create `backend/.env` from the example and use secret identifiers rather than raw production secrets:

```dotenv
NODE_ENV=production
PORT=5000
DATABASE_PROVIDER=postgres
POSTGRES_SSLMODE=no-verify
CORS_ORIGIN=https://d3pn12mzrv3aqy.cloudfront.net
FRONTEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
BACKEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
AWS_REGION=ap-southeast-1
AWS_S3_BUCKET_NAME=<BUCKET_NAME>
COGNITO_USER_POOL_ID=<USER_POOL_ID>
COGNITO_CLIENT_ID=<APP_CLIENT_ID>
DATABASE_SECRET_ID=balan-coffee/dev/database
SMTP_SECRET_ID=balan-coffee/dev/smtp
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AUTH_SECRET_ID=balan-coffee/dev/auth
BEDROCK_MODEL_ID=apac.amazon.nova-lite-v1:0
AUTH_COOKIE_SECURE=true
```

The production viewer endpoint is HTTPS, so `AUTH_COOKIE_SECURE=true` is required. The supplied log evidence shows CloudFront-origin authentication requests were blocked by CORS; verify the exact values above, then run `docker compose up -d --force-recreate backend`.

### 9. Readiness checklist

+ [ ] EC2 can reach RDS on port 5432.
+ [ ] EC2 can call Secrets Manager, S3, Cognito, Bedrock, and CloudWatch over HTTPS.
+ [ ] Elastic IP is allocated and ready to associate with EC2.
+ [ ] CloudFront points to the EC2 public DNS and the API/health behaviors have caching disabled.
+ [ ] `CORS_ORIGIN` and `FRONTEND_URL` exactly match the CloudFront HTTPS URL.
+ [ ] `backend/.env` exists on EC2 and is not tracked by Git.
+ [ ] No AWS access keys or secret values are present in the repository.
+ [ ] RDS snapshot/backup and clean-up ownership are recorded.
