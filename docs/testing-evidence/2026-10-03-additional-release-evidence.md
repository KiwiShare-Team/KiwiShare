# Additional release test evidence - 2026-10-03

All timestamps use Pacific/Auckland unless the captured protocol output states UTC.

## Production API checks

Target: `https://kiwishare.onrender.com`

### KS-T-050 - Backend health on Render

```text
GET /health
HTTP/2 200
x-render-origin-server: Render

OK
```

### KS-T-051 - Unauthenticated access

```text
GET /api/users/me
HTTP/2 401

{"status":"error","message":"Access denied. Missing authorization token."}
```

## KS-T-056 - API latency baseline and light load

Command:

```bash
API_PERF_BASE_URL=https://kiwishare.onrender.com \
API_PERF_OUTPUT=docs/testing-evidence/api-performance/2026-10-03-render-follow-up.json \
pnpm perf:api
```

Results:

| Metric | Result |
| --- | ---: |
| Requests | 80 |
| Successful | 80 |
| Failed | 0 |
| Error rate | 0% |
| Concurrency | 4 |
| Overall p50 | 620.01 ms |
| Overall p95 | 1,407.51 ms |
| Throughput | 5.31 requests/s |
| Smoke result | Pass |

The raw machine-readable result is stored in
[`api-performance/2026-10-03-render-follow-up.json`](./api-performance/2026-10-03-render-follow-up.json).

## Automated regression

Commands:

```bash
pnpm --filter web test
pnpm --filter server test -- --runInBand
```

Results:

- Web: 8 passed, 0 failed.
- Server: 24 suites passed, 264 tests passed, 0 failed.

Case-specific automated evidence includes:

- `KS-T-033`: price-drop notification policy, preferences, quota, idempotency and provider-failure handling passed in `server/test/priceNotifications.test.ts`.
- `KS-T-041`: accepted chat text is sanitised before storage and return; message-moderation normalisation and rejection tests passed in `server/test/chat.test.ts` and `server/test/messageModeration.test.ts`.
- `KS-T-048`: duplicate report submissions are rejected without creating another receipt or history entry in `server/test/reports.test.ts`.
- `KS-T-051`: protected endpoints reject requests without authentication in the server suite and the live production check above.
- `KS-T-052`: OTP rate limiting passed in `server/test/app.test.ts`; this validation used the isolated test environment.

## KS-T-070 - Production web smoke

Target: `https://kiwishare.online`

Verified in the production web client:

- the login page loaded with password and email-OTP options;
- the marketplace loaded 46 items with images, prices, categories and locations;
- an item detail page loaded its image, title, price, seller, trust score, location and description;
- the browser console showed no blocking errors during the smoke flow.

Screenshots:

- [`ks-t-070-web-login.png`](./screenshots/2026-10-03/ks-t-070-web-login.png)
- [`ks-t-070-web-home.png`](./screenshots/2026-10-03/ks-t-070-web-home.png)
- [`ks-t-070-web-item-detail.png`](./screenshots/2026-10-03/ks-t-070-web-item-detail.png)

The item-detail screenshot also records `KS-T-031` seller identity evidence: seller name, profile image, student-verification status and trust score are visible together.

## Physical-iPhone evidence supplied by the team

- `KS-T-006`: OTP email, invalid/expired OTP error, and resend countdown.
- `KS-T-009`: signed-out Chat and Watchlist screens require authentication.
- `KS-T-067`: Rui's evidence shows physical-iPhone order, payment, meetup, reporting and seller-profile states, plus Android reports, profile and marketplace discovery.
- `KS-T-068`: Sam's physical-iPhone screenshot shows an authenticated own-listing detail page and the owner watchlist restriction.
- `KS-T-058`, `KS-T-060`, `KS-T-061`: Android dark-mode screenshots show login, marketplace discovery and Watchlist error/Retry states.

Rui physical-device photographs:

- [`ks-t-067-rui-orders-physical-iphone.jpg`](./screenshots/2026-10-03/ks-t-067-rui-orders-physical-iphone.jpg)
- [`ks-t-067-rui-trade-chat-physical-iphone.jpg`](./screenshots/2026-10-03/ks-t-067-rui-trade-chat-physical-iphone.jpg)
- [`ks-t-067-rui-report-listing-iphone.jpg`](./screenshots/2026-10-03/ks-t-067-rui-report-listing-iphone.jpg)
- [`ks-t-067-rui-public-profile-iphone.jpg`](./screenshots/2026-10-03/ks-t-067-rui-public-profile-iphone.jpg)
- [`ks-t-067-rui-reports-android.png`](./screenshots/2026-10-03/ks-t-067-rui-reports-android.png)
- [`ks-t-067-rui-profile-android.png`](./screenshots/2026-10-03/ks-t-067-rui-profile-android.png)
- [`ks-t-067-rui-home-android.png`](./screenshots/2026-10-03/ks-t-067-rui-home-android.png)
- [`ks-t-068-sam-own-listing-watchlist-iphone.jpg`](./screenshots/2026-10-03/ks-t-068-sam-own-listing-watchlist-iphone.jpg)

