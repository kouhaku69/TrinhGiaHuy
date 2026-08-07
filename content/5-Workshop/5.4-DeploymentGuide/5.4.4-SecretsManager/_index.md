---
title: "Configure AWS Secrets Manager"
date: "2026-08-07"
weight: 4
chapter: false
pre: "<b> 5.4.4. </b>"
---

# Store sensitive configuration in AWS Secrets Manager

backend/config/runtimeConfig.js reads four Secret IDs, calls GetSecretValue, parses JSON, and caches the values. In production, a missing required secret prevents the backend from starting.

Create four **Other type of secret** JSON secrets:

### balan-coffee/dev/database

~~~json
{"POSTGRES_URI":"postgresql://<USER>:<PASSWORD>@<RDS_ENDPOINT>:5432/balancoffee?sslmode=no-verify"}
~~~

### balan-coffee/dev/smtp

~~~json
{
  "EMAIL_HOST": "<SMTP_HOST>",
  "EMAIL_PORT": "587",
  "EMAIL_USER": "<SMTP_USER>",
  "EMAIL_PASSWORD": "<SMTP_PASSWORD>",
  "EMAIL_FROM": "<FROM_ADDRESS>"
}
~~~

### balan-coffee/dev/cognito

~~~json
{"COGNITO_CLIENT_SECRET":"<APP_CLIENT_SECRET>"}
~~~

### balan-coffee/dev/auth

~~~json
{
  "JWT_SECRET": "<LONG_RANDOM_VALUE>",
  "SESSION_SECRET": "<DIFFERENT_LONG_RANDOM_VALUE>"
}
~~~

The AWS managed Secrets Manager key is sufficient for the workshop. If a customer-managed KMS key is selected, add kms:Decrypt to the EC2 role. Do not enable rotation until the application has a tested synchronization procedure.

~~~dotenv
DATABASE_SECRET_ID=balan-coffee/dev/database
SMTP_SECRET_ID=balan-coffee/dev/smtp
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AUTH_SECRET_ID=balan-coffee/dev/auth
~~~

Validate metadata without printing SecretString:

~~~bash
aws secretsmanager describe-secret --secret-id balan-coffee/dev/database
docker compose logs --tail=100 backend
~~~

