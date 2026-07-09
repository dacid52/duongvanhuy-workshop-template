---
title: "Worklog Tuần 2"
date: 2026-05-11
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:
* Nghiên cứu kiến trúc Amazon VPC và các thành phần mạng chính trên AWS.
* Cấu hình các tài nguyên mạng nền tảng và lớp bảo mật phục vụ kết nối EC2.
* Tìm hiểu các mô hình kết nối như VPN, VPC Peering và AWS Transit Gateway.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - Xây dựng bố cục mạng cơ bản:<br>&emsp; + Tạo các subnet theo từng phạm vi truy cập.<br>&emsp; + Cấu hình Route Table.<br>&emsp; + Kết nối Internet Gateway và xem xét vị trí triển khai NAT Gateway. | 27/04/2026 | 27/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Thiết lập các lớp bảo vệ mạng:<br>&emsp; + Security Group.<br>&emsp; + Network ACL.<br>&emsp; + Chuẩn bị Key Pair để truy cập instance an toàn. | 28/04/2026 | 28/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Khởi tạo các EC2 instance trên những subnet đã cấu hình.<br>- Kiểm tra đường kết nối giữa các instance và khả năng truy cập ra bên ngoài khi phù hợp. | 29/04/2026 | 29/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tìm hiểu các phương án kết nối nâng cao:<br>&emsp; + Site-to-Site VPN.<br>&emsp; + VPC Peering.<br>&emsp; + AWS Transit Gateway.<br>- So sánh tình huống sử dụng của từng mô hình trong thiết kế mạng AWS. | 30/04/2026 | 30/04/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 2:
* Nắm được cách tổ chức một VPC cơ bản và vai trò của các thành phần mạng chính thông qua các bước triển khai đầu tiên.
* Hoàn thành việc cấu hình lớp định tuyến và kiểm soát truy cập phục vụ kết nối EC2.
* Có cái nhìn rõ hơn về thời điểm áp dụng VPN, VPC Peering và Transit Gateway trong môi trường AWS.
