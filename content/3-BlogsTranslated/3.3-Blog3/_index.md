---
title: "Blog 3"
date: "2026-07-31"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Amazon S3 Lifecycle: Automatically reduce storage costs

**Adapted from AWS Documentation and AWS Storage Blog**  
Topics: Amazon S3, Cost Optimization, Storage

---

Not every object stored in Amazon S3 needs the same level of access forever. Recent logs may be used frequently, older logs may be accessed rarely, and very old logs may no longer be needed.

**Amazon S3 Lifecycle** automates this process with rules.

## Two important actions

### Transition

Move objects to another storage class after a specified age.

```text
Day 0     → S3 Standard
Day 30    → S3 Standard-IA
Day 90    → S3 Glacier Flexible Retrieval
```

### Expiration

Remove objects after they are no longer required.

```text
Day 365 → Expire
```

## Simple example

For a bucket containing website logs:

```text
s3://my-website-logs/
```

a lifecycle policy can keep recent logs in S3 Standard, move older logs to less frequently accessed storage, and expire them after one year.

## Create a lifecycle rule

In the AWS Console:

```text
Amazon S3
→ Buckets
→ Select bucket
→ Management
→ Lifecycle rules
→ Create lifecycle rule
```

Give the rule a name such as:

```text
archive-old-logs
```

Then choose the actions and object scope.

Example:

```text
30 days  → S3 Standard-IA
90 days  → S3 Glacier Flexible Retrieval
365 days → Expire
```

## Important note about small objects

Lifecycle transitions have request costs. AWS currently prevents objects smaller than **128 KB** from transitioning by default because transition charges can outweigh storage savings for very small objects.

Before creating a rule, consider:

```text
Object size
+
Access frequency
+
Retention period
```

## Good use cases

S3 Lifecycle is useful for:

- old logs;
- backups;
- historical data;
- media that becomes rarely accessed;
- long-term archives.

## Main lesson

The key question is not simply:

> “Which S3 storage class is cheapest?”

A better question is:

> “How will this data be accessed over time?”

Lifecycle rules are useful when access changes from frequent to infrequent and eventually to archive or deletion.

## References

- [AWS Documentation – Managing the lifecycle of objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
- [AWS Documentation – Transitioning objects using Amazon S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html)
- [AWS Storage Blog – Optimize storage costs with Amazon S3 Lifecycle](https://aws.amazon.com/blogs/storage/optimize-storage-costs-with-new-amazon-s3-lifecycle-filters-and-actions/)

---

## Conclusion

S3 Lifecycle is a simple way to automate storage cost optimization. Define when data should move, archive, or expire, and Amazon S3 handles the lifecycle automatically.
