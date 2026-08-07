---
title: "Configure Amazon CloudFront"
date: "2026-08-07"
weight: 9
chapter: false
pre: "<b> 5.4.9. </b>"
---

# Create the CloudFront distribution for the EC2 origin

1. Open **CloudFront → Distributions → Create distribution**.
2. Select an Other/custom origin.
3. Use the EC2 public DNS that resolves to Elastic IP 54.251.119.230. Do not enter a raw IP in Origin domain.
4. Use HTTP-only port 80 for the current origin.
5. Set viewer policy to **Redirect HTTP to HTTPS**.
6. Set the default root object to index.html and enable compression.

The active endpoint is:

~~~text
https://d3pn12mzrv3aqy.cloudfront.net
~~~

## Default frontend behavior

| Setting | Value |
|---|---|
| Path | Default (*) |
| Methods | GET, HEAD, OPTIONS |
| Cache policy | CachingOptimized or equivalent static policy |
| Viewer policy | Redirect HTTP to HTTPS |

## Ordered dynamic behaviors

| Path | Methods | Cache | Origin request |
|---|---|---|---|
| /api/* | All required methods | CachingDisabled | Required cookies, query strings, and headers |
| /health | GET, HEAD | CachingDisabled | Minimal |
| /uploads/* | GET, HEAD | Disabled or short TTL | According to media design |
| /backend/uploads/* | GET, HEAD | Disabled or short TTL | According to media design |

Authentication uses cookies and credentials: true, so /api/* must forward the required cookies.

~~~dotenv
CORS_ORIGIN=https://d3pn12mzrv3aqy.cloudfront.net
FRONTEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
BACKEND_URL=https://d3pn12mzrv3aqy.cloudfront.net
AUTH_COOKIE_SECURE=true
~~~

~~~bash
docker compose up -d --force-recreate backend
curl -I https://d3pn12mzrv3aqy.cloudfront.net/
curl -i https://d3pn12mzrv3aqy.cloudfront.net/health
curl -i https://d3pn12mzrv3aqy.cloudfront.net/api/products
~~~

Retest login/refresh because earlier logs showed a blocked CORS origin.

When direct EIP testing is no longer needed, restrict port 80 with the CloudFront origin-facing managed prefix list. The current viewer connection is HTTPS, but the CloudFront-to-EC2 hop remains HTTP.

**Evidence:** distribution overview, origin, default behavior, /api/* behavior, and /health result.

