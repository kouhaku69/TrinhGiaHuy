---
title: "Deploy Docker on EC2"
date: "2026-08-07"
weight: 8
chapter: false
pre: "<b> 5.4.8. </b>"
---

# Deploy the frontend and backend with Docker

| Container | Function | Port |
|---|---|---|
| balan-frontend | React/Vite build served by Nginx | 80:80 |
| balan-backend | Node.js/Express API | 5000:5000; not open in the security group |

Nginx proxies /api/, /health, and upload paths to backend:5000. Compose waits for the backend health check before starting the frontend.

## 1. Install and clone

~~~bash
docker --version
docker compose version
git --version
git clone <AUTHORIZED_REPOSITORY_URL> balan-coffee
cd balan-coffee
git checkout aws-workshop-v2
git pull --ff-only origin aws-workshop-v2
git rev-parse --short HEAD
~~~

Record the deployed commit SHA without exposing a GitHub token.

## 2. Create backend/.env

~~~dotenv
NODE_ENV=production
PORT=5000
DATABASE_PROVIDER=postgres
POSTGRES_SSLMODE=no-verify
CORS_ORIGIN=https://d3pn12mzrv3aqy.cloudfront.net
FRONTEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
BACKEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
AWS_REGION=ap-southeast-1
AWS_S3_BUCKET_NAME=<MEDIA_BUCKET>
COGNITO_USER_POOL_ID=<USER_POOL_ID>
COGNITO_CLIENT_ID=<APP_CLIENT_ID>
DATABASE_SECRET_ID=balan-coffee/dev/database
SMTP_SECRET_ID=balan-coffee/dev/smtp
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AUTH_SECRET_ID=balan-coffee/dev/auth
BEDROCK_MODEL_ID=apac.amazon.nova-lite-v1:0
AUTH_COOKIE_SECURE=true
~~~

Do not add access keys or secret values.

## 3. Build, run, and test

~~~bash
git check-ignore backend/.env
docker compose config
docker compose build --pull
docker compose up -d
docker compose ps
docker compose logs --tail=100 backend
curl -i http://localhost/health
curl -i http://localhost/api/products
~~~

The backend must be healthy, PostgreSQL connected, and the frontend running.

![Running container evidence](/images/5-Workshop/evidence-docker-containers.png)

For updates, pull the branch and run docker compose up -d --build. Roll back to the last stable commit when necessary; source rollback does not roll back RDS data/schema.

