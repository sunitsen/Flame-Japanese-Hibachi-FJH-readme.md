# 🗺️ Flame Japanese Hibachi Project Blueprint

This document provides the high-level technical strategy and logical architecture of the **Flame Japanese Hibachi** platform. It is designed to be a developer-friendly guide to understanding how the system is built and how work is structured.

---

## 🛠️ Master Tech Stack
The project leverages a modern, typesafe, and high-performance stack:

*   **Framework**: [Next.js](https://nextjs.org/) (App Router & Server Components)
*   **Language**: [TypeScript](https://www.typescriptlang.org/) (Strict Mode)
*   **Database & ORM**: [PostgreSQL](https://www.postgresql.org/) + [Prisma ORM](https://www.prisma.io/)
*   **Authentication**: [NextAuth.js](https://next-auth.js.org/) (JWT Strategy)
*   **State Management**: [Zustand](https://github.com/pmndrs/zustand)
*   **Styling**: Vanilla CSS / Tailwind (Responsive Design)
*   **Integrations**: 
    *   **DoorDash Drive API**: Logistics & Delivery
    *   **Stripe**: Payment Processing
    *   **Resend**: Email Communication
    *   **Google Maps**: Geolocation & Store Selection

---

## 🏗️ Project Architecture & Modules

Instead of strict calendar deadlines, the project is organized into **Logical Layers**. Each module listed below has its own dedicated documentation folder in the root directory.

### Layer 1: Core Foundation (COMPLETED)
*   **FJH-75: [Technical Requirement Definition](../Technical%20Requirement%20Definition/README.md)**: Universal system constraints.
*   **FJH-92: [Data Model Definition](../Data%20Model%20Definition/README.md)**: The Prisma schema and relationship maps.

### Layer 2: Identity & Store Management (ACTIVE)
*   **FJH-93: [Multi-Store System Design](../Multi-Store%20System%20Design/README.md)**: Global vs Store-level inheritance.
*   **FJH-94: [Role & CMS System Design](../Role%20&%20CMS%20System%20Design/README.md)**: RBAC Matrix and Dynamic Content slots.
*   **FJH-96: [Authentication & Session Design](../Authentication%20&%20Session%20Design/README.md)**: NextAuth configuration and guest sessions.
*   **FJH-107: [External Services & Communication](../External%20Services%20&%20Communication%20Design/README.md)**: Email notifications (Resend) and Google Maps.
*   **FJH-113: [Business Logic – Promotions & Catering](../Business%20Logic%20–%20Promotions%20&%20Catering/README.md)**: Promotion engine and bulk order logic.

### Layer 3: Commerce & Ordering (PLANNED)
*   **FJH-102: [Payment & Transaction Flow](../Payment%20&%20Transaction%20Flow%20Design/README.md)**: Checkout logic, Stripe integration, and DoorDash handoffs.
*   **Menu Engine**: Universal items with location-based overrides.
*   **Stateful Cart**: Client-side persistence and modifier logic.

---

## 🍱 Deliverables Manifest
- **[DATA_MODEL.md](./DATA_MODEL.md):** The authoritative database reference.
- **[GITHUB_STRUCTURE.md](./GITHUB_STRUCTURE.md):** Repository organization guide.

---

**Last Updated:** 2026-04-20 | **Version:** 1.1.0 Blueprint (No-Time-Tracking Mode)
