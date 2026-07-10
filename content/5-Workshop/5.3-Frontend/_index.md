---
title: "Frontend & Deploy"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Run frontend locally

```powershell
npm run dev
```

Open http://localhost:5173 — the app calls the API via `VITE_API_URL`.

The Vite dev server runs on port 5173; the homepage loads the movie list from the backend API.

![Dev server](/images/5-Workshop/5.3-Frontend/dev-server.png)

#### Production build

```powershell
npm run build
```

Output is in `frontend/dist/`.

#### Deploy to Amplify Hosting

Production app: **https://main.d2zv6ka00i1nyo.amplifyapp.com**

Deploy using the script:

```powershell
.\scripts\deploy-hosting.ps1
```

The script zips `frontend/dist`, uploads to Amplify app `d2zv6ka00i1nyo`, branch `main`.

The Amplify Hosting console shows a successful deploy after uploading the production build zip.

![Amplify Hosting — deploy success](/images/5-Workshop/5.3-Frontend/amplify-hosting.png)

Open the live URL `main.d2zv6ka00i1nyo.amplifyapp.com` to confirm the production frontend works.

![Live homepage](/images/5-Workshop/5.3-Frontend/live-url.png)

#### Edge delivery & protection

Amplify Hosting is backed by **Amazon CloudFront** as CDN — the production URL ends with `amplifyapp.com`. This edge layer distributes HTML/JS/CSS globally over HTTPS.

In the Amplify console, **Enable firewall protections** is turned on — the frontend is protected by **AWS WAF** (CloudFront scope) with baseline managed rules.

Web ACL `spirit-movie-frontend` is created in **US East (N. Virginia)** — required for CloudFront WAF — with 5 rules filtering malicious requests before traffic reaches the app.

![WAF Web ACL](/images/5-Workshop/5.3-Frontend/waf-web-acl.png)

**Next step**

→ [5.4 Application Demo](../5.4-Demo/)
