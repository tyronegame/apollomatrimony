# Apollo Matrimony — apollomatrimony.com

![Next.js 16](https://img.shields.io/badge/Next.js-16.3-black?style=for-the-badge&logo=next.js)
![React 19](https://img.shields.io/badge/React-19.0-61DAFB?style=for-the-badge&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=for-the-badge&logo=typescript)
![Prisma 6](https://img.shields.io/badge/Prisma-6.19-2D3748?style=for-the-badge&logo=prisma)
![Tailwind CSS v4](https://img.shields.io/badge/Tailwind_CSS-v4.0-38B2AC?style=for-the-badge&logo=tailwind-css)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16.0-4169E1?style=for-the-badge&logo=postgresql)
![Docker Ready](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker)

**Apollo Matrimony** (`apollomatrimony.com`) is a premium, Tamil-first matrimonial matchmaking platform engineered specifically for Tamil families worldwide. It bridges traditional cultural heritage with a modern, high-performance digital experience.

---

## Key Features

- **Traditional Jathagam / Horoscope Engine**: Built-in 10-Porutham scoring engine based on Cooper sun longitude and Lahiri ayanamsa (`nakshatra`, `rasi`, `gothram`).
- **Multi-Role Dashboards**: Dedicated experiences for self-managed member profiles and parent-managed accounts with privacy controls.
- **Verification & Moderation Portal**: Multi-step photo verification, ID verification, and administrative audit logging (`AuditLog`).
- **Pluggable Payment Gateways**: Unified payment interface supporting **Razorpay**, **Stripe**, and local mock test flows.
- **Custom JWT Auth & Edge Security**: Secure dual-cookie JWT system (`am_at` access token + `am_rt` rotating refresh token) with proxy-level route security.
- **Robust Docker & Dokploy Ready**: Pre-configured standalone Docker production builds with automatic PostgreSQL database setup and migration scripts (`ensure-db.js`).

---

## Tech Stack

| Component | Technology / Library |
| --- | --- |
| **Framework** | Next.js 16 (App Router, Turbopack), React 19, TypeScript 5.9 |
| **Styling** | Tailwind CSS v4 (Custom Maroon, Gold, & Cream design system) |
| **Database & ORM** | PostgreSQL with Prisma 6 (44 relational models) |
| **Authentication** | Custom JWT + JOSE (`am_at`, `am_rt` cookies with rotation) |
| **Payment Gateways** | Razorpay, Stripe, Mock adapter (`PAYMENT_PROVIDER`) |
| **Content Engine** | `gray-matter` + Markdown parser for CMS, policies, and success stories |
| **Caching & Queue** | ioredis (optional) + SSE real-time stream with fallback polling |
| **Storage & Email** | Local storage / S3-compatible driver, SMTP / Nodemailer |

---

## Getting Started

### Prerequisites

- **Node.js**: `v20.9.0` or higher
- **PostgreSQL**: `v15` or higher (or Docker)
- **npm**: `v10` or higher

### Local Development Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/apollomatrimony/apollomatrimony.git
   cd apollomatrimony
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Configure Environment Variables**:
   Create a `.env` file in the root directory:
   ```dotenv
   # Core Application
   PORT=3000
   HOSTNAME="0.0.0.0"
   APP_URL="http://localhost:3000"

4. **Initialize Database & Seed Data**:
   ```bash
   # Run schema push to sync PostgreSQL tables
   npm run db:push

   # Seed admin account, communities, reference data, and demo profiles
   npm run db:seed
   ```

5. **Start Development Server**:
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000) in your browser.

---


---

## Available Scripts

| Command | Action |
| --- | --- |
| `npm run dev` | Starts local Next.js development server with hot reload |
| `npm run build` | Generates Prisma client and compiles production build |
| `npm start` | Runs the compiled Next.js production server |
| `npm run typecheck` | Validates TypeScript types (`tsc --noEmit`) |
| `npm run lint` / `lint:fix` | Runs ESLint 9 checks and automatic formatting fixes |
| `npm test` | Runs Vitest unit test suites |

---

## Production Deployment (Dokploy & Docker)

Apollo Matrimony is optimized for single-container standalone Docker deployment on platforms like **Dokploy**, **Coolify**, or **Kubernetes**.

### Deploying with Docker

1. **Build Docker Image**:
   ```bash
   docker build -t apollo-matrimony:latest .
   ```

2. **Run Container**:
   ```bash
   docker run -d \
     -p 3000:3000 \
     -e DATABASE_URL="postgresql://postgres:password@postgres-db:5432/apollo_matrimony?schema=public" \
     -e AUTH_SECRET="production-secret-key" \
     -e APP_URL="https://apollomatrimony.com" \
     --name apollo-matrimony-app \
     apollo-matrimony:latest
   ```

3. **Automatic Database Provisioning**:
   The Docker container includes an automated startup script (`prisma/ensure-db.js`) in `docker-entrypoint.sh` that automatically creates the `apollo_matrimony` database on the PostgreSQL server if it does not exist, and runs `prisma db push` prior to starting the web server.

---

```

---

## Security & Moderation

- **Data Privacy**: Contact details and phone numbers are hidden until mutual interest is accepted.
- **Audit Logging**: All administrative operations (verification approvals, account bans, content updates) are logged in the `AuditLog` table.

---

## License

Copyright © 2026 **Apollo Matrimony** (`apollomatrimony.com`). All rights reserved.
