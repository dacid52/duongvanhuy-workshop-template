---
title: "Demo ứng dụng"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Demo ứng dụng live

Truy cập: **https://main.d2zv6ka00i1nyo.amplifyapp.com**

**Luồng khách hàng**

**1. Duyệt phim** — Trang chủ hiển thị poster và thông tin phim đang chiếu.

![Trang chủ](/images/5-Workshop/5.4-Demo/home-movies.png)

**2. Chọn suất chiếu** — Chọn ngày, giờ và phòng chiếu phù hợp.

![Suất chiếu](/images/5-Workshop/5.4-Demo/showtimes.png)

**3. Chọn ghế** — Seat map cho phép chọn ghế trống; ghế đã đặt hiển thị khác màu.

![Seat map](/images/5-Workshop/5.4-Demo/seat-map.png)

**4. Thanh toán** — Xác nhận ghế và tổng tiền trên panel **Your order**, rồi bấm **Checkout** để chuyển sang cổng thanh toán.

![Checkout](/images/5-Workshop/5.4-Demo/checkout.png)

**5. VNPay sandbox** — Redirect sang trang VNPay test để thanh toán thử.

![VNPay](/images/5-Workshop/5.4-Demo/vnpay.png)

**6. Vé điện tử** — Sau thanh toán thành công, hệ thống phát hành e-ticket kèm mã vé (`TICKET:...`) để check-in tại rạp.

![Vé](/images/5-Workshop/5.4-Demo/ticket-qr.png)

**Đăng nhập (Cognito)**

Người dùng đăng ký/đăng nhập qua Cognito Hosted UI hoặc form trong app.

![Đăng nhập](/images/5-Workshop/5.4-Demo/login.png)

**Bảng quản trị Admin**

Admin đăng nhập bằng tài khoản thuộc nhóm `admin`:

**Dashboard** — Tổng quan doanh thu, số booking và trạng thái hệ thống.

![Admin dashboard](/images/5-Workshop/5.4-Demo/admin-dashboard.png)

**Quản lý phim** — Thêm, sửa, xóa phim và đồng bộ poster từ TMDB.

![Admin movies](/images/5-Workshop/5.4-Demo/admin-movies.png)

**Booking** — Xem danh sách đơn đặt vé và trạng thái thanh toán.

![Admin bookings](/images/5-Workshop/5.4-Demo/admin-bookings.png)

**Traffic / RUM** — Biểu đồ lỗi JS, thời gian tải trang từ CloudWatch RUM.

![Admin traffic](/images/5-Workshop/5.4-Demo/admin-traffic.png)

Trên AWS Console, CloudWatch RUM ghi nhận page views, load time và Web Vitals (LCP, INP…) của app live.

![CloudWatch RUM Console](/images/5-Workshop/5.4-Demo/cloudwatch-rum.png)

#### Kiểm tra bảo mật

| Kiểm tra | Kỳ vọng |
| --- | --- |
| HTTPS | URL bắt đầu bằng `https://` |
| Cognito | Chỉ user đăng nhập mới checkout / xem vé |
| WAF | Request bất thường bị Count/Block trên WAF metrics |
| API `/health` | Trả `mode: "aws"` khi trỏ backend thật |

Endpoint `/health` trả JSON `ok: true` và `mode: "aws"` khi kết nối đúng sandbox.

![Health API](/images/5-Workshop/5.4-Demo/health-api.png)

#### Bài học rút ra

- **Serverless + queue** giúp hệ thống chịu tải đỉnh mà không làm sập database.
- **WAF trên CloudFront** là lớp phòng thủ đầu tiên trước khi traffic tới ứng dụng.
- **Cognito** tách rõ user thường và admin — phù hợp mô hình RBAC đơn giản.
- **RUM + Admin Traffic** hỗ trợ quan sát trải nghiệm người dùng thực tế, không chỉ metric server.

**Bước tiếp theo**

→ [5.5 Dọn dẹp tài nguyên](../5.5-Cleanup/)
