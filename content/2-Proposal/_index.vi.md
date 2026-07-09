---
title: "Bản đề xuất"
date: 2026-05-11
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# Hệ thống Đặt vé Xem phim Trực tuyến
## Giải pháp AWS Serverless cho hệ thống đặt vé thời gian thực

### 1. Tóm tắt điều hành
Nền tảng Đặt vé xem phim được thiết kế để giải quyết bài toán đặt vé quy mô lớn, hỗ trợ xử lý hàng ngàn request đồng thời trong các đợt mở bán phim bom tấn. Hệ thống sử dụng kiến trúc Event-Driven, tận dụng các dịch vụ Serverless của AWS để đảm bảo tính sẵn sàng cao, khả năng mở rộng linh hoạt và trải nghiệm thanh toán mượt mà cho người dùng. 

### 2. Tuyên bố vấn đề
* **Vấn đề hiện tại:** Các hệ thống đặt vé truyền thống thường gặp tình trạng nghẽn cổ chai khi lượng truy cập tăng đột biến, gây ra lỗi trùng ghế (race condition), thời gian phản hồi chậm hoặc sập hệ thống trong giờ cao điểm.
* **Giải pháp:** Sử dụng kiến trúc Serverless với Amazon API Gateway và AWS Lambda để xử lý logic nghiệp vụ. Dữ liệu được bảo vệ và đồng bộ hóa tức thì nhờ Amazon ElastiCache (Redis) để giữ chỗ tạm thời (Seat Locking) và Amazon SQS để quản lý hàng đợi đơn hàng bất đồng bộ.
* **Lợi ích (ROI):** Giảm thiểu chi phí hạ tầng nhờ mô hình "Pay-as-you-go". Hệ thống tự động mở rộng theo nhu cầu thực tế. Độ tin cậy cao giúp tăng tỷ lệ chuyển đổi vé thành công và nâng cao sự hài lòng của khách hàng.
 
### 3. Kiến trúc giải pháp
Nền tảng áp dụng mô hình Serverless hoàn toàn để quản lý quy trình từ chọn ghế, thanh toán đến xuất vé.

![Movie Ticket Booking Platform](/images/2-Proposal/edge_architecture.jpeg)

![Movie Ticket Booking Platform](/images/2-Proposal/platform_architecture.jpeg)

**Dịch vụ AWS sử dụng:**
* **API Gateway:** Tiếp nhận yêu cầu đặt vé từ web/mobile.
* **AWS Lambda:** Xử lý logic đặt ghế, thanh toán và gửi xác nhận.
* **Amazon ElastiCache (Redis):** Xử lý "khóa ghế" thời gian thực, ngăn chặn trùng ghế.
* **Amazon SQS:** Hàng đợi tin nhắn xử lý đơn đặt vé, đảm bảo hệ thống không bị quá tải.
* **Amazon RDS (Multi-AZ):** Lưu trữ dữ liệu người dùng, hóa đơn và lịch chiếu.
* **Amazon Cognito:** Quản lý đăng nhập và bảo mật người dùng.
* **Amazon SNS:** Thông báo xác nhận vé thành công qua Email/SMS.  

*Thiết kế thành phần*  
- *Thiết bị biên*: Raspberry Pi thu thập và lọc dữ liệu cảm biến, gửi tới IoT Core.  
- *Tiếp nhận dữ liệu*: AWS IoT Core nhận tin nhắn MQTT từ thiết bị biên.  
- *Lưu trữ dữ liệu*: Dữ liệu thô lưu trong S3 data lake; dữ liệu đã xử lý lưu ở một S3 bucket khác.  
- *Xử lý dữ liệu*: AWS Glue Crawlers lập chỉ mục dữ liệu; ETL jobs chuyển đổi để phân tích.  
- *Giao diện web*: AWS Amplify lưu trữ ứng dụng Next.js cho bảng điều khiển và phân tích thời gian thực.  
- *Quản lý người dùng*: Amazon Cognito giới hạn 5 tài khoản hoạt động.  