Android dark-mode screenshots:

- [`ks-t-058-dark-login-android.png`](./screenshots/2026-10-03/ks-t-058-dark-login-android.png)
- [`ks-t-060-dark-home-android.png`](./screenshots/2026-10-03/ks-t-060-dark-home-android.png)
- [`ks-t-061-dark-watchlist-error-android.png`](./screenshots/2026-10-03/ks-t-061-dark-watchlist-error-android.png)

Privacy-sensitive email addresses are redacted in repository screenshots. One-time OTP values are retained as test evidence.

## Device matrix supplied by the team

- Jenny: iPhone 17
- Jack: iPhone 16 Pro, iOS 26.6.2
- Rui: iPhone 15
- Sam: iPhone, model not recorded
- Jaeger: Samsung S24 Ultra, Android version not recorded

## Additional physical-device evidence

- `KS-T-071`: Jack's iPhone 16 Pro running iOS 26.6.2 shows the dark-mode listing flow, including a one-photo upload, completed title and description, category, price, Auckland location, condition, sustainable-item control and the `Post item` action. The same device then shows the resulting marketplace listing and item-detail view.
- `KS-T-072`: Jaeger's Samsung S24 Ultra shows the authenticated Chat list with All, Unread, Buying and Selling filters, multiple item-linked conversations and an unread-count badge.
- `KS-T-073`: Jaeger's Samsung S24 Ultra provides additional Android chat evidence for `KS-T-039` (photo messages), `KS-T-040` (voice messages), `KS-T-061` (dark-mode chat) and `KS-T-069` (Android physical-device regression). The conversation shows two image messages and a 0:02 voice message in both dark and light mode, with the chat composer visible.
- `KS-T-074`: Jaeger's Samsung S24 Ultra shows seller-side listing status synchronisation in chat. After the seller removes the `Motorcycle combo` listing, the conversation displays `DELISTED`, the seller action confirmation, and the available `Relist Item` action. This supplements the chat/listing lifecycle evidence without being counted as a completed trade.

Screenshots:

- [`ks-t-071-jack-list-item-dark-iphone16pro.jpg`](./screenshots/2026-10-03/ks-t-071-jack-list-item-dark-iphone16pro.jpg)
- [`ks-t-071-jack-list-item-fields-dark-iphone16pro.jpg`](./screenshots/2026-10-03/ks-t-071-jack-list-item-fields-dark-iphone16pro.jpg)
- [`ks-t-071-jack-marketplace-dark-iphone16pro.jpg`](./screenshots/2026-10-03/ks-t-071-jack-marketplace-dark-iphone16pro.jpg)
- [`ks-t-071-jack-item-detail-dark-iphone16pro.jpg`](./screenshots/2026-10-03/ks-t-071-jack-item-detail-dark-iphone16pro.jpg)
- [`ks-t-072-jaeger-chat-list-samsung-s24-ultra.jpg`](./screenshots/2026-10-03/ks-t-072-jaeger-chat-list-samsung-s24-ultra.jpg)
- [`ks-t-073-jaeger-chat-photo-voice-dark-samsung-s24-ultra.jpg`](./screenshots/2026-10-03/ks-t-073-jaeger-chat-photo-voice-dark-samsung-s24-ultra.jpg)
- [`ks-t-073-jaeger-chat-photo-voice-light-samsung-s24-ultra.jpg`](./screenshots/2026-10-03/ks-t-073-jaeger-chat-photo-voice-light-samsung-s24-ultra.jpg)
- [`ks-t-073-jaeger-chat-paid-photo-light-samsung-s24-ultra.jpg`](./screenshots/2026-10-03/ks-t-073-jaeger-chat-paid-photo-light-samsung-s24-ultra.jpg)
- [`ks-t-074-jaeger-chat-delisted-relist-samsung-s24-ultra.jpg`](./screenshots/2026-10-03/ks-t-074-jaeger-chat-delisted-relist-samsung-s24-ultra.jpg)
