---
title: "Triển khai Backend"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Tổng quan

Backend được định nghĩa trong thư mục `amplify/` (Amplify Gen 2). Lệnh sandbox tạo:

- **Amazon Cognito** — User Pool + Identity Pool
- **HTTP API (API Gateway)** — REST endpoints
- **Lambda** — `booking-api`, `booking-worker`
- **DynamoDB** — bảng dữ liệu booking
- **SQS + DLQ** — hàng đợi xử lý đơn
- **S3** — bucket poster phim
- **CloudWatch RUM** — giám sát frontend

#### Deploy sandbox

```powershell
cd Movie-Ticket-Booking-Platform
npm run sandbox
```

Lần chạy đầu có thể mất 5–15 phút. Khi thành công, terminal hiển thị `apiUrl` và ghi `amplify_outputs.json`.

Terminal báo deploy thành công khi CloudFormation stack ở trạng thái `UPDATE_COMPLETE` và in ra URL API.

![Sandbox deploy thành công](/images/5-Workshop/5.2-Backend/sandbox-deploy.png)

#### Cập nhật `.env`

Dán `apiUrl` từ output vào `.env`:

```
VITE_API_URL=https://xxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
```

File `amplify_outputs.json` chứa `apiUrl`, Cognito pool ID và các output khác — copy `apiUrl` sang `.env`.

![amplify_outputs.json](/images/5-Workshop/5.2-Backend/amplify-outputs.png)

#### Kiểm tra trên AWS Console

| Dịch vụ | Kiểm tra |
| --- | --- |
| **Cognito** | User pool, app client, nhóm `admin` |
| **API Gateway** | Routes: `/health`, `/movies`, `/bookings`, `/payments/vnpay/*` |
| **DynamoDB** | Tables được tạo bởi sandbox |
| **SQS** | Booking queue + Dead Letter Queue |
| **Lambda** | 2 functions: API + worker |

**Cognito** — User Pool được tạo tự động, có app client và nhóm `admin` để phân quyền.

![Cognito User Pool](/images/5-Workshop/5.2-Backend/cognito-pool.png)

**API Gateway** — các route REST `/health`, `/movies`, `/bookings`, `/payments/vnpay/*` đã được đăng ký.

![API Gateway routes](/images/5-Workshop/5.2-Backend/api-gateway.png)

**DynamoDB** — sandbox tạo các bảng lưu phim, suất chiếu, ghế và booking.

![DynamoDB tables](/images/5-Workshop/5.2-Backend/dynamodb-tables.png)

**SQS** — hàng đợi booking kèm Dead Letter Queue để xử lý message lỗi.

![SQS queue](/images/5-Workshop/5.2-Backend/sqs-queue.png)

**Lambda** — hai function `booking-api` (đồng bộ) và `booking-worker` (xử lý queue).

![Lambda functions](/images/5-Workshop/5.2-Backend/lambda-functions.png)

#### Tạo tài khoản Admin

1. Đăng ký user qua app hoặc Cognito Console.
2. Thêm user vào nhóm **admin**:

```powershell
aws cognito-idp admin-add-user-to-group `
  --user-pool-id <USER_POOL_ID> `
  --username <email> `
  --group-name admin `
  --region ap-southeast-1
```

#### Kiểm tra API

```powershell
curl https://<API_URL>/health
```

Kết quả mong đợi: `{"ok":true,"mode":"aws","vnpayEnabled":...}`

Gọi `GET /health` trả JSON xác nhận backend AWS đang chạy và VNPay đã bật hay chưa.

![Health endpoint](/images/5-Workshop/5.4-Demo/health-api.png)

{{% notice warning %}}
**Quan trọng:** Không chạy `ampx sandbox delete` nếu muốn giữ dữ liệu phim và booking đã nhập. Sandbox delete sẽ xóa toàn bộ backend.
{{% /notice %}}

**Bước tiếp theo**

→ [5.3 Frontend & Deploy Hosting](../5.3-Frontend/)