### 4. Triển khai kỹ thuật
* **Các giai đoạn:**
    1. Thiết kế mô hình Database (Sơ đồ ghế, lịch chiếu) và kiến trúc Serverless.
    2. Phát triển các API nghiệp vụ (Authentication, Booking, Payment, Ticket).
    3. Tích hợp Redis để xử lý logic khóa ghế và SQS để xử lý đơn hàng.
    4. Kiểm thử tải (Load Testing) và triển khai trên môi trường AWS.
* **Yêu cầu:** * Sử dụng AWS CDK để tự động hóa triển khai hạ tầng.
    * Thiết lập Dead Letter Queue (DLQ) cho SQS để xử lý các đơn hàng lỗi.

### 5. Lộ trình triển khai
* **Tháng 1:** Phân tích yêu cầu, thiết kế kiến trúc và sơ đồ dữ liệu.
* **Tháng 2:** Lập trình Backend (Lambda) và tích hợp Redis/SQS.
* **Tháng 3:** Phát triển Frontend (Amplify/Next.js) và tích hợp hệ thống thanh toán.
* **Tháng 4:** Kiểm thử (UAT), tối ưu hiệu năng và vận hành. 

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  


### 6. Ước tính ngân sách

Chi phí dưới đây được ước tính cho giai đoạn khởi tạo và vận hành nền tảng với quy mô giả định từ 10.000 đến 50.000 người dùng hoạt động mỗi tháng (MAU).

| Dịch vụ AWS | Mục đích sử dụng | Ước tính (USD/tháng) |
| :--- | :--- | :--- |
| **Amazon Cognito** | Quản lý người dùng (Đăng ký/Đăng nhập) | 0.00 USD (Free Tier) |
| **API Gateway** | Điều phối request từ Frontend | ~1.50 USD |
| **AWS Lambda** | Xử lý logic đặt vé, thanh toán, xuất vé | ~1.00 USD |
| **S3 & CloudFront** | Lưu trữ giao diện & CDN phân phối | ~5.00 USD |
| **Amazon RDS (Multi-AZ)** | CSDL quan hệ (User, Đơn hàng, Lịch chiếu) | ~35.00 - 45.00 USD |
| **ElastiCache (Redis)** | Khóa ghế thời gian thực (Seat Locking) | ~15.00 - 20.00 USD |
| **SQS & SNS** | Hàng đợi & Thông báo xác nhận vé | ~0.50 USD |
| **CloudWatch & X-Ray** | Giám sát & Truy vết hệ thống | ~2.00 USD |
| **TỔNG CỘNG** | | **~55.00 - 75.00 USD/tháng** |

**Ghi chú tối ưu chi phí:**
* **Chính sách Free Tier:** Trong 12 tháng đầu tiên, nhóm được hưởng gói **AWS Free Tier**, giúp giảm đáng kể chi phí cho các dịch vụ cốt lõi như Lambda, S3, và RDS (tùy thuộc vào hạn mức sử dụng), đưa chi phí thực tế về gần mức **0 USD**.
* **Giải pháp tối ưu dài hạn:** * Sử dụng **Aurora Serverless v2** cho cơ sở dữ liệu để hệ thống tự động co giãn theo lưu lượng truy cập, thay vì duy trì instance cố định.
    * Áp dụng các gói **Reserved Instances** sau khi hệ thống vận hành ổn định để tiết kiệm 30-40% chi phí duy trì.
* **Mô hình tài chính:** Đây là kiến trúc **Pay-as-you-go** (trả theo mức độ sử dụng). Nếu lưu lượng truy cập thấp, chi phí sẽ tự động giảm xuống, tối ưu hóa lợi nhuận cho giai đoạn khởi nghiệp. 

### 7. Đánh giá rủi ro
* **Quá tải hệ thống:** Giảm thiểu bằng SQS để điều tiết traffic.
* **Trùng ghế:** Giải quyết bằng cơ chế Locking của Redis.
* **Lỗi thanh toán:** Sử dụng DLQ để cô lập đơn hàng và cảnh báo qua SNS cho Admin. 

### 8. Kết quả kỳ vọng
* **Hiệu năng:** Xử lý hàng ngàn giao dịch đặt vé mỗi phút.
* **Độ tin cậy:** Đảm bảo 100% dữ liệu ghế chính xác, không đặt trùng.
* **Khả năng mở rộng:** Tự động mở rộng khi người dùng tăng đột biến.