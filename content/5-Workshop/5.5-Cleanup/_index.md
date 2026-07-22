---
title: "Resource Cleanup"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Cleaning up AWS resources after the workshop helps avoid unexpected charges when Free Tier ends. Run cleanup only when you no longer need sandbox data, because deletion removes movies, users, and bookings.

#### Checklist

| Resource | Decision |
| --- | --- |
| Amplify sandbox (Lambda, API, DynamoDB, SQS, Cognito, etc.) | **Deleted** — `npx ampx sandbox delete` |
| CDKToolkit (CloudFormation) | **Kept** — CDK bootstrap |

### Step 1: Delete Amplify sandbox (backend)

In the project folder:

```powershell
# ONLY run when you are sure you want to delete the sandbox backend
npx ampx sandbox delete
```

Confirm when the CLI prompts for deletion. When finished, the CLI reports `Deployment completed` and `Finished deleting` (often ~3–4 minutes).

![Sandbox delete](/images/5-Workshop/5.5-Cleanup/sandbox-delete.png)

After deletion, check **CloudFormation → Stacks** (`ap-southeast-1`). Amplify sandbox stacks are gone; usually only **`CDKToolkit`** remains (CDK bootstrap — **do not delete**).

![Sandbox removed](/images/5-Workshop/5.5-Cleanup/sandbox-gone.png)

### Step 2: Verify DynamoDB

Confirm sandbox DynamoDB tables are gone — **Tables (0)** in `ap-southeast-1`.

![DynamoDB after cleanup](/images/5-Workshop/5.5-Cleanup/resources-gone.png)

**Next step**

→ [5.6 Overview & Cost](../5.6-Cost/)
