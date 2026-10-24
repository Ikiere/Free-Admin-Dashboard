# Free Admin Dashboard & Safe Supabase API Gateway

A full-stack, enterprise-grade eCommerce Admin Dashboard and secure Backend API Gateway built with **React 18**, **TypeScript**, **Tailwind CSS**, and **Node.js/Express**, featuring **zero-leakage safe Supabase database integration**.

---

## ⚡ What Makes This Safe for Supabase?

Unlike traditional frontend-only templates that expose Supabase connection keys or database credentials in client-side JavaScript bundles, **this architecture introduces a zero-leakage security boundary**:

1. **Backend Vault Encryption (AES-256-GCM)**: Sensitive credentials (including the Supabase `service_role` key and direct connection strings) are transmitted only to the secure backend and encrypted using AES-256-GCM with PBKDF2 key derivation.
2. **Server-Side API Proxying**: The browser only communicates with `/api/products`, `/api/categories`, `/api/orders`, `/api/supabase/...`. No client ever accesses the Supabase administrative keys directly.
3. **Dual Storage Engine**:
   - **Cloud Supabase**: When credentials are connected, queries are executed server-side against your Supabase PostgreSQL instance.
   - **Local Resilient Vault**: When Supabase is not connected or during offline local development, the backend automatically falls back to a resilient, seeded local store, ensuring the dashboard never crashes.
4. **Automated Schema Provisioning**: View, copy, and run the complete SQL migration directly in your Supabase SQL Editor with full Row-Level Security (RLS) policies and performance indexes.

---

## 🏗️ Project Structure

```
Free-Admin-Dashboard/
├── backend/                        # Node.js + Express + TypeScript Backend
│   ├── src/
│   │   ├── config/env.ts          # Environment variables & master vault key
│   │   ├── controllers/           # Products, Categories, Orders, Users, Supabase, Auth, Stats
│   │   ├── data/
│   │   │   ├── initialSeed.ts     # Rich eCommerce catalog & user seeds
│   │   │   └── supabase-schema.sql# PostgreSQL DDL with RLS policies & indexes
│   │   ├── middleware/            # JWT auth, error handlers, rate limiting
│   │   ├── routes/                # REST API routers (/api/...)
│   │   ├── services/
│   │   │   ├── auth.service.ts    # JWT & bcrypt authentication
│   │   │   ├── repository.service.ts # Dual-mode (Supabase / local store)
│   │   │   ├── supabase.service.ts   # Connection manager & table health checker
│   │   │   └── vault.service.ts   # AES-256-GCM cryptographic vault
│   │   └── index.ts               # Express server entrypoint
│   ├── .env.example
│   ├── package.json
│   └── tsconfig.json
│
├── src/                           # React 18 Admin Dashboard UI
│   ├── components/                # Modular UI components, Chart.js & Recharts
│   ├── pages/
│   │   ├── SupabaseSettings.tsx   # Dedicated Safe Supabase Management Console
│   │   ├── Products.tsx, CreateProduct.tsx, EditProduct.tsx
│   │   ├── Categories.tsx, CreateCategory.tsx
│   │   ├── Orders.tsx, CreateOrder.tsx
│   │   ├── Users.tsx, CreateUser.tsx
│   │   ├── Reviews.tsx, CreateReview.tsx
│   │   ├── Landing.tsx, LandingV2.tsx
│   │   └── Login.tsx, Register.tsx
│   ├── services/
│   │   └── api.ts                 # Type-safe API client with JWT interceptor
│   └── App.tsx                    # React Router configuration
├── package.json
├── tailwind.config.js
└── vite.config.ts                 # Configured with proxy to backend /api
```

---

## 🚀 Quick Start Guide

### 1. Install Dependencies

```bash
# Install frontend dependencies
npm install

# Install backend dependencies
cd backend && npm install && cd ..
```

### 2. Start the Backend API Server

```bash
cd backend
npm run dev
```

The backend starts at `http://localhost:5000` with live health check at `http://localhost:5000/api/health`.

### 3. Start the Admin Dashboard UI

In a separate terminal:

```bash
npm run dev
```

Open `http://localhost:3001` (or `http://localhost:5173`) in your browser.

---

## 🔒 Connecting Your Supabase Project Safely

1. Log into your Supabase Dashboard: [https://supabase.com/dashboard](https://supabase.com/dashboard).
2. Go to **Project Settings &rarr; API** and copy:
   - **Project URL** (`https://<project-ref>.supabase.co`)
   - **anon / public key**
   - **service_role key**
3. In the Admin Dashboard, navigate to **Supabase DB** in the sidebar.
4. Paste your credentials and click **Test Connection**.
5. Click **Save & Connect Supabase**. Credentials are encrypted immediately via AES-256-GCM.
6. Copy the SQL script provided on the page and paste it into **Supabase SQL Editor &rarr; Run** to create tables, indexes, and RLS policies.

---

## 🛡️ Default Development Credentials

- **Admin Email**: `admin@accessglobal.io`
- **Password**: `AdminPass123!`

---

## 📜 License

MIT License - Open Source.
