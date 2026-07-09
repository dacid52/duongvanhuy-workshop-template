---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# GIẢI QUYẾT BÀI TOÁN QUÁ TẢI TRONG NỀN TẢNG ĐẶT VÉ XEM PHIM BẰNG AWS SERVERLESS

Nền tảng Đặt vé xem phim trực tuyến vừa áp dụng thành công kiến trúc Event-Driven Serverless, cho phép hệ thống tự động mở rộng linh hoạt để xử lý hàng ngàn request đồng thời. Đây là bước tiến quan trọng giúp giải quyết triệt để tình trạng nghẽn cổ chai và lỗi trùng ghế (race condition) thường gặp trong các đợt mở bán vé phim bom tấn.

Các điểm chính cần nắm:

* Kiến trúc kết hợp **Amazon ElastiCache (Redis)** để thực hiện "khóa ghế" (Seat Locking) theo thời gian thực, ngăn chặn tuyệt đối việc hai khách hàng mua cùng một vị trí.
* Cơ chế **Xử lý bất đồng bộ (Asynchronous)** được triển khai qua **Amazon SQS**, đóng vai trò như một bộ đệm an toàn lưu trữ các đơn hàng đang chờ thanh toán.
* Tích hợp **AWS Lambda** làm các Worker chạy ngầm để từ từ kéo đơn hàng từ SQS xử lý và ghi nhận vào cơ sở dữ liệu **Amazon RDS**, bảo vệ DB khỏi tình trạng quá tải.
* Tiến trình gửi thông báo xác nhận vé được tách biệt hoàn toàn khỏi luồng người dùng và kích hoạt qua **Amazon SNS**, giúp giảm đáng kể thời gian chờ đợi trên giao diện web.
* Mô hình tài chính **Pay-as-you-go** tự động thu hẹp tài nguyên khi không có lịch chiếu hot, giúp tối ưu hóa chi phí hạ tầng so với việc duy trì server vật lý 24/7.
* Quản lý bảo mật truy cập tập trung qua **Amazon API Gateway** và **Cognito**, đảm bảo an toàn dữ liệu từ frontend (Next.js trên CloudFront) đến các dịch vụ backend nội bộ.

Kiến trúc này đặc biệt hữu ích khi rạp chiếu phim đối mặt với lượng truy cập tăng đột biến (spiky traffic) vào giờ vàng, đảm bảo hệ thống luôn ổn định, tỷ lệ chốt đơn thành công cao và nâng tầm trải nghiệm của khán giả.
