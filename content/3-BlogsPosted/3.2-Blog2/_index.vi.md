---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# TỐI ƯU HIỆU SUẤT VÀ BẢO MẬT TOÀN DIỆN CHO HỆ THỐNG ĐẶT VÉ VỚI AWS

Bên cạnh việc giải quyết bài toán chịu tải trong giờ cao điểm, nền tảng Đặt vé xem phim trực tuyến của chúng tôi còn chú trọng vào hai yếu tố sống còn khác: tốc độ phản hồi giao diện và bảo mật dữ liệu người dùng. Bằng cách áp dụng chiến lược bảo mật nhiều lớp (multi-layered security) và điện toán vùng biên (edge computing) của AWS, hệ thống mang lại trải nghiệm mượt mà và an toàn tuyệt đối.

Các điểm chính cần nắm:

* **Tối ưu tốc độ tải trang bằng CloudFront & S3:** Toàn bộ ứng dụng Frontend (Next.js) được lưu trữ tĩnh trên Amazon S3 và phân phối qua mạng lưới CDN toàn cầu của Amazon CloudFront, giúp khán giả truy cập web với độ trễ thấp nhất dù ở bất kỳ đâu.
* **Xác thực và Quản lý danh tính với Amazon Cognito:** Loại bỏ hoàn toàn việc tự xây dựng module đăng nhập phức tạp. Cognito đảm nhận việc xác thực người dùng, cung cấp JWT token an toàn và hỗ trợ khôi phục mật khẩu mà không cần quản lý database người dùng thủ công.
* **Cửa ngõ bảo mật Amazon API Gateway:** Đóng vai trò là "người gác cổng" duy nhất cho toàn bộ các Microservices nội bộ. API Gateway tích hợp sẵn tính năng xác thực token từ Cognito và kiểm soát lưu lượng (Throttling) để ngăn chặn các cuộc tấn công DDoS hoặc bot spam đặt vé.
* **Cô lập hạ tầng lõi trong Private VPC:** Các cơ sở dữ liệu quan trọng như Amazon RDS (lưu trữ giao dịch, thông tin vé) và ElastiCache Redis (khóa ghế) được đặt sâu bên trong các Private Subnet. Chúng hoàn toàn vô hình trước Internet công cộng và chỉ có thể được truy cập bởi các hàm AWS Lambda nội bộ.
* **Truy vết và Giám sát với AWS X-Ray & CloudWatch:** Mọi request đi qua hệ thống đều được theo dõi chi tiết. Nếu có điểm nghẽn (bottleneck) xảy ra tại cổng thanh toán hoặc quá trình ghi database, đội ngũ quản trị có thể ngay lập tức phát hiện và xử lý.

Sự kết hợp giữa phân phối nội dung ở vùng biên và mạng lưới VPC bảo mật nghiêm ngặt bên trong giúp nền tảng không chỉ phản hồi cực nhanh mà còn đáp ứng các tiêu chuẩn khắt khe nhất về an toàn thông tin tài chính của khách hàng.