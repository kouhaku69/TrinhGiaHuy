---
title: "Architecture Decisions"
date: "2026-08-07"
weight: 6
chapter: false
pre: "<b> 5.6. </b>"
---

## Architecture Decisions

This section records decisions reflected in the `aws-workshop-v2` repository and the confirmed deployment path.

### ADR-001 — Use one EC2 instance with Docker Compose

**Status:** Accepted for the workshop.

**Decision:** Run the frontend and backend containers on one EC2 instance using Docker Compose.

**Rationale:** The design is easy to understand, deploy, demonstrate, and troubleshoot. It preserves the repository's container definitions and keeps infrastructure cost low.

**Consequences:** The EC2 instance is a single point of failure, scaling is vertical/manual, and deployments can briefly affect both services. A production version should move toward multiple tasks or instances behind an Application Load Balancer.

### ADR-002 — Use Elastic IP as the stable EC2 origin address

**Status:** Accepted for the current deployment.

**Decision:** Associate Elastic IP `54.251.119.230` with the public EC2 instance. CloudFront uses the EC2 public DNS that resolves to this address as its custom origin.

**Rationale:** The EC2 origin must remain stable across normal instance stop/start cycles, while CloudFront requires a resolvable origin hostname rather than a raw IP value in the origin-domain field.

**Consequences:** The EIP remains useful for controlled origin tests, but users should use CloudFront. The current CloudFront-to-origin hop is still HTTP and the one EC2 origin remains a single point of failure.

### ADR-003 — Use Nginx as frontend server and reverse proxy

**Status:** Accepted.

**Decision:** Build the Vite application in a Node.js stage, copy `dist` into Nginx Alpine, and forward API/health/upload paths to `backend:5000`.

**Rationale:** Nginx efficiently serves static files, supports React SPA fallback, and allows same-origin frontend/API access without exposing a second public endpoint.

**Consequences:** Proxy configuration becomes part of the application release and must be tested whenever API routes change.

### ADR-004 — Use Amazon RDS for PostgreSQL

**Status:** Accepted and implemented.

**Decision:** Use PostgreSQL as the only supported runtime database provider and connect through the `pg` pool.

**Rationale:** RDS provides managed backups, patching options, metrics, and relational integrity. The repository includes a migration plan from MongoDB and records validated migrated row counts.

**Consequences:** The application depends on RDS availability and network configuration. Connection limits, schema migrations, backups, and TLS validation must be managed deliberately.

### ADR-005 — Use Cognito for user authentication

**Status:** Accepted and implemented.

**Decision:** Delegate sign-up, email confirmation, sign-in, password reset, and user identity operations to a Cognito User Pool.

**Rationale:** Cognito avoids storing and validating application passwords in the PostgreSQL business database and provides a managed identity service.

**Consequences:** User Pool/App Client configuration and Cognito service availability become runtime dependencies. Client secrets and token validation must be configured consistently.

### ADR-006 — Store application images in S3

**Status:** Accepted and implemented for backend upload/delete operations.

**Decision:** Process images with Sharp and store them in folder prefixes in an S3 bucket.

**Rationale:** Object storage is more suitable than the EC2 filesystem for durable application media and separates media lifecycle from container lifecycle.

**Consequences:** The team must define how browsers securely retrieve objects. Returning a direct S3 URL is not sufficient when objects are private; signed URLs or CloudFront Origin Access Control are future options.

### ADR-007 — Retrieve secrets from AWS Secrets Manager

**Status:** Accepted and implemented.

**Decision:** Load database, SMTP, Cognito client, JWT, and session secret groups at runtime using secret IDs, then cache them in memory.

**Rationale:** Secret values remain outside Git and can be controlled with IAM and audit logs.

**Consequences:** Backend startup depends on Secrets Manager and the EC2 role in production. Rotation requires testing how the in-process cache is refreshed.

### ADR-008 — Use Amazon Bedrock for AI recommendations

**Status:** Accepted and implemented.

**Decision:** Invoke Bedrock with product context and barista knowledge. Default to `apac.amazon.nova-lite-v1:0` unless `BEDROCK_MODEL_ID` is set.

**Rationale:** A managed foundation model adds bilingual recommendation capability without hosting an AI model on EC2.

**Consequences:** Responses have latency and usage cost; model access and output validation are required. The backend expects valid JSON and should continue to handle malformed or unavailable model responses safely.

### ADR-009 — Send backend container logs to CloudWatch Logs

**Status:** Accepted and implemented.

**Decision:** Use Docker's `awslogs` driver with log group `/balancoffee/backend`.

**Rationale:** Centralized logs remain available beyond an individual SSH/session and support search, dashboards, and alarms.

**Consequences:** Container startup/log delivery depends on IAM and region configuration. Log retention must be configured to control cost.

### ADR-010 — Use CloudFront as the viewer endpoint; defer Amazon SES

**Status:** CloudFront accepted and deployed; SES deferred.

**Decision:** Publish the application at `https://d3pn12mzrv3aqy.cloudfront.net` and forward requests to the EC2 origin. Continue using the repository's current Nodemailer/SMTP flow rather than claiming SES.

**Rationale:** Endpoint tests and backend request logs confirm CloudFront is in the active path. It provides viewer HTTPS and static caching. The email service still uses Nodemailer/SMTP rather than the AWS SES SDK.

**Consequences:** API and health behaviors must disable caching; required cookies/headers/query strings must be forwarded; backend CORS must match the CloudFront URL. TLS does not yet extend to the EC2 origin. SES remains a future option.

### Evolution roadmap

| Priority | Improvement | Outcome |
|---:|---|---|
| 1 | Custom domain, WAF, and TLS from CloudFront to an ALB/Nginx origin | Provide end-to-end encryption and stronger edge protection. |
| 2 | Private RDS subnets and stricter egress/ingress | Reduce database exposure. |
| 3 | Multiple application instances/tasks with health-based routing | Remove the EC2 single point of failure. |
| 4 | CI/CD and Infrastructure as Code | Make deployments reviewable and repeatable. |
| 5 | Controlled S3 delivery using signed URLs or CloudFront OAC | Secure private media access. |
| 6 | WAF, backup restore tests, and incident runbooks | Improve security and operational resilience. |
