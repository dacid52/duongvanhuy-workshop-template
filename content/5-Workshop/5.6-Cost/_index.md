---
title: "Overview & Cost"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Lab cost overview

After deployment and (optional) sandbox cleanup, this section summarizes **actual cost** in AWS Billing. Month-to-date cost is about **$7.82** (Budgets status OK; no cost anomalies detected).

![Billing home — cost overview](/images/5-Workshop/5.6-Cost/cost-overview.png)

#### Charges by service

**Bills → Charges by service** lists services used by Movie Ticket Booking Platform. Most lines show **USD 0.00** (Free Tier / credits).

![Charges by service (1)](/images/5-Workshop/5.6-Cost/cost-by-service-1.png)

![Charges by service (2)](/images/5-Workshop/5.6-Cost/cost-by-service-2.png)

Services on the bill match the workshop stack:

| Group | Services on Bills |
| --- | --- |
| Frontend / Edge | Amplify, WAF, Data Transfer |
| API & Compute | API Gateway, Lambda, Step Functions |
| Data & Queue | DynamoDB, SQS, S3 |
| Auth & Ops | Cognito, CloudWatch, CloudFormation, SNS, KMS, Secrets Manager |

#### Free Tier / lab notes

| Item | Notes |
| --- | --- |
| **Free plan / credits** | No direct charge during the free period; real billing starts when the period or credits end |
| **Amplify sandbox** | Creates Lambda, DynamoDB, API, etc. — visible on Bills; [clean up](../5.5-Cleanup/) when unused |
| **WAF** | Web ACL on CloudFront (usually `us-east-1`) |
| **DynamoDB / Lambda** | On-demand — often within Free Tier at lab traffic |

#### Workshop conclusion

After completing all sections, you have deployed **Movie Ticket Booking Platform** with a serverless AWS architecture: Edge (WAF + CloudFront), API/Lambda, DynamoDB, SQS, Cognito, VNPay, and RUM monitoring — ready to present and defend in mentorship reviews or technical interviews.
