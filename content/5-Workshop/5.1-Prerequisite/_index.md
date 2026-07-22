---
title: "Environment Setup"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Requirements

| Item | Version / notes |
| --- | --- |
| **AWS account** | FCJ lab / account allowed to deploy Amplify sandbox |
| **Node.js** | 18 or later |
| **npm** | Bundled with Node |
| **AWS CLI** | Access Key + region configured |
| **Region** | `ap-southeast-1` (Singapore) |
| **Git** | Clone public repo |

### Step 1: Sign in to AWS Management Console

1. Open [https://console.aws.amazon.com](https://console.aws.amazon.com).
2. Sign in with the lab account (root or IAM user).
3. Confirm **Account ID** and username in the top-right corner.

![AWS Console signed in](/images/5-Workshop/5.1-Prerequisite/console-login.png)

The top-right corner shows account **DHgLang** and Account ID **618647024917**.

![Account on Console](/images/5-Workshop/5.1-Prerequisite/console-account.png)

---

### Step 2: Select Singapore region

1. Open the **region** menu (top right).
2. Choose **Asia Pacific (Singapore) `ap-southeast-1`**.
3. Sandbox services (Cognito, DynamoDB, Lambda, API Gateway, etc.) are created here — except CloudFront-scoped WAF (later in `us-east-1`).

![Select region ap-southeast-1](/images/5-Workshop/5.1-Prerequisite/console-region.png)

Console region must match AWS CLI (`aws configure` → Default region `ap-southeast-1`).

![AWS region in CLI / config](/images/5-Workshop/5.1-Prerequisite/aws-region.png)

---

### Step 3: Check IAM (user / permissions)

1. Open **IAM** → **Users** (or **Roles** for lab roles).
2. Open your user → **Permissions**.
3. Confirm rights to deploy: CloudFormation, Lambda, API Gateway, DynamoDB, SQS, Cognito, S3, IAM (PassRole), CloudWatch, Amplify.

![IAM Users / Permissions](/images/5-Workshop/5.1-Prerequisite/iam-permissions.png)

Do not capture full **Access Key Secret** values. Show the key list (redacted) or the Permissions screen only.

To create an Access Key for CLI:

1. IAM → Users → your user → **Security credentials**.
2. **Create access key** → **Command Line Interface (CLI)**.
3. Save Access Key ID + Secret (shown once).

![Create Access Key — choose CLI](/images/5-Workshop/5.1-Prerequisite/iam-access-key-usecase.png)

![Retrieve access keys (Secret redacted)](/images/5-Workshop/5.1-Prerequisite/iam-access-key.png)

---

### Step 4: Install Node.js and verify

1. Download Node.js LTS from [https://nodejs.org](https://nodejs.org).
2. Install and keep PATH options enabled.
3. In PowerShell:

```powershell
node -v
npm -v
```

Expect Node ≥ 18 (e.g. `v20.x` / `v24.x`).

![Node.js version](/images/5-Workshop/5.1-Prerequisite/node-version.png)

---

### Step 5: Install and configure AWS CLI

1. Install AWS CLI v2 from the [AWS CLI install guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
2. Then run:

```powershell
aws --version
aws configure
```

Enter:

- AWS Access Key ID  
- AWS Secret Access Key  
- Default region: `ap-southeast-1`  
- Default output: `json`

3. Verify identity:

```powershell
aws sts get-caller-identity
```

A successful response includes `Account`, `Arn`, and `UserId` — ARN should look like `...user/Van_Huy` (IAM user), not root.

![AWS CLI identity — Van_Huy](/images/5-Workshop/5.1-Prerequisite/aws-cli.png)

---

### Step 6: Clone source and install dependencies

```powershell
git clone https://github.com/DHgLang/Movie-Ticket-Booking-Platform.git
cd Movie-Ticket-Booking-Platform
npm install
```

Main folders: `amplify/` (backend), `frontend/` (React), `scripts/` (deploy).

![Project structure](/images/5-Workshop/5.1-Prerequisite/project-structure.png)

---

### Step 7: Create `.env`

```powershell
copy .env.example .env
notepad .env
```

| Variable | Purpose |
| --- | --- |
| `VITE_API_URL` | API Gateway URL (after `npm run sandbox`) |
| `VITE_TMDB_API_KEY` | TMDB poster API key |
| `TMDB_API_KEY` | Same key for Lambda |
| `VNPAY_TMN_CODE`, `VNPAY_HASH_SECRET` | VNPay sandbox |
| `FRONTEND_URL`, `VNPAY_RETURN_URL` | Payment callback URLs |

Redact secrets before screenshots.

![.env file (secrets redacted)](/images/5-Workshop/5.1-Prerequisite/env-file.png)

{{% notice info %}}
**Note:** Do not commit `.env` or `amplify_outputs.json` — they are in `.gitignore`.
{{% /notice %}}

---

### Step 8: Quick Console check (before sandbox)

In Singapore region, note where resources will appear after sandbox deploy:

| Service | What to check |
| --- | --- |
| **CloudFormation** | Stacks view after sandbox |
| **DynamoDB** | Tables (may be empty now) |
| **Cognito** | User pools |
| **Lambda** | Functions |

**Next step**

When ready → [5.2 Backend Deployment](../5.2-Backend/)
