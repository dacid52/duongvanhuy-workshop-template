---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Building Movie Ticket Booking Platform on AWS

#### Overview

This workshop documents how to design, deploy, and operate **Movie Ticket Booking Platform** — a serverless movie ticket booking system on AWS using **AWS Amplify Gen 2**, **Amazon Cognito**, **API Gateway**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon SQS**, and **AWS Amplify Hosting** (backed by **Amazon CloudFront**).

The system is protected by multiple layers: **AWS WAF** on **CloudFront**, **Cognito** authentication, HTTPS end-to-end, and **CloudWatch RUM** monitoring.

**Reference links:**

- **Live app:** https://main.d2zv6ka00i1nyo.amplifyapp.com
- **Source code (public):** https://github.com/DHgLang/Movie-Ticket-Booking-Platform

#### Workshop goals

- Deploy an end-to-end **scalable** movie ticket booking platform (serverless).
- Apply **sync + async** patterns: fast seat locking via API, order processing via SQS queue.
- Integrate **multi-layer security**: WAF on CloudFront, Cognito, HTTPS, CloudWatch monitoring.
- Connect **VNPay sandbox** payment and an **Admin** panel.

#### AWS services used

| Service | Purpose |
| --- | --- |
| **AWS Amplify Gen 2 / Hosting** | Backend definition (CDK) and React frontend deploy |
| **Amazon CloudFront** + **AWS WAF** | CDN + bot / SQLi / XSS / rate-limit filtering |
| **Amazon Cognito** | Sign-up / sign-in; `admin` group |
| **Amazon API Gateway (HTTP API)** | REST entry: movies, showtimes, booking, payment |
| **AWS Lambda** | `booking-api` (sync) + `booking-worker` (SQS) |
| **Amazon DynamoDB** | Movies, showtimes, seats, bookings, tickets |
| **Amazon SQS + DLQ** | Order queue for traffic spikes |
| **Amazon S3** | Movie posters, static assets |
| **CloudWatch RUM** | Browser performance & error monitoring |

#### Architecture diagram

![Architecture diagram](/images/5-Workshop/architecture.png)

#### Booking flow (summary)

1. User browses movies → selects showtime → picks seats on seat map.
2. **POST lock seats** — Lambda writes seat state to DynamoDB (TTL lock).
3. **POST booking** — creates order, enqueues message to **SQS**.
4. **Worker Lambda** processes queue → updates booking → issues ticket.
5. (Optional) **VNPay sandbox** — payment redirect → confirm → ticket complete.

#### Contents

1. [Environment Setup](5.1-Prerequisite/)
2. [Backend Deployment (Amplify Sandbox)](5.2-Backend/)
3. [Frontend & Hosting Deploy](5.3-Frontend/)
4. [Application Demo](5.4-Demo/)
5. [Resource Cleanup](5.5-Cleanup/)
6. [Overview & Cost](5.6-Cost/)
