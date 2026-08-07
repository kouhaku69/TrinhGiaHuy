---
title: "Cấu hình Amazon CloudWatch"
date: "2026-08-07"
weight: 10
chapter: false
pre: "<b> 5.4.10. </b>"
---

# Thu thập log và metrics bằng CloudWatch

## 1. Backend logs

docker-compose.yml đã cấu hình awslogs:

| Thuộc tính | Giá trị |
|---|---|
| Region | ap-southeast-1 |
| Log group | /balancoffee/backend |
| Log stream | backend |
| Auto-create group | true |

Tạo log group trước để chủ động retention:

1. Mở **CloudWatch → Log groups → Create log group**.
2. Name: /balancoffee/backend.
3. Retention: 14 hoặc 30 ngày.

## 2. CloudWatch Agent cho memory/disk

EC2 metrics mặc định không có memory/disk usage. Cài unified CloudWatch Agent bằng Systems Manager hoặc package theo AMI, rồi dùng cấu hình tối thiểu:

~~~json
{
  "agent": {
    "metrics_collection_interval": 60,
    "run_as_user": "root"
  },
  "metrics": {
    "namespace": "CWAgent",
    "append_dimensions": {
      "InstanceId": "<EC2_INSTANCE_ID>"
    },
    "metrics_collected": {
      "mem": {
        "measurement": ["mem_used_percent"]
      },
      "disk": {
        "measurement": ["used_percent"],
        "resources": ["*"]
      }
    }
  }
}
~~~

Khởi động Agent và xác nhận status bằng lệnh phù hợp với hệ điều hành hoặc Systems Manager.

## 3. Dashboard

Tạo dashboard balan-coffee-operations gồm:

+ EC2 CPUUtilization, NetworkIn/Out và StatusCheckFailed.
+ CWAgent memory và disk.
+ RDS CPUUtilization, DatabaseConnections và FreeStorageSpace.
+ CloudFront Requests, 4xx/5xx và CacheHitRate.
+ Logs Insights cho backend ERROR/503.

![Minh chứng CloudWatch dashboard](/images/5-Workshop/evidence-cloudwatch-dashboard.png)

## 4. Alarm đề xuất

| Alarm | Ngưỡng ban đầu |
|---|---|
| EC2 status | StatusCheckFailed >= 1 trong 2 chu kỳ |
| EC2 CPU | > 80% trong 15 phút |
| Disk | > 80% trong 10 phút |
| RDS free storage | Dưới ngưỡng an toàn đã thống nhất |
| CloudFront 5xx | > 5% trong 5 phút |

Ngưỡng phải được điều chỉnh sau khi có baseline.

## 5. Logs Insights

~~~text
fields @timestamp, @message
| filter @message like /ERROR|Error|503|AccessDenied/
| sort @timestamp desc
| limit 50
~~~

Không ghi token, password, secret, authorization header đầy đủ hoặc dữ liệu cá nhân vào log.

