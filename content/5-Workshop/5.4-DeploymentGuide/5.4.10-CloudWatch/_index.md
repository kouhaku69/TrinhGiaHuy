---
title: "Configure Amazon CloudWatch"
date: "2026-08-07"
weight: 10
chapter: false
pre: "<b> 5.4.10. </b>"
---

# Collect logs and metrics with CloudWatch

docker-compose.yml configures the awslogs driver:

| Property | Value |
|---|---|
| Region | ap-southeast-1 |
| Log group | /balancoffee/backend |
| Log stream | backend |
| Auto-create | true |

Create the log group in advance and set retention to 14 or 30 days. The EC2 role must be able to write to this group.

Default EC2 metrics do not include memory/disk utilization. Install the unified CloudWatch Agent with Systems Manager or the package for the selected AMI:

~~~json
{
  "agent": {"metrics_collection_interval": 60, "run_as_user": "root"},
  "metrics": {
    "namespace": "CWAgent",
    "append_dimensions": {"InstanceId": "<EC2_INSTANCE_ID>"},
    "metrics_collected": {
      "mem": {"measurement": ["mem_used_percent"]},
      "disk": {"measurement": ["used_percent"], "resources": ["*"]}
    }
  }
}
~~~

Create dashboard balan-coffee-operations with EC2 CPU/network/status, CWAgent memory/disk, RDS CPU/connections/storage, CloudFront requests/errors/cache hit rate, and backend error logs.

![CloudWatch dashboard evidence](/images/5-Workshop/evidence-cloudwatch-dashboard.png)

Recommended initial alarms: EC2 status check >= 1, CPU > 80% for 15 minutes, disk > 80%, low RDS free storage, and CloudFront 5xx > 5% for 5 minutes. Adjust them after collecting a baseline.

~~~text
fields @timestamp, @message
| filter @message like /ERROR|Error|503|AccessDenied/
| sort @timestamp desc
| limit 50
~~~

Never log tokens, passwords, secrets, complete authorization headers, or personal data.

