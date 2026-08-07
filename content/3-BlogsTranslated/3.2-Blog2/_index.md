---
title: "Blog 2"
date: "2026-07-31"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Automatically start and stop EC2 with Amazon EventBridge Scheduler without writing Lambda

**Adapted from AWS Compute Blog, AWS DevOps Blog, and AWS Documentation** | Topics: Amazon EventBridge Scheduler, EC2, Automation, Cost Optimization, IAM

---

One of the easiest ways to waste money on AWS is to leave a development environment running 24/7 even though the team only works for 8–10 hours per day.

Assume an EC2 instance is needed only:

```text
Monday-Friday
08:00-18:00
```

Relying on someone to remember to stop it manually is not a reliable control.

Amazon EventBridge Scheduler solves this problem by creating one-time or recurring schedules that call AWS service APIs. With universal targets, you can call EC2 `StartInstances` and `StopInstances` directly instead of writing a Lambda function whose only job is to execute a few SDK calls.

Primary sources:

- [AWS Compute Blog – Introducing Amazon EventBridge Scheduler](https://aws.amazon.com/blogs/compute/introducing-amazon-eventbridge-scheduler/)
- [AWS DevOps Blog – EventBridge Scheduler L2 Construct](https://aws.amazon.com/blogs/devops/announcing-the-general-availability-of-the-amazon-eventbridge-scheduler-l2-construct/)
- [AWS Documentation – Amazon EventBridge Scheduler](https://docs.aws.amazon.com/eventbridge/latest/userguide/using-eventbridge-scheduler.html)
- [AWS What’s New – EventBridge Scheduler adds 619 new SDK API actions](https://aws.amazon.com/about-aws/whats-new/2026/05/amazon-eventbridge-sdk-integrations/)

---

## 1. Why not just use cron on EC2?

A familiar approach is:

```bash
crontab -e
```

and a script that calls the AWS CLI.

The problem is that the server running cron also has to stay available, be patched, have credentials, and remain reliable.

Another approach is:

```text
EventBridge Rule
-> Lambda
-> EC2 API
```

This works, but if Lambda only calls `StopInstances`, it adds a compute and code layer that may not be necessary.

EventBridge Scheduler can simplify the path to:

```text
Schedule
-> EC2 StopInstances API
```

---

## 2. What is EventBridge Scheduler?

EventBridge Scheduler is a managed serverless scheduler.

It supports:

```text
at(...)
rate(...)
cron(...)
```

### One-time

```text
at(2026-08-01T10:00:00)
```

### Rate

```text
rate(15 minutes)
```

### Cron

```text
cron(0 18 ? * MON-FRI *)
```

It also supports time zones, allowing you to configure:

```text
Asia/Ho_Chi_Minh
```

instead of manually converting every schedule to UTC.

---

## 3. Lab architecture

Goal:

```text
08:00 -> Start EC2
18:00 -> Stop EC2
Monday-Friday
Asia/Ho_Chi_Minh
```

Flow:

```text
EventBridge Scheduler
        |
        | assumes execution role
        v
       IAM Role
        |
        | ec2:StartInstances
        | ec2:StopInstances
        v
     EC2 Instance
```

Create two schedules:

1. `start-dev-ec2`
2. `stop-dev-ec2`

---

## 4. Step 1: Prepare an EC2 instance

Create a lab EC2 instance and save its Instance ID:

```text
i-0123456789abcdef0
```

Do not test this workflow directly on a production instance.

---

## 5. Step 2: Create an IAM execution role

EventBridge Scheduler needs to assume an IAM role before it can call EC2.

### Trust policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "scheduler.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Permission policy

Grant only the actions needed and scope them to the target instance:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:<region>:<account-id>:instance/i-0123456789abcdef0"
    }
  ]
}
```

Important idea:

> Scheduler does not automatically have permission to stop EC2. It can only perform actions allowed by its execution role.

---

## 6. Step 3: Start EC2 at 08:00

In the AWS Console:

```text
Amazon EventBridge
-> Scheduler
-> Create schedule
```

Example configuration:

```text
Name: start-dev-ec2
Schedule type: Recurring
Cron: cron(0 8 ? * MON-FRI *)
Time zone: Asia/Ho_Chi_Minh
Flexible time window: Off
```

Choose the EC2 API target corresponding to:

```text
StartInstances
```

Input:

```json
{
  "InstanceIds": [
    "i-0123456789abcdef0"
  ]
}
```

Select the execution role created earlier.

---

## 7. Step 4: Stop EC2 at 18:00

Create a second schedule:

```text
Name: stop-dev-ec2
Cron: cron(0 18 ? * MON-FRI *)
Time zone: Asia/Ho_Chi_Minh
```

Target:

```text
EC2 StopInstances
```

Input:

```json
{
  "InstanceIds": [
    "i-0123456789abcdef0"
  ]
}
```

Use the same execution role.

---

## 8. Why this can reduce cost

If a development instance runs only 10 hours per day, 5 days per week:

```text
10 x 5 = 50 hours/week
```

Running continuously would be:

```text
24 x 7 = 168 hours/week
```

The active compute time becomes:

```text
50 / 168 ≈ 29.8%
```

So more than 70% of unnecessary **running hours** can be removed.

That does not mean the total AWS bill always falls by exactly 70%, because:

- EBS volumes still incur storage cost while an instance is stopped;
- public IPv4 or other networking resources can still cost money;
- snapshots and data transfer are separate;
- some workloads genuinely need after-hours availability.

But for dev/test environments, scheduling is one of the easiest ways to remove idle compute.

---

## 9. Scheduler is not only for EC2

EventBridge Scheduler supports templated and universal targets across many AWS APIs.

Examples:

```text
Scheduler -> SNS
Scheduler -> SQS
Scheduler -> Lambda
Scheduler -> Step Functions
Scheduler -> ECS task
Scheduler -> AWS service API
```

In May 2026, AWS expanded Scheduler with hundreds of additional SDK API actions, allowing more operations to be scheduled directly without custom integration code.

---

## 10. Reliability: retries and DLQ matter

Production scheduling is not only about executing at the right time.

You also need to ask:

```text
What if the API call fails?
What if the target is temporarily unavailable?
What happens after retries?
```

EventBridge Scheduler supports:

- retry policies;
- event retention;
- flexible time windows;
- dead-letter queues using Amazon SQS;
- encryption.

For important workflows, configure a DLQ so failed schedules remain observable.

Scheduler uses an **at-least-once** delivery model, so targets should be designed to tolerate duplicate invocations.

---

## 11. Avoid overly broad IAM

Do not create an execution role like:

```json
{
  "Effect": "Allow",
  "Action": "ec2:*",
  "Resource": "*"
}
```

just because it is faster.

For a one-instance lab, scope the role to:

```text
Actions:
- ec2:StartInstances
- ec2:StopInstances

Resource:
- the specific EC2 instance ARN
```

Automated permissions deserve even more care because the action can repeat without a human watching.

---

## 12. When to use EventBridge Scheduler

Good fit when:

- the task has a time-based trigger;
- you need one-time or recurring schedules;
- you want to call AWS services without maintaining a cron server;
- time-zone support matters;
- you want managed retries and DLQ;
- you need centralized schedule management.

Use cases include:

- start/stop development EC2;
- send SNS reminders;
- run Step Functions nightly;
- trigger ECS tasks periodically;
- schedule one-time future events.

---

## 13. When not to use it

Do not use Scheduler as a replacement for load-based autoscaling.

If capacity should change according to CPU, request count, or queue depth, use services such as:

- EC2 Auto Scaling;
- Application Auto Scaling;
- ECS Service Auto Scaling.

Scheduler answers:

> “When should this action happen?”

Autoscaling answers:

> “How much capacity do I need?”

They are different problems.

---

## 14. Cleanup

After the lab:

1. Delete both schedules.
2. Delete the IAM execution role if no longer needed.
3. Terminate the lab EC2 instance.
4. Check EBS volumes, public IPv4/Elastic IP resources, and snapshots.
5. Review Billing/Cost Explorer.

For one-time schedules in other use cases, you can use `ActionAfterCompletion=DELETE` so the schedule is removed automatically after completion.

---

## References

- AWS Compute Blog: [Introducing Amazon EventBridge Scheduler](https://aws.amazon.com/blogs/compute/introducing-amazon-eventbridge-scheduler/)
- AWS DevOps Blog: [Announcing the General Availability of the Amazon EventBridge Scheduler L2 Construct](https://aws.amazon.com/blogs/devops/announcing-the-general-availability-of-the-amazon-eventbridge-scheduler-l2-construct/)
- AWS Documentation: [Amazon EventBridge Scheduler](https://docs.aws.amazon.com/eventbridge/latest/userguide/using-eventbridge-scheduler.html)
- AWS Documentation: [Managing targets in EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-targets.html)
- AWS What’s New: [EventBridge Scheduler adds 619 new SDK API actions](https://aws.amazon.com/about-aws/whats-new/2026/05/amazon-eventbridge-sdk-integrations/)

---

## Conclusion

Good automation does not always need Lambda, containers, or a server running cron.

If the requirement is simply:

```text
At this time
-> call an AWS API
```

EventBridge Scheduler may be the cleanest layer.

The main lesson is: **before writing more automation code, check whether a managed AWS service can perform the action directly**. Less code usually means fewer things to deploy, monitor, and debug.
