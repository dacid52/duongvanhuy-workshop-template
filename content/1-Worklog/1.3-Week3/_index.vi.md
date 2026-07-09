---
title: "Worklog Tuần 3"
date: 2026-05-11
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:
* Triển khai EC2 trên các subnet và kiểm tra khả năng kết nối trong kiến trúc mạng.
* Cấu hình NAT Gateway, EC2 Instance Connect Endpoint và mô hình Hybrid DNS với Route 53.
* Tìm hiểu AWS Backup cho EC2 và EBS, bao gồm giám sát và kiểm tra khôi phục dữ liệu.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - Khởi tạo EC2 trên các subnet đã quy hoạch.<br>- Kiểm tra đường truyền giữa các thành phần trong kiến trúc mạng nội bộ. | 04/05/2026 | 04/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Cấu hình NAT Gateway cho nhu cầu truy cập outbound từ tài nguyên private.<br>- Tìm hiểu vai trò của EC2 Instance Connect Endpoint trong truy cập quản trị an toàn. | 05/05/2026 | 05/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Thiết lập các thành phần Route 53 phục vụ mô hình Hybrid DNS.<br>- Theo dõi cách phân giải tên nội bộ giữa các môi trường được kết nối. | 06/05/2026 | 06/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tạo các thành phần AWS Backup:<br>&emsp; + Backup Vault.<br>&emsp; + Backup Plan.<br>&emsp; + Backup selections cho tài nguyên EC2 và EBS. | 07/05/2026 | 07/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Theo dõi trạng thái chạy backup và xem lại log sao lưu.<br>- Thực hiện một tình huống kiểm tra khôi phục để xác nhận cấu hình backup hoạt động đúng như mong đợi. | 08/05/2026 | 08/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 3:
* Hoàn thành việc triển khai EC2 theo subnet và kiểm tra các đường kết nối mạng chính.
* Hiểu rõ hơn vai trò của NAT Gateway, EC2 Instance Connect Endpoint và Route 53 trong truy cập an toàn và phân giải tên.
* Nắm được cách cấu hình AWS Backup và kiểm tra khả năng khôi phục cho EC2 và EBS.
