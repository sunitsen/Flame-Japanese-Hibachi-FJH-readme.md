# 🗺️ Flame Japanese Hibachi Development Roadmap

This roadmap outlines the path from project initialization to the full production launch of the **Flame Japanese Hibachi** platform. It synthesizes the repository structure, data model, and implementation phases into a single actionable guide.

---

## 🏗️ Phase 1: Foundation & Repository Setup
**Goal:** Establish the technical skeleton and infrastructure.

1.  **Initialize Repository**
    *   Setup folder structure as defined in `GITHUB_STRUCTURE.md`.
    *   Configure `.gitignore`, `.env.example`, and `tsconfig.json`.
    *   Setup GitHub Actions for CI/CD (`.github/workflows/ci.yml`).
2.  **Database Layer**
    *   Copy `schema.prisma` to the `prisma/` directory.
    *   Run `npx prisma migrate dev --name init` to sync the database.
    *   Implement initial `seed.ts` with sample shops and menu items.
3.  **Core Dependencies**
    *   Install Next.js, Prisma, NextAuth.js, and Zod.

---

## 📋 Planning Phase: Data Model Definition (FJH-120)
**Goal:** Define and validate the system's data architecture.

### Subtasks:
- [x] **FJH-36:** Identify core entities (Users, Stores, Menu, Orders, Promotions, etc.)
- [x] **FJH-44:** Define entity relationships (high-level)
- [x] **FJH-83:** Define base schema structure (users, stores, menu, orders, etc.)
- [x] **FJH-84:** Define multi-store schema structure (store-specific vs global data)
- [x] **FJH-95:** Validate data model against functional requirements

> [!NOTE]
> Detailed technical documentation for these tasks can be found in [DATA_MODEL.md](./DATA_MODEL.md).

---

## 📊 Phase 2: Master Development Timeline
**Total Estimated Duration:** 15-16 weeks

| Phase | Duration | Focus Area | Status |
| :--- | :--- | :--- | :--- |
| **1** | 2 Weeks | **Authentication & Identity** (OAuth, Sessions, AuthAccount) | ⏳ Planned |
| **2** | 1 Week | **Shop Management** (Multi-location CRUD, Operating Hours) | ⏳ Planned |
| **3** | 1 Week | **Menu Management** (Items, Modifiers, Store Overrides) | ⏳ Planned |
| **4** | 1 Week | **Shopping Cart & Checkout** (Stateful Cart, Validation) | ⏳ Planned |
| **5** | 2 Weeks | **Order Lifecycle** (Processing, Fulfillment Status) | ⏳ Planned |
| **6** | 1 Week | **DoorDash Integration** (Dispatch records, API Sync) | ⏳ Planned |
| **7** | 1 Week | **Promotions & Coupons** (Scheduling, Eligibility) | ⏳ Planned |
| **8** | 1 Week | **Analytics & Reporting** (Daily Snapshots, Aggregation) | ⏳ Planned |
| **9** | 1 Week | **Access Control (RBAC)** (Permissions, Audit Logs) | ⏳ Planned |
| **10** | 2 Weeks | **Testing & Security** (Unit, Integration, Security Audit) | ⏳ Planned |
| **11** | 1 Week | **Deployment & Infrastructure** (Vercel/AWS Setup) | ⏳ Planned |
| **12** | 1 Week | **Beta & Final Launch** (Production Rollout) | ⏳ Planned |

---

## 📂 Expected Repository Structure
Following the `GITHUB_STRUCTURE.md` guide:

```
flame-japanese-hibachi/
├── 📄 README.md                # Main Entry Point
├── 📄 DATA_MODEL.md            # Schema Documentation
├── 📂 prisma/                  # Database Core
│   └── schema.prisma           # Single source of truth
├── 📂 src/                     # Application Source
│   ├── api/                    # Domain-driven API routes
│   ├── services/               # Reusable business logic
│   ├── components/             # React UI components
│   └── lib/                    # Shared utilities (Prisma, Auth)
├── 📂 docs/                    # Detailed References
│   ├── API_REFERENCE.md
│   └── IMPLEMENTATION_GUIDE.md
└── 📂 tests/                   # Test Suite
```

---

## 🎯 Key Deliverables & Success Metrics
As defined in `DELIVERABLES_SUMMARY.md`:

- **Production-Ready Schema:** Supporting 27 models across 8 domains.
- **REST API:** 40+ endpoints with full request/response specs.
- **Security Check:** All admin actions captured in `AuditLog`.
- **Scalability:** Pre-computed `AnalyticsSnapshot` for high-performance dashboards.

---

## 🚀 Getting Started (Developer Checklist)
- [ ] Clone repository and run `npm install`.
- [ ] Configure local `.env` with PostgreSQL URL.
- [ ] Run `npx prisma migrate dev`.
- [ ] Begin **Phase 1: Authentication** development.

---

**Last Updated:** April 2026 | **Version:** 0.1.0 Roadmap
