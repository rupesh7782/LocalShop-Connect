# LocalShop Connect 🛍️

> **Local Shops. Stronger Together.**  
> A production-ready Zepto-style local commerce platform built with Next.js 15, Express, Prisma, Google Places, Razorpay, Firebase, and Cloudinary.

---

## ✨ Features

| Area | What's included |
|---|---|
| **Discovery** | Live browser geolocation + Google Places API for real nearby shops |
| **Search** | City / locality / pincode search with reverse geocoding |
| **Shop Listing** | Filter by Open Now, Free Delivery, Rating, Category · Sort by Popular / Nearest / Rated · Infinite scroll |
| **Shop Detail** | Products, reviews, Google Maps embed, directions, related shops |
| **Cart** | Single-shop cart, persisted in localStorage, floating cart widget |
| **Checkout** | Address management, delivery instructions, coupon codes, Razorpay online payment, Cash on Delivery |
| **Order Tracking** | Animated 5-step real-time status tracker, 15-second polling |
| **Auth** | Google OAuth, Email + Password, JWT sessions via NextAuth |
| **Customer Dashboard** | Orders, Saved Shops, Addresses, Notifications, Profile |
| **Owner Dashboard** | Analytics (revenue, orders, customers, products), Product CRUD, Order management, Shop registration |
| **Admin Dashboard** | Platform analytics, User management + role change, Shop approval/reject/suspend, Category CRUD, Coupon management, Order overview |
| **Notifications** | Firebase Cloud Messaging push notifications (foreground + background) |
| **Images** | Cloudinary upload for shop banners, logos, product images |
| **PWA** | Installable, offline fallback, manifest |
| **Dark Mode** | System-aware + manual toggle, persisted across sessions |

---

## 🗂️ Project Structure

```
localshop-connect/
├── backend/                 Express + Prisma API
│   ├── prisma/
│   │   ├── schema.prisma    Complete DB schema (15 models)
│   │   └── seed.js          Dev seed: categories + demo shop
│   ├── src/
│   │   ├── app.js           Express app + middleware
│   │   ├── server.js        Server entry point
│   │   ├── config/          Config, Prisma client
│   │   ├── middleware/       Auth (JWT + RBAC), validation, error handling
│   │   ├── routes/          14 route files
│   │   ├── controllers/     Business logic
│   │   └── services/        Google Places, Razorpay, Cloudinary, Firebase
│   ├── Dockerfile
│   └── .env.example
│
├── frontend/                Next.js 15 App Router
│   ├── src/
│   │   ├── app/             All pages (App Router)
│   │   │   ├── page.tsx            Homepage
│   │   │   ├── shops/              Listing + Detail
│   │   │   ├── cart/               Cart page
│   │   │   ├── checkout/           Checkout + Razorpay
│   │   │   ├── categories/         Browse categories
│   │   │   ├── deals/              Deals page
│   │   │   ├── about/              About page
│   │   │   ├── contact/            Contact form
│   │   │   ├── auth/               Sign in + Register
│   │   │   └── dashboard/          Customer / Owner / Admin
│   │   ├── components/      Reusable UI components
│   │   ├── hooks/           React Query + custom hooks
│   │   ├── store/           Zustand (cart + location)
│   │   └── lib/             API client, types, utils, auth, Razorpay, Firebase
│   ├── public/
│   │   ├── manifest.json    PWA manifest
│   │   └── firebase-messaging-sw.js
│   ├── Dockerfile
│   └── .env.example
│
├── vercel.json              Vercel deployment config
├── railway.toml             Railway deployment config
├── docker-compose.yml       Local dev with Postgres
└── README.md
```

---

## 🚀 Quick Start (Local Development)

### Prerequisites

- Node.js ≥ 18
- PostgreSQL (or use Docker Compose below)
- API keys: Google Maps/Places, Razorpay, Cloudinary, Firebase (see below)

### 1. Clone & Install

```bash
git clone https://github.com/your-username/localshop-connect.git
cd localshop-connect

# Install backend dependencies
cd backend && npm install && cd ..

# Install frontend dependencies
cd frontend && npm install && cd ..
```

### 2. Set Up Environment Variables

**Backend:**
```bash
cp backend/.env.example backend/.env
# Fill in all values in backend/.env
```

**Frontend:**
```bash
cp frontend/.env.example frontend/.env.local
# Fill in all values in frontend/.env.local
```

### 3. Database Setup

```bash
cd backend

# Run migrations (creates all tables)
npx prisma migrate dev --name init

# Seed with categories + demo shop
npm run seed

# Open Prisma Studio to inspect data (optional)
npm run prisma:studio
```

### 4. Run Development Servers

**Terminal 1 — Backend:**
```bash
cd backend
npm run dev
# API running at http://localhost:5000
```

**Terminal 2 — Frontend:**
```bash
cd frontend
npm run dev
# App running at http://localhost:3000
```

### 5. (Optional) Use Docker Compose

```bash
# Starts Postgres + backend + frontend together
docker-compose up --build
```

---

## 🔑 API Keys Setup Guide

### Google Cloud Console
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project → Enable these APIs:
   - **Maps JavaScript API** (for map embeds)
   - **Places API** (for nearby shop discovery — core feature)
   - **Geocoding API** (for text → coordinates and reverse geocoding)
3. Create an API key → Restrict it to your domain
4. Create OAuth 2.0 credentials → Add `http://localhost:3000/api/auth/callback/google` to Authorized redirect URIs

