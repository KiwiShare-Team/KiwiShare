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

## 4. Group Members
* **Sam Yao** (byao937)
* **Xinru Cui** (xcui573)
* **Yixuan Sun** (ysun801)
* **Rui Wang** (rwan911)
* **Jenny Yin** (jyin852)

![Team image](./team-image.jpg)
