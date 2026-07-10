---
title: "Application Demo"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Live application demo

URL: **https://main.d2zv6ka00i1nyo.amplifyapp.com**

**Customer flow**

**1. Browse movies** — The homepage shows posters and details for currently showing films.

![Homepage](/images/5-Workshop/5.4-Demo/home-movies.png)

**2. Select showtime** — Pick date, time, and screening room.

![Showtimes](/images/5-Workshop/5.4-Demo/showtimes.png)

**3. Pick seats** — The seat map lets you choose available seats; booked seats appear in a different color.

![Seat map](/images/5-Workshop/5.4-Demo/seat-map.png)

**4. Checkout** — Confirm seats and total on the **Your order** panel, then click **Checkout** to go to payment.

![Checkout](/images/5-Workshop/5.4-Demo/checkout.png)

**5. VNPay sandbox** — Redirect to the VNPay test page for a trial payment.

![VNPay](/images/5-Workshop/5.4-Demo/vnpay.png)

**6. E-ticket** — After successful payment, the system issues an e-ticket with a ticket code (`TICKET:...`) for cinema check-in.

![Ticket](/images/5-Workshop/5.4-Demo/ticket-qr.png)

**Sign-in (Cognito)**

Users sign up or sign in via Cognito Hosted UI or the in-app form.

![Login](/images/5-Workshop/5.4-Demo/login.png)

**Admin panel**

Sign in with an account in the `admin` group:

**Dashboard** — Overview of revenue, bookings, and system status.

![Admin dashboard](/images/5-Workshop/5.4-Demo/admin-dashboard.png)

**Movies** — Add, edit, or remove movies and sync posters from TMDB.

![Admin movies](/images/5-Workshop/5.4-Demo/admin-movies.png)

**Bookings** — View booking list and payment status.

![Admin bookings](/images/5-Workshop/5.4-Demo/admin-bookings.png)

**Traffic / RUM** — JS error and page load charts from CloudWatch RUM.

![Admin traffic](/images/5-Workshop/5.4-Demo/admin-traffic.png)

In the AWS Console, CloudWatch RUM records page views, load time, and Web Vitals (LCP, INP, etc.) for the live app.

![CloudWatch RUM Console](/images/5-Workshop/5.4-Demo/cloudwatch-rum.png)

#### Security checks

| Check | Expected |
| --- | --- |
| HTTPS | URLs use `https://` |
| Cognito | Checkout / tickets require signed-in user |
| WAF | Anomalous requests counted/blocked in WAF metrics |
| API `/health` | Returns `mode: "aws"` when pointing to real backend |

The `/health` endpoint returns JSON with `ok: true` and `mode: "aws"` when connected to the sandbox.

![Health API](/images/5-Workshop/5.4-Demo/health-api.png)

#### Key takeaways

- **Serverless + queue** absorbs traffic spikes without overloading the database.
- **WAF on CloudFront** is the first defense layer before traffic reaches the app.
- **Cognito** separates regular users and admins — simple RBAC model.
- **RUM + Admin Traffic** observes real user experience, not just server metrics.

**Next step**

→ [5.5 Resource Cleanup](../5.5-Cleanup/)
