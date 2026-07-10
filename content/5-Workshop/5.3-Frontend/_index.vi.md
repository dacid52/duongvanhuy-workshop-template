---
title: "Frontend & Deploy"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Chạy frontend local

```powershell
npm run dev
```

Mở http://localhost:5173 — frontend gọi API qua `VITE_API_URL`.

Vite dev server chạy tại port 5173; trang chủ load danh sách phim từ API backend.

![Dev server](/images/5-Workshop/5.3-Frontend/dev-server.png)

#### Build production

```powershell
npm run build
```

Output nằm trong `frontend/dist/`.

#### Deploy lên Amplify Hosting

App production: **https://main.d2zv6ka00i1nyo.amplifyapp.com**

Có thể deploy bằng script:

```powershell
.\scripts\deploy-hosting.ps1
```

Script build zip từ `frontend/dist`, upload lên Amplify app `d2zv6ka00i1nyo`, branch `main`.

Console Amplify Hosting báo deploy thành công sau khi upload zip production build.

![Amplify Hosting — deploy success](/images/5-Workshop/5.3-Frontend/amplify-hosting.png)

Truy cập URL live `main.d2zv6ka00i1nyo.amplifyapp.com` để xác nhận frontend production hoạt động.

![Trang chủ live](/images/5-Workshop/5.3-Frontend/live-url.png)

#### Phân phối & bảo vệ Edge

Amplify Hosting phía sau dùng **Amazon CloudFront** làm CDN — URL production kết thúc bằng `amplifyapp.com`. Lớp edge này phân phối HTML/JS/CSS tới người dùng toàn cầu với HTTPS.

Trên Console Amplify, mục **Enable firewall protections** đã bật — frontend được bảo vệ bởi **AWS WAF** (scope CloudFront) với các managed rules cơ bản.

Web ACL `spirit-movie-frontend` được tạo ở region **US East (N. Virginia)** — yêu cầu của AWS khi WAF gắn CloudFront — gồm 5 rules lọc request độc hại trước khi traffic tới ứng dụng.

![WAF Web ACL](/images/5-Workshop/5.3-Frontend/waf-web-acl.png)

**Bước tiếp theo**

→ [5.4 Demo ứng dụng](../5.4-Demo/)