### Razorpay
1. Sign up at [razorpay.com](https://razorpay.com)
2. Dashboard → Settings → API Keys → Generate Key
3. Copy `Key ID` → `NEXT_PUBLIC_RAZORPAY_KEY_ID` and `RAZORPAY_KEY_ID`
4. Copy `Key Secret` → `RAZORPAY_KEY_SECRET` (never expose this client-side)

### Cloudinary
1. Sign up at [cloudinary.com](https://cloudinary.com)
2. Dashboard → copy Cloud Name, API Key, API Secret

### Firebase
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Create project → Add Web App → copy config values
3. Project Settings → Cloud Messaging → Generate Web Push certificate (VAPID key)
4. Generate Service Account key → download JSON → extract `project_id`, `client_email`, `private_key` for backend

### Neon (Production Postgres)
1. Sign up at [neon.tech](https://neon.tech)
2. Create database → copy Connection String → `DATABASE_URL`

---

## 🚢 Deployment

### Frontend → Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# From project root
vercel

# Set environment variables in Vercel dashboard
# (or use vercel.json env references with vercel env add)
```

### Backend → Railway

```bash
# Install Railway CLI
npm i -g @railway/cli

railway login
railway init

# Deploy from /backend
cd backend
railway up

# Add environment variables in Railway dashboard
# The railway.toml handles build + start commands automatically
```

### Database → Neon

1. Create a Neon database at [neon.tech](https://neon.tech)
2. Copy the connection string to `DATABASE_URL` in Railway environment
3. Railway's deploy command runs `prisma migrate deploy` automatically

---

## 📡 API Reference

All endpoints are prefixed with `/api`.

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/auth/register` | — | Email/password registration |
| POST | `/auth/login` | — | Email/password login → JWT |
| POST | `/auth/google` | — | Google sign-in upsert → JWT |
| GET | `/auth/me` | JWT | Current user |
| GET | `/location/search?q=Nagpur` | — | Text → coordinates |
| GET | `/location/reverse?lat=&lng=` | — | Coordinates → address |
| GET | `/location/nearby-shops?lat=&lng=&radius=&category=` | — | **Live** nearby shops (DB + Google Places) |
| GET | `/shops` | — | Paginated shop listing with filters |
| GET | `/shops/:slug` | — | Shop detail + products + reviews |
| POST | `/shops` | OWNER | Register new shop |
| PATCH | `/shops/:id` | OWNER | Update shop |
| POST | `/shops/:id/favorite` | CUSTOMER | Toggle favorite |
| GET | `/products?shopId=` | — | Products for a shop |
| POST | `/products` | OWNER | Add product |
| PATCH | `/products/:id` | OWNER | Update product / toggle availability |
| DELETE | `/products/:id` | OWNER | Delete product |
| GET | `/categories` | — | All categories |
| POST | `/categories` | ADMIN | Create category |
| POST | `/orders` | CUSTOMER | Place order (Razorpay or COD) |
| POST | `/orders/:id/verify-payment` | CUSTOMER | Verify Razorpay signature |
| GET | `/orders` | JWT | List orders (role-filtered) |
| GET | `/orders/:id` | JWT | Order detail + status history |
| PATCH | `/orders/:id/status` | OWNER/ADMIN | Update order status |
| GET | `/reviews/shop/:shopId` | — | Reviews for a shop |
| POST | `/reviews` | CUSTOMER | Submit review (requires delivered order) |
| GET/POST/PATCH/DELETE | `/addresses` | CUSTOMER | Address book CRUD |
| GET | `/favorites` | CUSTOMER | Saved shops |
| GET/PATCH | `/notifications` | JWT | Notifications, mark read |
| GET/POST/PATCH/DELETE | `/coupons` | ADMIN | Coupon management |
| GET | `/coupons/validate/:code` | JWT | Validate coupon at checkout |
| POST | `/upload?folder=` | JWT | Cloudinary image upload |
| GET | `/owner/shops` | OWNER | Owner's shops |
| GET | `/owner/analytics?shopId=` | OWNER | Revenue, orders, customers chart |
| GET | `/admin/users` | ADMIN | All users |
| PATCH | `/admin/users/:id/role` | ADMIN | Change role |
| GET | `/admin/shops` | ADMIN | All shops (filterable by status) |
| PATCH | `/admin/shops/:id/approve` | ADMIN | Approve shop |
| PATCH | `/admin/shops/:id/reject` | ADMIN | Reject shop |
| PATCH | `/admin/shops/:id/suspend` | ADMIN | Suspend shop |
| GET | `/admin/analytics` | ADMIN | Platform-wide stats |

---

## 🔐 Demo Credentials (after seeding)

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@localshopconnect.demo | Admin@12345 |
| Owner | owner@localshopconnect.demo | Owner@12345 |

Register as a customer through the normal sign-up flow.

---

## 🏗️ Tech Stack

**Frontend:** Next.js 15, TypeScript, Tailwind CSS, shadcn/ui, Framer Motion, Lucide Icons, React Query, Zustand, NextAuth  
**Backend:** Node.js, Express.js, Prisma ORM, PostgreSQL  
**Third-party:** Google Maps + Places API, Razorpay, Cloudinary, Firebase Cloud Messaging  
**Database:** Neon (Production), PostgreSQL (Local)  
**Deployment:** Vercel (Frontend), Railway (Backend), Neon (Database)  

---

## 📱 PWA Support

The app is installable as a Progressive Web App:
- Add to Home Screen on mobile browsers
- Offline fallback page
- Background push notifications via Firebase

---

## 🧪 Seed Data vs Production Data

- **Development:** `npm run seed` (backend) seeds 8 categories and 1 demo shop in Nagpur.
- **Production:** The "Near Me" and location search features call **live Google Places API** to return real businesses near the user. No dummy data is shown in production.

---

## 📄 License

MIT — free to use, modify, and deploy.

---

Built with ❤️ for local commerce.
