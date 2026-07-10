---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Việc dọn dẹp tài nguyên AWS sau khi hoàn thành workshop giúp tránh phát sinh chi phí ngoài ý muốn khi hết Free Tier.

{{% notice warning %}}
Chỉ dọn dẹp khi **không còn cần** dữ liệu sandbox. Xóa nhầm sẽ mất toàn bộ phim, user và booking đã tạo.
{{% /notice %}}

#### Checklist

| Tài nguyên | Quyết định |
| --- | --- |
| Amplify sandbox (Lambda, API, DynamoDB, SQS, Cognito…) | **Đã xóa** — `npx ampx sandbox delete` |
| CDKToolkit (CloudFormation) | **Giữ** — bootstrap CDK |

### Bước 1: Xóa Amplify sandbox (backend)

Trong thư mục project:

```powershell
# CHỈ chạy khi chắc chắn muốn xóa backend sandbox
npx ampx sandbox delete
```

Xác nhận trên terminal khi lệnh hỏi delete. Khi xong, CLI báo `Deployment completed` và `Finished deleting` (ví dụ ~3–4 phút).

![Sandbox delete](/images/5-Workshop/5.5-Cleanup/sandbox-delete.png)

Sau khi xóa, kiểm tra **CloudFormation → Stacks** (region `ap-southeast-1`). Stack sandbox Amplify không còn; thường chỉ còn **`CDKToolkit`** (bootstrap CDK — **không xóa**).

![Sandbox đã xóa](/images/5-Workshop/5.5-Cleanup/sandbox-gone.png)

### Bước 2: Kiểm tra DynamoDB

Xác nhận bảng DynamoDB của sandbox đã biến mất — **Tables (0)** trong region `ap-southeast-1`.

![DynamoDB sau cleanup](/images/5-Workshop/5.5-Cleanup/resources-gone.png)

**Bước tiếp theo**

→ [5.6 Tổng quan & chi phí](../5.6-Cost/)
