---
title: "Monitoring Guide"
date: "2026-08-07"
weight: 5
chapter: false
pre: "<b> 5.5. </b>"
---

## Monitoring Guide

### 1. Monitoring objectives

Monitoring must answer five operational questions:

1. Is the EC2 host reachable and healthy?
2. Are both Docker containers running, and is the backend health endpoint successful?
3. Can the backend connect to RDS and AWS services?
4. Are users seeing elevated errors or response delays?
5. Is CloudFront reaching the origin and serving current content?

### 2. Repository logging configuration

The backend service uses Docker's `awslogs` driver:

| Setting | Value |
|---|---|
| Region | `ap-southeast-1` |
| Log group | `/balancoffee/backend` |
| Log stream | `backend` |
| Create group | `true` |

The EC2 instance role therefore needs permission to create or use this log group and publish log events. If the log driver cannot initialize, the container may fail before the application starts.

### 3. Basic operational checks

```bash
docker compose ps
docker inspect --format '{{json .State.Health}}' balan-backend
curl -f http://localhost/health
curl -f http://54.251.119.230/health
curl -f https://d3pn12mzrv3aqy.cloudfront.net/health
```

Use `docker compose logs --tail=100 backend` for immediate diagnosis. CloudWatch Logs is the centralized source for historical backend output.

### 4. CloudWatch dashboard

Create a dashboard with at least:

| Widget | Metric/source | Purpose |
|---|---|---|
| EC2 CPU | `AWS/EC2 – CPUUtilization` | Detect sustained compute pressure. |
| EC2 network | `NetworkIn`, `NetworkOut` | Identify traffic changes or network anomalies. |
| EC2 status | `StatusCheckFailed` | Detect host or instance failure. |
| RDS CPU | `AWS/RDS – CPUUtilization` | Detect database saturation. |
| RDS connections | `DatabaseConnections` | Detect pool leaks or capacity pressure. |
| RDS storage | `FreeStorageSpace` | Prevent storage exhaustion. |
| RDS latency | Read/write latency | Detect slow database operations. |
| Backend logs | CloudWatch Logs Insights | Count errors and review request patterns. |
| CloudFront | Requests, cache hit, 4xx/5xx | Detect edge/origin and caching problems. |

The supplied dashboard confirms that the CloudWatch Agent is already publishing memory and disk metrics in addition to standard EC2 CPU, network, and status-check metrics.

![CloudWatch EC2 dashboard evidence](/images/5-Workshop/evidence-cloudwatch-dashboard.png)

Observed over the selected three-hour window on 2026-08-07: memory was approximately 15.3%–19.1%, disk increased from approximately 62.6% to 64.4%, standard CPU utilization was mostly below 1% with brief peaks near 7.37%, and `StatusCheckFailed` remained 0. These figures describe the captured window only and are not long-term capacity baselines.

### 5. Recommended alarms

Tune thresholds after observing a normal baseline. Suggested starting points:

| Alarm | Starting condition | Response |
|---|---|---|
| EC2 status check | `StatusCheckFailed >= 1` for 2 periods | Investigate or recover the instance. |
| EC2 CPU | Above 80% for 15 minutes | Inspect traffic and container usage; right-size if persistent. |
| RDS storage | Below an agreed free-space threshold | Increase storage or remove unnecessary data. |
| RDS connections | Near the safe connection limit | Inspect pool configuration and stuck requests. |
| Backend errors | Repeated `ERROR`, `503`, or uncaught exceptions | Correlate with RDS and AWS API events. |
| No logs | No backend events during expected active traffic | Check container state, IAM, and log driver. |
| CloudFront 5xx | Above 5% for 5 minutes as an initial threshold | Check origin reachability, EC2/Nginx health, and behavior configuration. |

Alarm actions should notify the project owner through an approved channel. Record the recipient and escalation owner in the final report.

### 6. Logs Insights examples

Recent errors:

```text
fields @timestamp, @message
| filter @message like /ERROR|Error|503/
| sort @timestamp desc
| limit 50
```

Health-check events:

```text
fields @timestamp, @message
| filter @message like /Health check|health/
| sort @timestamp desc
| limit 50
```

Do not log passwords, tokens, secret values, full authorization headers, or sensitive customer information.

### 7. Incident triage sequence

1. Check the CloudFront distribution status, behavior, and 4xx/5xx metrics.
2. Test CloudFront `/health`, then compare it with the direct Elastic IP and local health endpoints.
3. Confirm the Elastic IP is associated with `i-03642ee2788132cb3` and review EC2 status checks.
4. Check the EC2 Security Group, route table, and CloudFront origin configuration.
5. Run `docker compose ps` and review recent backend logs in CloudWatch.
6. Inspect RDS availability, connections, storage, and security group rules.
7. Check Secrets Manager, Cognito, S3, or Bedrock permissions only when the failure concerns that integration.
8. Roll back the application revision if the incident began immediately after deployment.

### 8. Service-specific symptoms

| Symptom | Likely area | First check |
|---|---|---|
| Website does not load | CloudFront, origin, SG, Nginx, frontend | Compare CloudFront, EIP, local health and `docker compose ps`. |
| Login/refresh blocked | Backend CORS or CloudFront cookie forwarding | Exact `CORS_ORIGIN`, `FRONTEND_URL`, API behavior and cookies. |
| `/health` returns 503 | RDS or secret loading | RDS status, `POSTGRES_URI`, port 5432 rule. |
| Login fails | Cognito configuration | User Pool/App Client IDs and EC2 role permissions. |
| Image upload fails | S3 | Bucket name, region, IAM actions, Block Public Access delivery design. |
| Chatbot fails | Bedrock | Model ID/inference profile and `bedrock:InvokeModel`. |
| Container logs absent | CloudWatch Logs | EC2 role and `awslogs` settings. |

### 9. Evidence checklist

+ [ ] CloudWatch log group and recent backend events.
+ [x] EC2 metrics dashboard and status checks supplied.
+ [ ] RDS metrics graph.
+ [ ] Dashboard containing the agreed widgets.
+ [ ] At least one tested alarm.
+ [ ] Successful local and public health response.
+ [ ] CloudFront request/error/cache metrics and corrected authentication test.
+ [ ] Screenshots exclude credentials and customer data.
