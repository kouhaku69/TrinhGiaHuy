---
title: "Cấu hình IAM, EC2 và Elastic IP"
date: "2026-08-07"
weight: 2
chapter: false
pre: "<b> 5.4.2. </b>"
---

# Tạo IAM role, EC2 instance và Elastic IP

## 1. Tạo IAM role cho EC2

1. Mở **IAM → Roles → Create role**.
2. Chọn **AWS service → EC2**.
3. Đặt tên BalanCoffeeEC2Role.
4. Gắn CloudWatchAgentServerPolicy.
5. Tạo inline policy BalanCoffeeAppPolicy theo phạm vi tối thiểu dưới đây và thay toàn bộ placeholder:

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

Sau khi kiểm thử thành công, thu hẹp Resource của Cognito và Bedrock về đúng User Pool/model hoặc inference profile. EC2 instance profile cung cấp temporary credentials cho AWS SDK; không đặt access key tĩnh trong production.

## 2. Tạo EC2

Vào **EC2 → Instances → Launch instances**:

| Cấu hình | Giá trị |
|---|---|
| Name | balan-coffee-app |
| AMI | Amazon Linux 2023 hoặc Ubuntu LTS; ghi đúng AMI thực tế |
| Instance type | t3.medium |
| VPC/Subnet | balan-coffee-vpc / balan-public-a |
| Security Group | balan-coffee-ec2-sg |
| IAM instance profile | BalanCoffeeEC2Role |
| Storage | gp3, tối thiểu 20 GiB |

Instance đang vận hành đã được xác nhận là i-03642ee2788132cb3, t3.medium, ap-southeast-1a.

## 3. Gắn Elastic IP

1. Vào **EC2 → Elastic IP addresses → Allocate Elastic IP address**.
2. Chọn địa chỉ vừa tạo → **Actions → Associate Elastic IP address**.
3. Resource type: **Instance**; chọn EC2 của ứng dụng.
4. Xác nhận Elastic IP đang vận hành: 54.251.119.230.

## 4. Kiểm tra

Ưu tiên **Systems Manager Session Manager**. Nếu dùng SSH, chỉ cho phép IP quản trị.

~~~bash
aws sts get-caller-identity
curl -I http://54.251.119.230/
~~~

Kết quả AWS STS phải thể hiện assumed role của EC2, không phải IAM user access key.

![Minh chứng EC2 đang chạy](/images/5-Workshop/evidence-ec2-instances.png)
