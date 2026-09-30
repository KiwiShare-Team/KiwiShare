# KiwiShare — Assessor Live Demonstration & Quick-Test Guide

**Course**: COMPSCI 734 · University of Auckland  
**Team**: Five Guys  
**Project**: KiwiShare (Sustainable Second-Hand Community Marketplace)

---

## 1. Live Deployed Environments & Access Links

Assessors can interact with the live production deployment immediately without configuring local toolchains:

| Component | Production Access URL | Notes |
| :--- | :--- | :--- |
| **Web Portal (React + Vite)** | **[https://kiwishare.online](https://kiwishare.online)** | Hosted as static assets on Cloudflare Workers with SPA edge routing |
| **Backend RESTful API (Koa)** | **[https://kiwishare.onrender.com](https://kiwishare.onrender.com)** | Deployed Node.js 22 LTS container on Render Cloud |
| **MongoDB Atlas Production Cluster** | `kiwishare.ll1go7c.mongodb.net/kiwishare_prod` | High-availability 3-node replica set with GeoJSON indexes |
| **Android APK Direct Download** | **[GitHub Releases](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/releases)** | Download `KiwiShare-Android-vX.X.X.apk` directly to test on any Android phone |
| **Firebase App Distribution** | Automated CI/CD Stream | Automatic deployment of Android builds to registered testers |

---

## 2. Pre-Seeded Evaluation Accounts

The production database is pre-populated with realistic New Zealand community listings and test accounts:

> **Universal Test Password for all accounts**: `password123`

| Account Email | Display Name | Role / Persona | Ideal For Evaluating |
| :--- | :--- | :--- | :--- |
| **`admin@kiwishare.online`** | Kiwi Admin | **Platform Administrator** | Governance Dashboard, GMV & Platform fee analytics, Item Moderation, User TrustScore & Ban controls. |
| **`moviegoer24@gmail.com`** | MovieGoer24 | **Verified Student Seller / Admin** | Presenting QR Handover codes, creating AI-assisted listings, voice chat messages. |
| **`testbuyer@kiwishare.online`** | Alex Turner | **Verified Student Buyer** | Scanning Seller's QR Handover code, exploring localized feed by distance, chat. |
| **`sophie.nz@kiwishare.online`** | Sophie Chen | **Community Member** | Viewing multi-user marketplace interactions and listings across Auckland, Wellington, Christchurch. |

---

## 3. Fast Assessment Walkthroughs (3 to 5 Minutes)

### Scenario A: Web Administration & Governance Dashboard
1. Navigate to **[https://kiwishare.online/login](https://kiwishare.online/login)**.
2. Sign in with:
   - **Email**: `admin@kiwishare.online`
   - **Password**: `password123`
3. Notice the **Admin Board** button appearing in the top navigation bar. Click it or visit **[https://kiwishare.online/admin](https://kiwishare.online/admin)**.
4. **Evaluate Key Capabilities**:
   - **Overview Tab**: Live GMV, Platform Fees (NZD), Order status breakdown, User and Item counts.
   - **Categories Tab**: Inspect and toggle activation status and sort order for the 10 marketplace categories.
   - **Items Tab**: Review active listings with real Auckland locations; test the **Reassign/Transfer Item** ownership function.
   - **Users Tab**: Inspect verified student badges, adjust Trust Scores, and test ban/unban moderation.

---

### Scenario B: Mobile In-Person QR Handover Verification
*Demonstrates: Native Camera (`mobile_scanner`), Dynamic QR generation (`qr_flutter`), and atomic database state transitions.*

1. **Install Mobile App**:
   - Download the latest APK from **[GitHub Releases](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/releases)** onto an Android phone/emulator, or run via `pnpm dev` in Chrome.
2. **Seller Side (Display QR Code)**:
   - Sign in as `moviegoer24@gmail.com` (`password123`).
   - Navigate to **Meetups / Orders** -> select order **`#ORD_DEMO_MEETUP_2026`** (*Retro Velvet Armchair*).
   - Observe the confirmed meetup location (*Ponsonby Central, Auckland*) and the generated **Seller's Handover QR Code**.
3. **Buyer Side (Scan & Verify)**:
   - On a second phone (or logged in as `testbuyer@kiwishare.online`):
   - Open order `#ORD_DEMO_MEETUP_2026` -> Tap **"Scan Handover QR Code"**.
   - Aim the camera at the Seller's QR screen.
   - **Outcome**: The trusted buyer completes the handover through the valid QR flow. The transaction commits atomically, the item status changes to `sold`, and the buyer and seller each receive exactly **+5 Trust Score points once**. Public profile and product views show exact scores through 200 and `200+` above 200, while administrators continue to see the exact stored score.

*(Alternatively, evaluate the reverse order `#ORD_DEMO_BIKE_SCANNER_2026` where `moviegoer24@gmail.com` acts as the Buyer scanning `testbuyer@kiwishare.online`'s bicycle).*

---

### Scenario C: Voice Messaging & Cloudflare R2 Uploads
1. Inside the mobile app, open any item and tap **Chat with Seller**.
2. **Audio Microphone**: Hold down the microphone button to record a voice message.
3. Release to send: The mobile app compresses the audio, streams it to the Koa backend, where `ffmpeg-static` sanitizes and transcodes the binary.
4. Tap Play: Visual audio playback with waveform scrubber powered by `just_audio`.
5. **Image Upload**: Tap the image icon to upload a listing photo via Cloudflare R2 presigned S3 URLs.

---

### Scenario D: Smart Discovery, Recommendations & Share Handoff

1. On mobile, tap the home search field to enter the dedicated full-screen marketplace search.
2. Type a partial phrase and observe server-backed search-as-you-type suggestions across active marketplace data.
3. Return to Home and compare the recommendation feed: ranking incorporates watchlist affinity, location proximity, engagement, freshness, sustainability and item condition.
4. Open a listing and share/copy its public `/s/:itemId` link. On a mobile browser the branded landing page attempts the `kiwishare:///items/:itemId` deep link; without the app it exposes install options and **Continue on web**.
5. In the publish flow, demonstrate AI-assisted listing content from seller context/images and confirm that the seller reviews the generated suggestion before publishing.

> **Accuracy note:** the current search autocomplete is deterministic/server-backed. The mobile app contains a Remote Config flag for an AI/semantic-search experiment, but the current suggestion endpoint itself is not model-generated.

---

### Scenario E: Safe Pay → Meetup → Verified Handover

1. Open an active buyer/seller order on mobile or web.
2. Demonstrate that payment and meetup agreement can occur in either order while the backend keeps a single authoritative transaction state.
3. Complete a Stripe-backed Safe Pay step or use a seeded paid order, then inspect the saved-card/wallet and order-detail state.
4. Confirm the meetup and verify that the handover action/QR unlocks only when both payment and meetup requirements are satisfied.
5. Complete handover and observe the connected effects: order completion, ownership transfer, seller-release state, Trust Score credit and review eligibility.

> **Payment wording:** KiwiShare implements an escrow-style application state and seller-release workflow around Stripe payment verification. It should not be presented as a regulated escrow institution or as proof of a segregated escrow account.

---

## 4. Architectural Key Highlights for Evaluators

- **Decoupled Architecture**: Koa RESTful API + MongoDB Atlas + React Web + Flutter Multiplatform.
- **Atomic Operations**: Critical handover transfers execute within `runMongoTransaction` sessions to prevent race conditions.
- **Cloud-Native Edge Storage**: Cloudflare R2 S3-compatible presigned uploads keep large listing/chat media off the application server data path.
- **Marketplace Feedback Loops**: Watchlists influence recommendation ranking and notifications; completed transactions influence reputation; KiwiGold/VIP promotion influences featured discovery.
- **Cross-Platform Transaction Source of Truth**: Flutter and React consume the same server-derived payment/meetup/handover readiness instead of implementing separate client rules.
- **Native-to-Web Share Handoff**: `/s/:itemId` provides an external share surface that can reopen an installed app through `kiwishare://` or fall back to install/web continuation.
- **Automated CI/CD**: Web changes merged to `pre` build and deploy the React portal to Cloudflare Workers when the deployment token is configured; merges to `main` trigger Semantic Versioning tags, Android APK generation, Firebase distribution, and the independent Render backend release flow.
