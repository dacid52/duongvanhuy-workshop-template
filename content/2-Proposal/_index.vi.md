---
title: "Bản đề xuất"
date: 2026-05-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Movie Ticket Booking Platform
## Giải pháp AWS Serverless cho hệ thống đặt vé thời gian thực

### 1. Tóm tắt điều hành
Nền tảng **Movie Ticket Booking Platform** giải quyết bài toán đặt vé quy mô lớn — hỗ trợ nhiều request đồng thời khi mở bán suất chiếu. Hệ thống dùng kiến trúc **Event-Driven Serverless** trên AWS (Amplify Gen 2, API Gateway, Lambda, DynamoDB, SQS), kết hợp Cognito, Amplify Hosting, WAF và thanh toán VNPay sandbox để đảm bảo sẵn sàng cao, mở rộng linh hoạt và trải nghiệm đặt vé mượt.

### 2. Tuyên bố vấn đề
* **Vấn đề hiện tại:** Hệ thống đặt vé truyền thống dễ nghẽn khi traffic tăng đột biến, gây trùng ghế (race condition), phản hồi chậm hoặc sập trong giờ cao điểm.
* **Giải pháp:** API Gateway + Lambda xử lý logic đồng bộ (khóa ghế, tạo booking); DynamoDB lưu trạng thái ghế có TTL; SQS + worker Lambda xử lý đơn bất đồng bộ; Cognito xác thực; VNPay sandbox thanh toán.
* **Lợi ích (ROI):** Pay-as-you-go, tự co giãn theo tải, giảm chi phí máy chủ cố định; tăng tỷ lệ đặt vé thành công.

### 3. Kiến trúc giải pháp
Quy trình: duyệt phim → chọn suất → khóa ghế → booking → (tuỳ chọn) VNPay → e-ticket.

![Kiến trúc hệ thống đặt vé xem phim trên AWS](/images/2-Proposal/architecture.png)

**Dịch vụ AWS sử dụng:**
* **Amazon Cognito:** Đăng ký / đăng nhập; nhóm admin cho trang quản trị.
* **Amazon API Gateway (HTTP API):** Cổng REST (`/health`, `/{proxy+}` → Lambda).
* **AWS Lambda:** `booking-api` (đồng bộ) và `booking-worker` (xử lý queue).
* **Amazon DynamoDB:** Lưu phim, suất chiếu, ghế, booking, vé; khóa ghế bằng TTL.
* **Amazon SQS + DLQ:** Hàng đợi đơn hàng; message lỗi vào Dead Letter Queue.
* **Amazon S3:** Poster phim / asset tĩnh backend.
* **AWS Amplify Hosting + CloudFront:** Deploy frontend React; CDN phân phối.
* **AWS WAF:** Bảo vệ edge (CloudFront) — managed rules.
* **CloudWatch RUM:** Giám sát hiệu năng & lỗi phía trình duyệt.
* **VNPay sandbox:** Cổng thanh toán thử (tích hợp qua Lambda).

**Thiết kế thành phần**
* **Frontend:** React (Vite) — trang khách + Admin Panel.
* **Auth:** Cognito JWT; phân quyền admin qua Cognito Groups.
* **API & Compute:** HTTP API → Lambda booking-api; worker consume SQS.
* **Data:** DynamoDB (single-table BookingStore + Amplify Data models).
* **Edge:** Amplify Hosting / CloudFront + WAF.
* **Observability:** CloudWatch Logs, Metrics, RUM (Admin Traffic).

### 4. Triển khai kỹ thuật
* **Các giai đoạn:**
    1. Thiết kế kiến trúc Amplify Gen 2 và mô hình dữ liệu (phim, suất, ghế, booking).
    2. Phát triển API (auth, movies, lock seats, booking, VNPay).
    3. Tích hợp DynamoDB TTL + SQS/DLQ + worker Lambda.
    4. Deploy frontend Amplify Hosting, bật WAF; kiểm thử E2E trên URL live.
* **Yêu cầu:**
    * Hạ tầng định nghĩa bằng Amplify Gen 2 / CDK (`amplify/backend.ts`).
    * DLQ cho SQS để cô lập đơn lỗi.
    * Không commit `.env` / `amplify_outputs.json` lên Git public.

### 5. Lộ trình triển khai
* **Tháng 1:** Phân tích yêu cầu, thiết kế kiến trúc, Cognito + khung React.
* **Tháng 2:** Backend Lambda / DynamoDB / SQS; lock ghế và booking.
* **Tháng 3:** VNPay, Admin, RUM; deploy Amplify Hosting + WAF.
* **Tháng 4:** UAT, báo cáo workshop Hugo, demo Mentor.

### 6. Ước tính ngân sách

Chi phí ước tính cho giai đoạn lab / sandbox và demo (traffic thấp; Free Tier áp dụng khi đủ điều kiện):

| Dịch vụ AWS | Mục đích sử dụng | Ước tính (USD/tháng) |
| :--- | :--- | :--- |
| **Amazon Cognito** | Đăng ký / đăng nhập | ~0 (Free Tier) |
| **API Gateway** | HTTP API | ~1.00 |
| **AWS Lambda** | booking-api + booking-worker | ~1.00 |
| **Amazon DynamoDB** | Dữ liệu booking (on-demand) | ~2.00 – 5.00 |
| **Amazon SQS** | Queue + DLQ | ~0.50 |
| **Amazon S3** | Poster / asset | ~1.00 |
| **Amplify Hosting / CloudFront** | Frontend + CDN | ~1.00 – 5.00 |
| **AWS WAF** | Web ACL (CloudFront) | theo rule / request |
| **CloudWatch RUM** | Giám sát frontend | ~1.00 – 3.00 |
| **TỔNG CỘNG (ước lượng)** | | **~10 – 25 USD/tháng** (lab thấp hơn nếu Free Tier) |

**Ghi chú:**
* Sandbox Amplify dùng pay-as-you-go; nên dọn dẹp khi không còn cần demo.
* Không dùng RDS / ElastiCache — giảm chi phí cố định so với kiến trúc truyền thống.

### 7. Đánh giá rủi ro
* **Quá tải API:** Giảm bằng SQS điều tiết đơn hàng.
* **Trùng ghế:** Khóa ghế trên DynamoDB kèm TTL; worker xác nhận booking.
* **Lỗi thanh toán / worker:** DLQ cô lập message lỗi; kiểm tra `/health` và CloudWatch Logs.
* **Tấn công web:** WAF trên CloudFront + HTTPS + Cognito.

### 8. Kết quả kỳ vọng
* **Hiệu năng:** Đặt vé end-to-end ổn định trên sandbox và URL live.
* **Độ tin cậy:** Ghế không bị double-book trong luồng lock → booking → worker.
* **Khả năng mở rộng:** Serverless tự co giãn theo tải; chi phí theo mức dùng.
