# LuxeHaven — Luxury Resort & Boutique Hotel Management System

LuxeHaven is a modern, full-stack resort reservation and hospitality property management application built using the MERN stack (**React 18 + Vite**, **Node.js**, **Express**, and **MongoDB with Mongoose**).

The system features robust role-based access control (RBAC), atomic overlapping reservation prevention, server-side pricing validation, automated session handling, and an intuitive administrative operations dashboard.

---

## 🏛️ System Architecture & Data Flow

```text
React 18 + Vite (Client)
   │ (HTTP / Axios with Bearer JWT interceptors)
   ▼
Express REST API (Server)
   │
   ├─► CORS & JSON Body Parsing
   ├─► Authentication Middleware (`verifyToken`)
   ├─► Authorization Guard (`requireAdmin`)
   ├─► Controllers (Auth, Rooms, Bookings, Admin)
   │
   ▼
Mongoose ODM (Models & Schemas)
   │
   ▼
MongoDB (Database with Compound Indexes)
```

---

## ✨ Core Features

### 🌟 Guest & Customer Experience
- **Elegant Landing Page**: Modern hero section with real-time date and room category search widget, featured suites preview, luxury amenities showcase, and guest testimonials.
- **Customer Registration & Authentication**: Secure client and server validation. Public registrations default to customer privilege (`userType: 'ordinary'`).
- **Suites & Accommodations Catalog**: Dynamic filtering by keyword, suite type (`Single`, `Double`, `Deluxe`, `Suite`, `Family`), price range, and calendar availability dates.
- **Detailed Suite View**: High-resolution gallery preview, luxury amenities list, booked date calendars, and interactive booking widget.
- **Server-Side Pricing Engine**: Booking duration (`totalDays`) and cost (`totalAmount`) are strictly calculated on the backend to prevent malicious price tampering.
- **My Reservations**: List of active and past bookings, check-in/out dates, pricing breakdown, and instant cancellation with real-time inventory release.
- **Personalized Guest Dashboard**: Welcome header, reservation overview, and profile details.

### 🛡️ Administrative Portal
- **Executive Operations Dashboard**: High-level KPI summary cards for Total Revenue, Confirmed Bookings, Room Inventory, and Registered Guests.
- **All Bookings Management**: Global reservation view with guest contact information, room details, status filtering, and administrative cancellation capabilities.
- **Live Inventory Control**: Suite directory with instant room availability toggling (`Available` / `Unavailable`) and safe deletion guards.
- **Suite Creation Form**: Streamlined form with room number uniqueness validation, category selection, nightly pricing, descriptions, features, and image URLs.
- **User Directory**: Comprehensive directory of registered customer and administrative accounts with creation timestamps and roles.

---

## 🔒 Security Audit & Best Practices Implemented

1. **Role Escalation Protection**: Public signup (`POST /api/auth/signup`) explicitly ignores any client-supplied `userType` and unconditionally assigns `'ordinary'`.
2. **Password Security**: Passwords are securely hashed using `bcryptjs` with salt factor 10. User schemas configure `password: { select: false }` so hashes are never exposed in API queries or responses.
3. **Session & JWT Security**: JSON Web Tokens are signed using `process.env.JWT_SECRET` with configurable expiration. Axios interceptors automatically attach the `Authorization: Bearer <token>` header and clear expired credentials upon 401 responses.
4. **Reservation Integrity**: Overlapping bookings for the same room during confirmed reservation dates are strictly prevented via MongoDB queries:
   `fromDate < newCheckOut AND toDate > newCheckIn`. Cancelled reservations immediately free inventory.
5. **Authorization Guards**: Customers can only inspect or cancel their own bookings. Administrative routes require both a valid token and `userType === 'admin'`.

---

## 🚀 Quick Start Guide

### Prerequisites
- **Node.js**: v18.0.0 or higher
- **MongoDB**: Either a local MongoDB instance (`mongodb://localhost:27017/luxehaven`), MongoDB Atlas connection string, or rely on in-memory mode.

---

### Step 1: Install Dependencies

```bash
# In the project root:
cd LuxeHaven

# Install backend dependencies
cd Server
npm install

# Install frontend dependencies
cd ../Client
npm install
```

---

### Step 2: Environment Configuration

In `Server/.env`:
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/luxehaven
JWT_SECRET=luxehaven_super_secret_jwt_key_2026_production
JWT_EXPIRES_IN=7d
CLIENT_URL=http://localhost:5173
NODE_ENV=development
```

---

### Step 3: Run the Application

#### Option A: Run Both Services Concurrently
From the project root:
```bash
# Start backend server
npm run server

# In a separate terminal, start frontend
npm run client
```

The application will be accessible at:
- **Frontend**: `http://localhost:5173`
- **Backend API**: `http://localhost:5000/api`

---

## 🔑 Demo Test Accounts

| Role | Email | Password | Permissions |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@luxehaven.com` | `AdminPassword123!` | Full Admin Portal, All Rooms, All Bookings, User Directory |
| **Guest Customer** | `customer@luxehaven.com` | `CustomerPass123!` | Room Reservations, Personal Dashboard, Booking History |

---

## 📄 License
ISC License © 2026 LuxeHaven Hospitality Group