---
title: "Chuẩn bị môi trường"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Yêu cầu

| Hạng mục | Phiên bản / ghi chú |
| --- | --- |
| **Tài khoản AWS** | Lab FCJ / tài khoản có quyền deploy Amplify sandbox |
| **Node.js** | 18 trở lên |
| **npm** | Đi kèm Node |
| **AWS CLI** | Đã cấu hình Access Key + region |
| **Region** | `ap-southeast-1` (Singapore) |
| **Git** | Clone repo public |

### Bước 1: Đăng nhập AWS Management Console

1. Mở [https://console.aws.amazon.com](https://console.aws.amazon.com).
2. Đăng nhập bằng tài khoản lab (root hoặc IAM user được cấp).
3. Ở góc trên bên phải, xác nhận **Account ID** và tên user.

![Đăng nhập AWS Console](/images/5-Workshop/5.1-Prerequisite/console-login.png)

Góc trên phải hiển thị tài khoản **DHgLang** và Account ID **618647024917**.

![Account trên Console](/images/5-Workshop/5.1-Prerequisite/console-account.png)

---

### Bước 2: Chọn region Singapore

1. Click menu **region** (góc trên phải).
2. Chọn **Asia Pacific (Singapore) `ap-southeast-1`**.
3. Mọi dịch vụ sandbox (Cognito, DynamoDB, Lambda, API Gateway…) sẽ tạo ở region này — trừ WAF gắn CloudFront (dùng `us-east-1` sau này).

![Chọn region ap-southeast-1](/images/5-Workshop/5.1-Prerequisite/console-region.png)

Region trên Console phải khớp với AWS CLI (`aws configure` → Default region `ap-southeast-1`).

![AWS region trong CLI / config](/images/5-Workshop/5.1-Prerequisite/aws-region.png)

---

### Bước 3: Kiểm tra IAM (user / quyền)

1. Vào **IAM** → **Users** (hoặc **Roles** nếu dùng role lab).
2. Mở user đang dùng → tab **Permissions**.
3. Xác nhận có quyền triển khai: CloudFormation, Lambda, API Gateway, DynamoDB, SQS, Cognito, S3, IAM (PassRole), CloudWatch, Amplify.

![IAM Users / Permissions](/images/5-Workshop/5.1-Prerequisite/iam-permissions.png)

{{% notice warning %}}
Không chụp **Access Key Secret** đầy đủ. Chỉ chụp danh sách key (che Secret) hoặc màn Permissions.
{{% /notice %}}

Nếu cần tạo Access Key cho CLI:

1. IAM → Users → user của bạn → **Security credentials**.
2. **Create access key** → chọn **Command Line Interface (CLI)** → tạo key.
3. Lưu Access Key ID + Secret (chỉ hiện 1 lần).

![Tạo Access Key — chọn CLI](/images/5-Workshop/5.1-Prerequisite/iam-access-key-usecase.png)

![Retrieve access keys (che Secret)](/images/5-Workshop/5.1-Prerequisite/iam-access-key.png)

---

### Bước 4: Cài Node.js và kiểm tra

1. Tải Node.js LTS từ [https://nodejs.org](https://nodejs.org) (Windows Installer).
2. Cài đặt, giữ tùy chọn thêm Node vào PATH.
3. Mở PowerShell:

```powershell
node -v
npm -v
```

Kỳ vọng Node ≥ 18 (ví dụ `v20.x` / `v24.x`).

![Node.js version](/images/5-Workshop/5.1-Prerequisite/node-version.png)

---

### Bước 5: Cài và cấu hình AWS CLI

1. Tải AWS CLI v2: [AWS CLI install guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
2. Cài xong, mở PowerShell:

```powershell
aws --version
aws configure
```

Nhập lần lượt:

- AWS Access Key ID  
- AWS Secret Access Key  
- Default region: `ap-southeast-1`  
- Default output: `json`

3. Kiểm tra đăng nhập:

```powershell
aws sts get-caller-identity
```

Kết quả có `Account`, `Arn`, `UserId` là đúng — ARN phải dạng `...user/Van_Huy` (IAM user), không dùng root.

![AWS CLI identity — Van_Huy](/images/5-Workshop/5.1-Prerequisite/aws-cli.png)

---

### Bước 6: Clone mã nguồn và cài dependency

```powershell
git clone https://github.com/DHgLang/Movie-Ticket-Booking-Platform.git
cd Movie-Ticket-Booking-Platform
npm install
```

Project có các thư mục chính: `amplify/` (backend), `frontend/` (React), `scripts/` (deploy).

![Cấu trúc project](/images/5-Workshop/5.1-Prerequisite/project-structure.png)

---

### Bước 7: Tạo file `.env`

```powershell
copy .env.example .env
notepad .env
```

| Biến | Mục đích |
| --- | --- |
| `VITE_API_URL` | URL API Gateway (điền sau `npm run sandbox`) |
| `VITE_TMDB_API_KEY` | API key poster TMDB |
| `TMDB_API_KEY` | Cùng key — phía Lambda |
| `VNPAY_TMN_CODE`, `VNPAY_HASH_SECRET` | VNPay sandbox |
| `FRONTEND_URL`, `VNPAY_RETURN_URL` | Callback sau thanh toán |

Che secret trước khi chụp.

![File .env (che secret)](/images/5-Workshop/5.1-Prerequisite/env-file.png)

{{% notice info %}}
**Lưu ý:** Không commit `.env` và `amplify_outputs.json` — đã có trong `.gitignore`.
{{% /notice %}}

---

### Bước 8: Kiểm tra nhanh trên Console (trước khi sandbox)

Trước khi chạy `npm run sandbox`, mở Console (region Singapore) và ghi nhận tài khoản “sạch” hoặc sẵn sàng tạo resource:

| Dịch vụ | Việc kiểm tra |
| --- | --- |
| **CloudFormation** | Biết chỗ xem stack sau khi sandbox deploy |
| **DynamoDB** | Tables (có thể đang trống) |
| **Cognito** | User pools |
| **Lambda** | Functions |

**Bước tiếp theo**

Sau khi chuẩn bị xong → [5.2 Triển khai Backend](../5.2-Backend/)
