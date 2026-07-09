---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# SOLVING OVERLOAD ISSUES IN MOVIE TICKET BOOKING PLATFORMS WITH AWS SERVERLESS

The online Movie Ticket Booking Platform has successfully adopted an Event-Driven Serverless architecture, allowing the system to automatically and flexibly scale to handle thousands of concurrent requests. This is a crucial step forward in comprehensively resolving the bottlenecks and race conditions (double-booking) commonly encountered during blockbuster ticket releases.

Key takeaways:

* The architecture integrates **Amazon ElastiCache (Redis)** to implement real-time "Seat Locking", absolutely preventing two customers from purchasing the same seat.
* The **Asynchronous processing** mechanism is deployed via **Amazon SQS**, acting as a secure buffer to store pending orders awaiting payment.
* Integrates **AWS Lambda** as background Workers to gradually poll orders from SQS for processing and record them into the **Amazon RDS** database, protecting the DB from being overloaded.
* The ticket confirmation notification process is completely decoupled from the main user flow and triggered via **Amazon SNS**, significantly reducing wait times on the web interface.
* The **Pay-as-you-go** financial model automatically scales down resources when there are no high-demand screenings, optimizing infrastructure costs compared to maintaining physical servers 24/7.
* Centralized access security management via **Amazon API Gateway** and **Cognito**, ensuring data safety from the frontend (Next.js on CloudFront) to internal backend services.

This architecture is particularly useful when cinemas face spiky traffic during prime time, ensuring the system remains stable, maintaining high successful transaction rates, and elevating the audience experience.
