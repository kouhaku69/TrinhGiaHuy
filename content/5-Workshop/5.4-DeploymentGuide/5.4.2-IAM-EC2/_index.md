---
title: "Configure IAM, EC2, and the Elastic IP"
date: "2026-08-07"
weight: 2
chapter: false
pre: "<b> 5.4.2. </b>"
---

# Create the IAM role, EC2 instance, and Elastic IP

## 1. Create the EC2 role

1. Open **IAM → Roles → Create role**.
2. Select **AWS service → EC2**.
3. Name the role BalanCoffeeEC2Role.
4. Attach CloudWatchAgentServerPolicy.
5. Add BalanCoffeeAppPolicy and replace all placeholders:

~~~json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadApplicationSecrets",
      "Effect": "Allow",
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "arn:aws:secretsmanager:ap-southeast-1:<ACCOUNT_ID>:secret:balan-coffee/*"
    },
    {
      "Sid": "UseMediaBucket",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::<MEDIA_BUCKET>/*"
    },
    {
      "Sid": "UseCognito",
      "Effect": "Allow",
      "Action": [
        "cognito-idp:SignUp",
        "cognito-idp:ConfirmSignUp",
        "cognito-idp:ResendConfirmationCode",
        "cognito-idp:InitiateAuth",
        "cognito-idp:GetTokensFromRefreshToken",
        "cognito-idp:RevokeToken",
        "cognito-idp:ForgotPassword",
        "cognito-idp:ConfirmForgotPassword",
        "cognito-idp:ChangePassword",
        "cognito-idp:GlobalSignOut"
      ],
      "Resource": "*"
    },
    {
      "Sid": "InvokeBedrock",
      "Effect": "Allow",
      "Action": ["bedrock:InvokeModel", "bedrock:InvokeModelWithResponseStream"],
      "Resource": "*"
    },
    {
      "Sid": "WriteBackendLogs",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:DescribeLogStreams",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:ap-southeast-1:<ACCOUNT_ID>:log-group:/balancoffee/backend:*"
    }
  ]
}
~~~

After validation, restrict Cognito and Bedrock resources to the actual User Pool and model/inference profile. The instance profile supplies temporary credentials; do not store static access keys in production.

## 2. Launch EC2

| Setting | Value |
|---|---|
| Name | balan-coffee-app |
| AMI | Amazon Linux 2023 or Ubuntu LTS; record the actual AMI |
| Instance type | t3.medium |
| VPC/Subnet | balan-coffee-vpc / balan-public-a |
| Security group | balan-coffee-ec2-sg |
| IAM instance profile | BalanCoffeeEC2Role |
| Storage | gp3, at least 20 GiB |

The verified running instance is i-03642ee2788132cb3 in ap-southeast-1a.

## 3. Associate the Elastic IP

Allocate an Elastic IP, choose **Actions → Associate Elastic IP address**, and select the application instance. The verified address is 54.251.119.230.

## 4. Validate

Prefer Systems Manager Session Manager. If SSH is used, restrict it to the administrator IP.

~~~bash
aws sts get-caller-identity
curl -I http://54.251.119.230/
~~~

STS must show an EC2 assumed role rather than IAM user access-key credentials.

![Running EC2 evidence](/images/5-Workshop/evidence-ec2-instances.png)
