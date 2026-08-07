---
title: "Configure Amazon Cognito"
date: "2026-08-07"
weight: 6
chapter: false
pre: "<b> 5.4.6. </b>"
---

# Create the Cognito User Pool and App Client

## 1. Source-code requirements

The backend calls the Cognito SDK for registration, email confirmation, login, refresh, logout, and password reset. The active flow does **not use Hosted UI** and requires an App Client secret to calculate SECRET_HASH.

## 2. Create the User Pool

1. Open **Amazon Cognito → User pools → Create user pool**.
2. Name: balan-coffee-users.
3. Sign-in identifier: **Email**.
4. Self-registration: Enabled.
5. Required attributes: email and name when required by the registration form.
6. Email verification: send a verification code with Cognito.
7. MFA: optional/off for the workshop; recommended for administrators.

## 3. Password policy

| Setting | Value |
|---|---|
| Minimum length | At least 8 characters |
| Uppercase/lowercase | Required |
| Number | Required |
| Special character | Required |
| Temporary password validity | 7 days |

## 4. Create the App Client

| Setting | Value |
|---|---|
| Name | balan-coffee-web-client |
| Client type | Confidential/client secret enabled |
| Authentication flow | ALLOW_USER_PASSWORD_AUTH |
| Refresh | ALLOW_REFRESH_TOKEN_AUTH |
| Token revocation | Enabled |
| Prevent user existence errors | Enabled |

Never put the Client Secret in the frontend. Store it in balan-coffee/dev/cognito.

~~~dotenv
COGNITO_USER_POOL_ID=<USER_POOL_ID>
COGNITO_CLIENT_ID=<APP_CLIENT_ID>
COGNITO_SECRET_ID=balan-coffee/dev/cognito
AWS_REGION=ap-southeast-1
~~~

## 5. Test

Test sign-up, email verification, login, /api/auth/me, token refresh, logout, and forgot-password.

{{% notice warning %}}
Earlier logs showed CORS blocked origin for CloudFront. Set CORS_ORIGIN and FRONTEND_URL exactly to https://d3pn12mzrv3aqy.cloudfront.net, recreate the backend, and retest every authentication flow.
{{% /notice %}}

**Evidence:** User Pool overview, password policy, email verification, and App Client authentication flows.

