# Technical Requirement Definition (FJH-75)

## 🏗️ Architecture Overview
The Flame Japanese Hibachi platform is built as a **Modular Monolith**. This ensures simplicity in deployment while maintaining a clean separation of concerns, allowing for future scalability into microservices if needed.

## 🛠️ Tech Stack
- **Frontend**: Next.js 14+ (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Backend/API**: Next.js API Routes (Serverless)
- **Database**: PostgreSQL 15+
- **ORM**: Prisma 5+
- **Auth**: NextAuth.js / JWT
- **Validation**: Zod

## 📡 Core Requirements
- **Multi-Location Support**: Data stores must be isolated by `ShopId`.
- **Real-time Order Tracking**: Using optimized database pooling for status updates.
- **DoorDash Integration**: Support for third-party delivery dispatch.
- **Role-Based Access Control (RBAC)**: Distinct permissions for Customers, Staff, and Admins.
- **Audit Logging**: Comprehensive tracking of all administrative actions.

## 🔗 Reference Documentation
For a deep dive into implementation paths and endpoint specs:
- 👉 **[Implementation Guide](../roadmap/DELIVERABLES_SUMMARY.md)**
- 👉 **[API Reference Documentation](../roadmap/README.md)** (Linked via Roadmap)

---
**Status:** Defined ✅
**Phase:** 1 (Foundation)
