---
title: "Lambda"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---
# Initializing Lambda

### Create Lambda Function

Select **Create function** → **Author from scratch**.  
![lambda](/images/5-Workshop/5.3-Lambda/lambda1.png)

In the **Basic information** section, configure:
- Function name: **taskhub-backend-1**
- Runtime: **.NET 8** (C#/F#/Powershell)
- Architecture: **arm64** (default)

In **Permissions** → Change default execution role:
- Select **Create a new role with basic Lambda permissions**  
  (AWS will automatically create a role with CloudWatch Logs write access)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda2.png)

Keep the remaining settings as default and click **Create function** to finish.

![lambda2](/images/5-Workshop/5.3-Lambda/lambda3.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda4.png)

---

### Build Backend & Package into ZIP

Because Lambda does not allow direct code editing for the .NET runtime, you must upload a ZIP file.

- On your local machine, build the backend using:

```bash
dotnet publish -c Release -o publish
```

- Open the **publish** folder  

→ Select all files inside it, do **not** select the folder itself.

- Compress them into **backend.zip**

### Upload Source Code to Lambda

- In the **Code** tab → select **Upload from** → **.zip file**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda5.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda7.png)

- Select the file **backend.zip**

- Click **Upload** and wait for Lambda to deploy

![lambda2](/images/5-Workshop/5.3-Lambda/lambda8.png)

After the upload completes, the **Code properties** section will display the package size, SHA256 hash, and the last updated timestamp.

![lambda2](/images/5-Workshop/5.3-Lambda/lambda10.png)

---

### Verify Runtime Settings

In the **Runtime settings** section:

- Runtime: **.NET 8**
- Handler: **YourProject::YourNamespace.YourHandler::FunctionHandler**  
  (depends on your project structure)
- Architecture: **arm64**

Ensure that Lambda is running the correct entry point.

---

### Configure Environment Variables

- Open the **Configuration** tab → **Environment variables**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda11.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda12.png)

- Click **Edit** → **Add environment variable**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda13.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda14.png)

Add example variables:

- **ASPNETCORE_ENVIRONMENT** = **Production**
- **DynamoDB_TableName** = **Your_dynamoDB**
- **Jwt_Secret** = **Your_JWT_Secret**

Click **Save**.

---

### Configure Timeout & Memory

Open **Configuration** → **General configuration**

Click **Edit**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda15.png)

Set:

- Memory: **512MB – 1024MB**
- Timeout: **30s** (suitable for APIs) or higher if needed

![lambda2](/images/5-Workshop/5.3-Lambda/lambda16.png)

Click **Save**.

---

### Add Trigger (API Gateway)

To expose Lambda as an API with a public URL:

- Go to **Configuration** → **Triggers**
- Click **Add trigger**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda17.png)

Choose:

- **API Gateway**
- **Create an API**
- **REST API**

Security: Open (or IAM/Authorizer depending on your system)

Click **Add**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda18.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda19.png)