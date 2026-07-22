---
title: "Proposal"
date: 2026-05-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Movie Ticket Booking Platform
## AWS Serverless Solution for Real-time Booking Systems

### 1. Executive Summary
The **Movie Ticket Booking Platform** handles concurrent ticket sales during peak showtimes. It uses an **event-driven serverless** stack on AWS (Amplify Gen 2, API Gateway, Lambda, DynamoDB, SQS), with Cognito, Amplify Hosting, WAF, and VNPay sandbox for availability, scale, and a smooth booking experience.

### 2. Problem Statement
* **Current Challenges:** Traditional booking systems bottleneck under traffic spikes — race conditions (double-booking), high latency, or outages at peak hours.
* **Proposed Solution:** API Gateway + Lambda for sync logic (seat lock, create booking); DynamoDB with TTL for seat state; SQS + worker Lambda for async orders; Cognito for identity; VNPay sandbox for payment.
* **Benefits (ROI):** Pay-as-you-go, auto-scale with demand, lower fixed server cost; higher successful booking rate.

### 3. Solution Architecture
Flow: browse movies → select showtime → lock seats → booking → (optional) VNPay → e-ticket.

![Movie Ticket Booking Platform Architecture](/images/2-Proposal/architecture.png)

**AWS Services Used:**
* **Amazon Cognito:** Sign-up / sign-in; `admin` group for the admin panel.
* **Amazon API Gateway (HTTP API):** REST entry (`/health`, `/{proxy+}` → Lambda).
* **AWS Lambda:** `booking-api` (sync) and `booking-worker` (queue consumer).
* **Amazon DynamoDB:** Movies, showtimes, seats, bookings, tickets; seat locks via TTL.
* **Amazon SQS + DLQ:** Order queue; failed messages go to the Dead Letter Queue.
* **Amazon S3:** Movie posters / backend static assets.
* **AWS Amplify Hosting + CloudFront:** React frontend deploy and CDN.
* **AWS WAF:** Edge protection on CloudFront (managed rules).
* **CloudWatch RUM:** Browser performance and error monitoring.
* **VNPay sandbox:** Trial payment gateway (via Lambda).

**Component Design:**
* **Frontend:** React (Vite) — customer UI + Admin Panel.
* **Auth:** Cognito JWT; admin RBAC via Cognito Groups.
* **API & Compute:** HTTP API → booking-api Lambda; worker consumes SQS.
* **Data:** DynamoDB (BookingStore single-table + Amplify Data models).
* **Edge:** Amplify Hosting / CloudFront + WAF.
* **Observability:** CloudWatch Logs, Metrics, RUM (Admin Traffic).

### 4. Technical Implementation
* **Phases:**
    1. Amplify Gen 2 architecture and data model (movies, showtimes, seats, bookings).
    2. Business APIs (auth, movies, lock seats, booking, VNPay).
    3. DynamoDB TTL + SQS/DLQ + worker Lambda.
    4. Amplify Hosting deploy, enable WAF; E2E tests on the live URL.
* **Requirements:**
    * Infrastructure as code via Amplify Gen 2 / CDK (`amplify/backend.ts`).
    * SQS DLQ for failed orders.
    * Do not commit `.env` / `amplify_outputs.json` to a public Git repo.

### 5. Implementation Roadmap
* **Month 1:** Requirements, architecture, Cognito + React shell.
* **Month 2:** Lambda / DynamoDB / SQS; seat lock and booking.
* **Month 3:** VNPay, Admin, RUM; Amplify Hosting + WAF.
* **Month 4:** UAT, Hugo workshop report, Mentor demo.

### 6. Budget Estimation

Estimated monthly cost for lab / sandbox and low-traffic demo (Free Tier may apply):

| AWS Service | Purpose | Estimated (USD/Month) |
| :--- | :--- | :--- |
| **Amazon Cognito** | Auth | ~0 (Free Tier) |
| **API Gateway** | HTTP API | ~1.00 |
| **AWS Lambda** | booking-api + booking-worker | ~1.00 |
| **Amazon DynamoDB** | Booking data (on-demand) | ~2.00 – 5.00 |
| **Amazon SQS** | Queue + DLQ | ~0.50 |
| **Amazon S3** | Posters / assets | ~1.00 |
| **Amplify Hosting / CloudFront** | Frontend + CDN | ~1.00 – 5.00 |
| **AWS WAF** | Web ACL (CloudFront) | per rules / requests |
| **CloudWatch RUM** | Frontend monitoring | ~1.00 – 3.00 |
| **TOTAL (estimate)** | | **~10 – 25 USD/Month** (lower in lab with Free Tier) |

**Notes:**
* Amplify sandbox is pay-as-you-go — clean up when demos are done.
* No RDS / ElastiCache — avoids fixed instance cost vs classic stacks.

### 7. Risk Assessment
* **API overload:** Mitigated by SQS order buffering.
* **Double-booking:** DynamoDB seat lock with TTL; worker confirms booking.
* **Payment / worker failures:** DLQ isolation; `/health` and CloudWatch Logs.
* **Web attacks:** WAF on CloudFront + HTTPS + Cognito.

### 8. Expected Outcomes
* **Performance:** Stable end-to-end booking on sandbox and live URL.
* **Reliability:** No double-book in lock → booking → worker flow.
* **Scalability:** Serverless scales with load; cost follows usage.
