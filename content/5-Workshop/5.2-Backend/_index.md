---
title: "Backend Deployment"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Overview

The backend is defined under `amplify/` (Amplify Gen 2). The sandbox command provisions:

- **Amazon Cognito** — User Pool + Identity Pool
- **HTTP API (API Gateway)** — REST endpoints
- **Lambda** — `booking-api`, `booking-worker`
- **DynamoDB** — booking data tables
- **SQS + DLQ** — order processing queue
- **S3** — movie poster bucket
- **CloudWatch RUM** — frontend monitoring

#### Deploy sandbox

```powershell
cd Movie-Ticket-Booking-Platform
npm run sandbox
```

First run may take 5–15 minutes. On success, the terminal prints `apiUrl` and writes `amplify_outputs.json`.

The terminal shows a successful deploy when the CloudFormation stack reaches `UPDATE_COMPLETE` and prints the API URL.

![Sandbox deploy success](/images/5-Workshop/5.2-Backend/sandbox-deploy.png)

#### Update `.env`

Paste `apiUrl` from output into `.env`:

```
VITE_API_URL=https://xxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
```

The `amplify_outputs.json` file contains `apiUrl`, Cognito pool ID, and other outputs — copy `apiUrl` into `.env`.

![amplify_outputs.json](/images/5-Workshop/5.2-Backend/amplify-outputs.png)

#### Verify in AWS Console

| Service | Check |
| --- | --- |
| **Cognito** | User pool, app client, `admin` group |
| **API Gateway** | Routes: `/health`, `/movies`, `/bookings`, `/payments/vnpay/*` |
| **DynamoDB** | Tables created by sandbox |
| **SQS** | Booking queue + Dead Letter Queue |
| **Lambda** | 2 functions: API + worker |

**Cognito** — the User Pool is created automatically with an app client and an `admin` group for RBAC.

![Cognito User Pool](/images/5-Workshop/5.2-Backend/cognito-pool.png)

**API Gateway** — REST routes `/health`, `/movies`, `/bookings`, `/payments/vnpay/*` are registered.

![API Gateway routes](/images/5-Workshop/5.2-Backend/api-gateway.png)

**DynamoDB** — sandbox creates tables for movies, showtimes, seats, and bookings.

![DynamoDB tables](/images/5-Workshop/5.2-Backend/dynamodb-tables.png)

**SQS** — booking queue with a Dead Letter Queue for failed messages.

![SQS queue](/images/5-Workshop/5.2-Backend/sqs-queue.png)

**Lambda** — two functions: `booking-api` (sync) and `booking-worker` (queue processor).

![Lambda functions](/images/5-Workshop/5.2-Backend/lambda-functions.png)

#### Create Admin account

1. Register a user via the app or Cognito Console.
2. Add user to **admin** group:

```powershell
aws cognito-idp admin-add-user-to-group `
  --user-pool-id <USER_POOL_ID> `
  --username <email> `
  --group-name admin `
  --region ap-southeast-1
```

#### Test API

```powershell
curl https://<API_URL>/health
```

Expected: `{"ok":true,"mode":"aws","vnpayEnabled":...}`

Calling `GET /health` returns JSON confirming the AWS backend is running and whether VNPay is enabled.

![Health endpoint](/images/5-Workshop/5.4-Demo/health-api.png)

**Next step**

→ [5.3 Frontend & Hosting](../5.3-Frontend/)
