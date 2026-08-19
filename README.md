# KiwiShare Monorepo - Team Five Guys (COMPSCI 734)

Welcome to the **KiwiShare** Monorepo, a secure and sustainable second-hand marketplace developed for COMPSCI 734.

---

## 1. Project Navigation Dashboard

* **System Architecture**: Detailed native integrations and data models are documented in [architecture.md](file:///Users/samyao/Work/project-implementation-five-guys/docs/architecture.md).
* **API Specifications**: Exposed endpoints and payload mappings are listed in [api-spec.md](file:///Users/samyao/Work/project-implementation-five-guys/docs/api-spec.md).
* **OWASP Top 10 Blueprint**: Cybersecurity risk mitigations are outlined in [security-owasp.md](file:///Users/samyao/Work/project-implementation-five-guys/docs/security-owasp.md).
* **Shared Data Contracts**: JSON schemas are defined in the [docs/schemas/](file:///Users/samyao/Work/project-implementation-five-guys/docs/schemas/) folder.

---

## 2. Directory Structure

```text
kiwishare/
├── .github/
│   ├── workflows/             # CI/CD path-triggered workflows (mobile.yml / backend.yml)
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/                      # Systems design & documentation
│   ├── architecture.md
│   ├── api-spec.md
│   └── security-owasp.md
├── docs/schemas/              # Data contract JSON schemas
│   ├── user.schema.json
│   └── item.schema.json
├── server/                    # Backend RESTful Server (Node.js + Koa + TypeScript + MongoDB)
│   ├── src/                   # Source controllers & middlewares
│   ├── test/                  # Jest integration test suite
│   ├── package.json
│   └── tsconfig.json
├── mobile/                    # Mobile Client (Flutter Cross-Platform App)
│   ├── lib/                   # View components & providers
│   ├── test/                  # Widget & Unit test suite
│   └── pubspec.yaml
├── package.json               # Root monorepo workspace configurations
├── pnpm-workspace.yaml        # Workspace declarations
└── README.md                  # Project entrypoint dashboard
```

---

## 3. One-Click Bootstrap & Launch Guide

Ensure Node.js and `pnpm` are installed (Node.js v20+ recommended).

### Step 1: Install Dependencies (All Subprojects)
Run the bootstrap command from the root directory to automatically resolve packages for both mobile and backend:
```bash
pnpm bootstrap
```

### Step 2: Running Tests (All Subprojects)
Trigger the entire automated test suite (Jest integration tests and Flutter widget/unit tests):
```bash
pnpm test:all
```

### Step 3: Running Static Analysis (All Subprojects)
Perform codebase checks (ESLint for backend TS, and Flutter analyzer for mobile Dart):
```bash
pnpm lint:all
```

### Step 4: Running the App

* **Connect directly to deployed Cloud Backend (`https://kiwishare.onrender.com`)**:
  ```bash
  pnpm dev
  ```
* **Full-stack Local Development (Starts local Koa + MongoDB server & mobile concurrently)**:
  ```bash
  pnpm dev:local
  ```
* **Local Backend on Physical Mobile Device**:
  ```bash
  pnpm dev:device
  ```

Alternatively, you can start the components individually:
* **Backend Development Server (Hot-Reload)**:
  ```bash
  pnpm server:dev
  ```
* **Mobile Client (Cloud Backend)**:
  ```bash
  pnpm mobile:dev
  ```
* **Mobile Client (Local Backend)**:
  ```bash
  pnpm mobile:dev:local
  ```

---
## 4. Tech Stack

| Platform | Why We Chose It | Official Links |
|---|---|---|
| **Firebase Ecosystem** (Auth / Phone / FCM) | Provides out-of-the-box phone number one-tap authentication, zero-cost offline push notifications (FCM), and millisecond-level real-time data channels — significantly shortening development time for core mobile client flows. | [Firebase](https://firebase.google.com/) · [Cloud Messaging](https://firebase.google.com/docs/cloud-messaging) · [Authentication](https://firebase.google.com/docs/auth) |
| **Render** | Supports automatic build and containerized hosting triggered by Git commits, removing the need for complex server operations, and provides a minimal, stable deployment environment for Node.js backends and webhook services. | [Render](https://render.com/) |
| **MongoDB Atlas** | Naturally suited to the variable product attributes and complex, nested structures of listing cards and chat sessions in a second-hand marketplace; supports native geolocation (GeoJSON) proximity search and flexible schema expansion. | [MongoDB Atlas](https://www.mongodb.com/atlas) |
| **Resend** | A developer-first, next-generation email API with high deliverability. Lightweight and reliable, used to send registration, security verification codes, and key transaction receipt emails within seconds. | [Resend](https://resend.com/) |
| **Stripe (Stripe Connect)** | A mature, compliant escrow and split-payment system. Supports holding buyer payments in escrow, automatic fund release after offline QR-code verification, and fully automated refund and dispute-prevention mechanisms. | [Stripe](https://stripe.com/) · [Stripe Connect](https://stripe.com/connect) |

## 5. Document Map

As the project grows, our documentation is now spread across four places, each with a specific purpose:

**README (this page) — Navigation**
The entry point. No detailed content, just links and an overview: project summary, team, tech stack, quick start, and document links.
🔗 [README.md](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/blob/pre/README.md)

**`docs/` — Formal Project Documents (versioned, tracked with code)**
Finalized deliverables that need version control, living alongside the codebase:
- Product Spec
- Design Spec
- Test Spec
- Schemas

🔗 [docs/](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/tree/pre/docs)

**Wiki — Team Knowledge Base (process & history)**
Everything produced during team collaboration that we'll want to look up later:
- Meeting Minutes
- Decision Log
- Development Notes (environment setup, deployment, gotchas)

🔗 [Wiki](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/wiki)

**Google Docs — Collaborative Drafts (not yet finalized, frequently changing)**
Anything still being written or needing multi-person live editing:
- Report Drafts
- Presentation Drafts
- Brainstorms
- Anything the team wants to temporarily drop in for discussion

🔗 [Shared Drive Folder](https://drive.google.com/drive/folders/1HK6EkAZsz0FjwCu4Yby7qfp0N3rxHjRs?usp=sharing)

> **Flow:** README points the way → `docs/` shows the finished output → Wiki shows the process → Google Docs is where we edit together.

## 6. Group Members
* **Sam Yao** (byao937)
* **Xinru Cui** (xcui573)
* **Yixuan Sun** (ysun801)
* **Rui Wang** (rwan911)
* **Jenny Yin** (jyin852)

![Team image](./team-image.jpg)
