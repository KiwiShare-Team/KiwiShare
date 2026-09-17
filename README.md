# KiwiShare

**Team Five Guys · COMPSCI 734 · University of Auckland**

KiwiShare is a cross-platform second-hand marketplace built for COMPSCI 734.

The project is organised as a monorepo containing:

- **Flutter client** — mobile and browser-based Flutter application
- **React web client** — separate Vite/React web application
- **Koa backend** — Node.js + TypeScript REST API
- **MongoDB Atlas** — application data storage
- **Firebase** — authentication and supporting mobile services
- **Render** — deployed backend environment

---

## 🎯 Coursework Evaluation Quick Links

- **🚀 Assessor 3-Minute Live Demo Guide**: [`docs/demo-guide.md`](docs/demo-guide.md)
- **📊 Interactive Presentation Slide Deck (HTML)**: [`docs/presentation.html`](docs/presentation.html)
- **🌐 Production Web Portal**: [https://kiwishare.online](https://kiwishare.online)
- **☁️ Production Backend API**: [https://kiwishare.onrender.com](https://kiwishare.onrender.com)
- **📱 Android APK Releases**: [GitHub Releases](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/releases)
- **🏛️ System Architecture & Native Integrations**: [`docs/architecture.md`](docs/architecture.md)
- **🔒 Cybersecurity & OWASP Top 10 Blueprint**: [`docs/security-owasp.md`](docs/security-owasp.md)

---

## 📋 COMPSCI 734 Rubric Alignment Matrix

| Evaluation Criteria | KiwiShare Implementation Evidence & Deliverables | Core Documentation |
| :--- | :--- | :--- |
| **Integrated Solution** *(Mobile + Web + Cloud)* | **Flutter App** (buyer/seller mobile UX), **React/Vite Portal** (administration & metrics), **Koa Node.js REST API**, **MongoDB Atlas** (ReplicaSet cluster). | [`docs/architecture.md`](docs/architecture.md)<br>[`docs/demo-guide.md`](docs/demo-guide.md) |
| **Technical Mastery** *(Flutter & Cloud Tools)* | Declarative GoRouter routing, Provider state management, MongoDB 2dsphere geo-indexing, Cloudflare R2 S3-presigned URLs, atomic sessions. | [`docs/architecture.md`](docs/architecture.md)<br>[`mobile/lib/`](mobile/lib/) |
| **Independent Learning** *(Beyond Course Material)* | **Voice audio recording/playback & FFmpeg transcoding**; **Cloudflare R2 edge storage**; **AI-powered listing generation via Gemini**; **Vector OpenStreetMap**; **Crypto QR handover claims**. | *See "Beyond Course Material" below* |
| **Genuine User Need** *(Second-hand trust)* | Addresses circular economy barriers in Aotearoa: verified student badges, user Trust Scores (0-100), and anti-ghosting in-person handover verification. | [`docs/product-requirements/`](docs/product-requirements/) |
| **Modern Mobile Capabilities** | **Camera** (`image_picker`), **QR Scanner** (`mobile_scanner`), **Microphone/Audio** (`record` + `just_audio`), **GPS/Geocoding** (`geolocator`), **Interactive Maps** (`flutter_map`), **Push Notifications** (`firebase_messaging`). | [`docs/architecture.md#3`](docs/architecture.md) |
| **Software Architecture** | Decoupled RESTful layer, Mongoose schema modeling, MongoDB transactional isolation (`runMongoTransaction`), unified API error filter. | [`docs/architecture.md`](docs/architecture.md)<br>[`docs/api-spec.md`](docs/api-spec.md) |
| **Software Engineering Practices** | PNPM Monorepo, strict TypeScript typing, automated Lint/Formatting, Husky git hooks, Conventional Commits + Semantic Versioning. | [`.github/workflows/`](.github/workflows/) |
| **Rigorous Testing Strategy** | 12 backend test suites, multi-layered Flutter unit/widget/integration test suites, detailed subsystem verification reports. | [`docs/testing-strategy/`](docs/testing-strategy/) |
| **Cybersecurity & Privacy** | JWT authentication, bcrypt salt rounds 12, rate limiting, removal of technology banners, MongoDB TTL auto-expiring QR tokens, chat sanitization. | [`docs/security-owasp.md`](docs/security-owasp.md) |
| **Project Management** | Formal Git Flow (`main`, `pre`, `feature/*`, `hotfix/*`), automated PR assignments, release notes generation, continuous integration. | [`docs/git-flow.md`](docs/git-flow.md) |
| **Realistic Deployment** | Fully live in production: custom domain with SSL (`kiwishare.online`), Render cloud hosting, MongoDB Atlas cluster, GitHub Releases APKs. | [`docs/deployment.md`](docs/deployment.md) |
| **Communication & Rationale** | Comprehensive markdown blueprints covering schemas, OWASP controls, UI design specifications, and release operations. | [`docs/`](docs/) |

---

## 🚀 Independent Learning: Technologies Applied Beyond Course Scope

To elevate KiwiShare beyond standard coursework expectations, the team researched, benchmarked, and implemented several advanced technologies:

1. **Hardware Voice Messaging & Server-Side Audio Transcoding**:
   - Integrated hardware microphone capture (`record`) and real-time audio playback (`just_audio`) within chat threads.
   - Deployed a statically bundled `ffmpeg-static` worker on the Koa backend to validate, clean, and transcode raw client voice memos before distribution.
2. **High-Throughput Cloudflare R2 S3-Presigned Architecture**:
   - Rather than streaming large binary photos through the Node.js API process (which risks memory bloat), the backend generates time-limited S3 presigned PUT URLs, enabling clients to upload media directly to edge storage.
3. **Cryptographic In-Person QR Handover Protocol**:
   - Designed an in-person exchange system using cryptographically hashed single-use tokens (`qr_flutter` display + `mobile_scanner` real-time camera recognition) executed in atomic MongoDB sessions to eliminate ghosting and delivery disputes.
4. **Server-Side AI Listing Assistant (Google Gemini)**:
   - Integrated Google Gemini (`gemini-2.5-flash-lite`) via backend service APIs to generate intelligent descriptions, condition tags, and sustainability badges from raw user titles.
5. **Vector OpenStreetMap Native Mapping**:
   - Implemented `flutter_map` and `latlong2` vector tiling as a privacy-friendly, open-source alternative to proprietary map SDKs.
6. **Automated Monorepo Release & Versioning Pipeline**:
   - Built a custom GitHub Actions CD pipeline utilizing `standard-version` and Conventional Commits to automatically calculate semantic versions, publish changelogs, tag Git commits, and build cross-platform binaries on merges to `main`.

---

## Repository Structure

```text
project-implementation-five-guys/
├── .github/
│   └── workflows/              # CI/CD workflows
│
├── docs/                       # Architecture, API and project documentation
│   ├── architecture.md
│   ├── api-spec.md
│   ├── security-owasp.md
│   └── schemas/
│
├── mobile/                     # Flutter application
│   ├── lib/
│   ├── test/
│   └── pubspec.yaml
│
├── server/                     # Koa + TypeScript backend
│   ├── src/
│   ├── test/
│   ├── .env.example
│   └── package.json
│
├── web/                        # React + Vite web application
│   ├── src/
│   └── package.json
│
├── scripts/
│   └── mobile-dev.mjs          # Cross-platform Flutter launcher
│
├── package.json                # Root development commands
├── pnpm-workspace.yaml
└── README.md
```

---

# Quick Start

## 1. Prerequisites

All developers need:

- **Node.js 20+**
- **pnpm**
- **Flutter**
- **Git**

Check your environment:

```bash
node --version
pnpm --version
flutter --version
git --version
```

### Platform-specific requirements

| Target | Requirements |
|---|---|
| Flutter Chrome | Flutter + Chrome |
| Android emulator/device | Android SDK / Android Studio |
| iOS Simulator | macOS + Xcode |
| Physical iPhone | macOS + Xcode + configured iPhone |
| React Web | Node.js + pnpm |
| Backend | Node.js + MongoDB credentials |

You do **not** need every platform installed to work on the project.

For example, a developer working through Flutter Chrome only needs Flutter and Chrome.

---

# Initial Setup

Clone the repository:

```bash
git clone https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys.git
cd project-implementation-five-guys
```

Install Node and Flutter dependencies:

```bash
pnpm bootstrap
```

This installs the monorepo dependencies and runs:

```bash
flutter pub get
```

for the Flutter client.

---

# Backend Environment

Create the backend environment file:

```bash
cp server/.env.example server/.env
```

Then configure `server/.env`.

Example:

```env
NODE_ENV=development
PORT=3000

MONGODB_URI=mongodb+srv://<username>:<password>@<cluster>/kiwishare_prod
MONGODB_URI_TEST=mongodb+srv://<username>:<password>@<cluster>/kiwishare_test

JWT_SECRET=<your-secret>

RESEND_API_KEY=<optional>
RESEND_FROM=<optional>

# Optional AI listing suggestions (server-side only)
GEMINI_API_KEY=<optional>
GEMINI_MODEL=gemini-2.5-flash-lite
```

> Never commit `server/.env` or credentials to Git.

During development, the backend uses the configured development/test MongoDB database.

Run the backend alone:

```bash
pnpm server:dev
```

The local API is available at:

```text
http://localhost:3000
```

Example endpoint:

```text
http://localhost:3000/api/usedItems
```

---

# Development Commands

The root commands are designed around two independent concepts:

1. **Which frontend target to run**
2. **Whether it connects to the local or remote backend**

For normal development, use the high-level `pnpm dev:*` commands below.

## Recommended Commands

| Command | Starts | Backend |
|---|---|---|
| `pnpm dev` | Flutter Chrome + Koa | Local |
| `pnpm dev:android` | Flutter Android + Koa | Local |
| `pnpm dev:ios:sim` | Flutter iOS Simulator + Koa | Local |
| `pnpm dev:ios` | Flutter physical iPhone + Koa | Local |
| `pnpm dev:web` | React/Vite Web + Koa | Local |
| `pnpm dev:all` | Flutter Chrome + React Web + Koa | Local |
| `pnpm dev:remote` | Flutter Chrome only | Render |

---

## Default Development

For most Flutter development:

```bash
pnpm dev
```

This starts:

```text
Flutter Chrome
      │
      ▼
http://localhost:3000
      │
      ▼
Koa Backend
      │
      ▼
MongoDB Atlas
```

This is the recommended setup for fast UI, state-management and API development.

---

## Android Development

Start an Android emulator or connect an Android device first.

Then run:

```bash
pnpm dev:android
```

This starts:

```text
Flutter Android
      │
      ▼
Local Koa Backend
      │
      ▼
MongoDB Atlas
```

The development launcher automatically detects available Android devices.

If multiple matching devices are connected, specify one explicitly:

```bash
pnpm mobile android local <device-id>
```

Example:

```bash
pnpm mobile android local emulator-5554
```

---

## iOS Simulator

Requires **macOS + Xcode**.

Start an iOS Simulator, then:

```bash
pnpm dev:ios:sim
```

The launcher automatically detects the running simulator.

---

## Physical iPhone

Requires **macOS + Xcode** and an iPhone configured for Flutter development.

Run:

```bash
pnpm dev:ios
```

For a physical phone, `localhost` would refer to the phone itself rather than the development computer.

The development launcher therefore detects the host machine's LAN address and passes the appropriate local API URL to Flutter.

If automatic detection is unsuitable for your network:

```bash
LOCAL_API_HOST=192.168.1.10 pnpm dev:ios
```

Replace the address with the LAN IP of your development computer.

The computer and phone must be reachable on the same network.

---

# React Web Application

`web/` is a separate React + Vite application.

Run it with the local backend:

```bash
pnpm dev:web
```

This starts:

```text
React / Vite
     │
     ▼
Local Koa Backend
```

Run the React frontend by itself:

```bash
pnpm web:dev
```

---

# Run Everything Locally

To run the primary local development stack:

```bash
pnpm dev:all
```

This starts:

```text
                    ┌── Flutter Chrome
                    │
Local Koa Backend ──┼── React / Vite Web
                    │
                    └── MongoDB Atlas
```

Android and iOS targets are intentionally not included in `dev:all`.

They can be launched separately when native-platform testing is required.

---

# Local Frontend + Remote Backend

The deployed backend is:

```text
https://kiwishare.onrender.com
```

To run Flutter Chrome locally against the deployed backend:

```bash
pnpm dev:remote
```

You can also combine any supported Flutter target with the remote backend:

```bash
pnpm mobile android remote
pnpm mobile ios-sim remote
pnpm mobile ios-device remote
pnpm mobile chrome remote
```

This is useful for distinguishing frontend issues from local-backend issues.

---

# Flutter Development Launcher

Flutter targets are managed by:

```text
scripts/mobile-dev.mjs
```

General syntax:

```bash
pnpm mobile <target> <backend> [device-id]
```

Supported targets:

```text
chrome
android
ios-sim
ios-device
```

Supported backends:

```text
local
remote
```

Examples:

```bash
pnpm mobile chrome local
pnpm mobile chrome remote

pnpm mobile android local
pnpm mobile android remote

pnpm mobile ios-sim local
pnpm mobile ios-device local
```

Normally, team members should use the simpler `pnpm dev:*` commands instead.

The lower-level `pnpm mobile` command is mainly useful for unusual target/backend combinations.

---

# API Resolution

The Flutter client supports both local and deployed backend environments.

When using:

```text
API_BASE_URL=local
```

the client resolves the correct local backend address depending on its platform:

| Flutter target | Local backend |
|---|---|
| Chrome | `http://localhost:3000` |
| Android Emulator | `http://10.0.2.2:3000` |
| iOS Simulator | `http://localhost:3000` |
| Physical device | Host LAN IP |

For remote development, the client connects to:

```text
https://kiwishare.onrender.com
```

---

# Testing

Run all automated tests:

```bash
pnpm test:all
```

Or individually:

### Flutter

```bash
pnpm mobile:test
```

### Backend

```bash
pnpm server:test
```

---

# Static Analysis

Run all project checks:

```bash
pnpm lint:all
```

Individual checks:

```bash
pnpm mobile:analyze
pnpm server:lint
pnpm web:lint
```

---

# Formatting

Format Flutter/Dart source:

```bash
pnpm format:all
```

---

# Production Builds

## Android APK

```bash
pnpm mobile:build:android
```

## iOS

Requires macOS + Xcode:

```bash
pnpm mobile:build:ios
```

## React Web

```bash
pnpm web:build
```

## Backend

```bash
pnpm server:build
```

## Full Build

```bash
pnpm build:all
```

> Platform-specific builds still require the appropriate platform toolchain.

---

# Tech Stack

| Layer | Technology |
|---|---|
| Mobile / Cross-platform UI | Flutter / Dart |
| Web Application | React / TypeScript / Vite |
| Backend | Node.js / Koa / TypeScript |
| Database | MongoDB Atlas |
| Authentication / Mobile Services | Firebase |
| Email | Resend |
| Object Storage | Cloudflare R2 |
| Deployment | Render |
| Package Management | pnpm |
| Source Control / CI | GitHub / GitHub Actions |

---

# Documentation

Project documentation is kept in `docs/`.

### Architecture

[System Architecture](./docs/architecture.md)

### API

[API Specification](./docs/api-spec.md)

### Security

[OWASP Security Notes](./docs/security-owasp.md)

### Data Contracts

[JSON Schemas](./docs/schemas/)

---

# Documentation Workflow

We use different locations for different kinds of information.

### README

The starting point for developers.

Contains:

- project overview
- environment setup
- development commands
- repository navigation

### `docs/`

Version-controlled project documentation.

Contains finalized technical documentation such as:

- architecture
- API specification
- security design
- schemas
- project specifications

### GitHub Wiki

Team process and knowledge base.

Use it for:

- meeting minutes
- decision logs
- development notes
- environment gotchas
- implementation history

[KiwiShare Wiki](https://github.com/UOA-CS734-S2-2026/project-implementation-five-guys/wiki)

### Google Drive

Used for actively edited collaborative documents such as:

- reports
- presentation drafts
- brainstorming
- working documents

[Shared Google Drive](https://drive.google.com/drive/folders/1HK6EkAZsz0FjwCu4Yby7qfp0N3rxHjRs?usp=sharing)

The intended workflow is:

```text
Google Docs
    ↓
collaborative drafting

Wiki
    ↓
team knowledge and decisions

docs/
    ↓
final version-controlled technical documents

README
    ↓
navigation and developer onboarding
```

---

# Git Workflow

Before starting work:

```bash
git pull
pnpm bootstrap
```

Create or switch to the appropriate feature branch.

Before opening a pull request:

```bash
pnpm lint:all
pnpm test:all
```

Do not commit:

- `.env`
- credentials
- API secrets
- local SDK paths
- IDE-specific machine configuration

Development tooling should remain portable across team members wherever possible.

---

# Troubleshooting

## Flutter says more than one device is connected

Check:

```bash
flutter devices
```

Then either use one of the target-specific development commands or specify a device:

```bash
pnpm mobile android local <device-id>
```

---

## No Android device found

Make sure an Android emulator is running or a physical Android device is connected.

Then:

```bash
flutter devices
```

---

## iOS Simulator is unavailable

iOS Simulator development requires:

```text
macOS
+
Xcode
+
Flutter iOS toolchain
```

It is not available on Windows or Linux.

---

## Backend cannot connect to MongoDB

Confirm:

```text
server/.env
```

exists and contains valid:

```env
MONGODB_URI=
MONGODB_URI_TEST=
```

If the application attempts to connect to:

```text
127.0.0.1:27017
```

the MongoDB environment configuration has probably not been loaded.

---

## Flutter cannot find Chrome

Check:

```bash
flutter devices
flutter doctor -v
```

Chrome should appear as:

```text
Chrome (web) • chrome
```

---

# Team

**Five Guys — COMPSCI 734**

- Sam Yao — `byao937`
- Xinru Cui — `xcui573`
- Yixuan Sun — `ysun801`
- Rui Wang — `rwan911`
- Jenny Yin — `jyin852`

![Team Five Guys](./team-image.jpg)

---

## KiwiShare

Building a safer and more convenient second-hand marketplace across mobile and web.
