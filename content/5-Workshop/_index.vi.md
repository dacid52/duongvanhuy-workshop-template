---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng Movie Ticket Booking Platform trên AWS

#### Tổng quan

Workshop này mô tả quy trình thiết kế, triển khai và vận hành **Movie Ticket Booking Platform** — nền tảng đặt vé xem phim serverless trên AWS, sử dụng **AWS Amplify Gen 2**, **Amazon Cognito**, **API Gateway**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon SQS** và **AWS Amplify Hosting** (phía sau là **Amazon CloudFront**).

Hệ thống được bảo vệ bởi nhiều lớp: **AWS WAF** gắn **CloudFront**, xác thực **Cognito**, HTTPS end-to-end và giám sát **CloudWatch RUM**.

**Liên kết tham khảo:**

- **Ứng dụng live:** https://main.d2zv6ka00i1nyo.amplifyapp.com
- **Mã nguồn (public):** https://github.com/DHgLang/Movie-Ticket-Booking-Platform

#### Mục tiêu workshop

- Triển khai end-to-end một nền tảng đặt vé phim có khả năng **mở rộng theo tải** (serverless).
- Áp dụng mô hình **đồng bộ + bất đồng bộ**: khóa ghế nhanh qua API, xử lý đơn hàng qua hàng đợi SQS.
- Tích hợp **bảo mật đa lớp**: WAF trên CloudFront, Cognito, HTTPS, giám sát CloudWatch.
- Kết nối **thanh toán VNPay sandbox** và **bảng quản trị Admin**.

#### Các dịch vụ AWS sử dụng

| Dịch vụ | Mục đích |
| --- | --- |
| **AWS Amplify Gen 2 / Hosting** | Định nghĩa backend (CDK) và deploy frontend React |
| **Amazon CloudFront** + **AWS WAF** | CDN + lọc bot, SQLi, XSS, rate limit |
| **Amazon Cognito** | Đăng ký / đăng nhập; nhóm `admin` |
| **Amazon API Gateway (HTTP API)** | Cổng REST: phim, suất chiếu, đặt vé, thanh toán |
| **AWS Lambda** | `booking-api` (đồng bộ) + `booking-worker` (SQS) |
| **Amazon DynamoDB** | Phim, suất chiếu, ghế, booking, vé |
| **Amazon SQS + DLQ** | Hàng đợi đơn hàng khi traffic tăng đột biến |
| **Amazon S3** | Poster phim, asset tĩnh |
| **CloudWatch RUM** | Giám sát hiệu năng & lỗi phía trình duyệt |

#### Sơ đồ Architecture

![Sơ đồ Architecture](/images/5-Workshop/architecture.png)

#### Luồng đặt vé (tóm tắt)

1. Người dùng duyệt phim → chọn suất → chọn ghế trên seat map.
2. **POST lock seats** — Lambda ghi trạng thái ghế vào DynamoDB (TTL lock).
3. **POST booking** — tạo đơn, đẩy message vào **SQS**.
4. **Worker Lambda** xử lý queue → cập nhật booking → phát hành vé.
5. (Tuỳ chọn) **VNPay sandbox** — redirect thanh toán → confirm → hoàn tất vé.

#### Nội dung

1. [Chuẩn bị môi trường](5.1-Prerequisite/)
2. [Triển khai Backend (Amplify Sandbox)](5.2-Backend/)
3. [Frontend & Deploy Hosting](5.3-Frontend/)
4. [Demo ứng dụng](5.4-Demo/)
5. [Dọn dẹp tài nguyên](5.5-Cleanup/)
6. [Tổng quan & chi phí](5.6-Cost/)
