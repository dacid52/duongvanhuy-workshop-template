---
title: "Tổng quan & chi phí"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Tổng quan chi phí lab

Sau khi triển khai và (tuỳ chọn) dọn sandbox, phần này tổng hợp **chi phí thực tế** trên AWS Billing. Tài khoản lab dùng **free plan / credits** — banner ghi *Your free plan account does not get charged*. Month-to-date rất thấp (ví dụ **$0.30**).

![Billing home — tổng quan chi phí](/images/5-Workshop/5.6-Cost/cost-overview.png)

#### Charges by service

Trang **Bills → Charges by service** liệt kê các dịch vụ liên quan Movie Ticket Booking Platform. Phần lớn dòng hiện **USD 0.00** (Free Tier / credits).

![Charges by service (1)](/images/5-Workshop/5.6-Cost/cost-by-service-1.png)

![Charges by service (2)](/images/5-Workshop/5.6-Cost/cost-by-service-2.png)

Các dịch vụ trên bill khớp stack workshop:

| Nhóm | Dịch vụ trên Bills |
| --- | --- |
| Frontend / Edge | Amplify, WAF, Data Transfer |
| API & Compute | API Gateway, Lambda, Step Functions |
| Data & Queue | DynamoDB, SQS, S3 |
| Auth & Ops | Cognito, CloudWatch, CloudFormation, SNS, KMS, Secrets Manager |

#### Ghi chú Free Tier / lab

| Hạng mục | Ghi chú |
| --- | --- |
| **Free plan / credits** | Không bị charge trực tiếp trong kỳ free; hết kỳ hoặc hết credit thì tính phí thật |
| **Amplify sandbox** | Tạo Lambda, DynamoDB, API… — theo dõi trên Bills; nên [dọn dẹp](../5.5-Cleanup/) khi không dùng |
| **WAF** | Web ACL gắn CloudFront (thường region `us-east-1`) |
| **DynamoDB / Lambda** | On-demand — dễ nằm trong Free Tier khi traffic lab thấp |

#### Kết luận workshop

Sau khi hoàn thành các mục, bạn đã triển khai **Movie Ticket Booking Platform** với kiến trúc serverless trên AWS: Edge (WAF + CloudFront), API/Lambda, DynamoDB, SQS, Cognito, VNPay và giám sát RUM — đủ để trình bày và bảo vệ trước mentor hoặc phỏng vấn kỹ thuật.
