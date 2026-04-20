# 🗺️ Flame Japanese Hibachi Development Roadmap

This roadmap outlines the path from project initialization to the full production launch. It serves as the master planning document for the **Flame Japanese Hibachi** platform.

**Quick Links:** [📊 Current Status](../STATUS.md) | [🚀 Release Notes](./RELEASE_NOTES.md) | [📈 Data Model](./DATA_MODEL.md)

---

## 🏗️ Phase 1: Foundation & Repository Setup (COMPLETE)
**Goal:** Establish the technical skeleton and infrastructure.

1.  **Initialize Repository** ✅
    *   Setup folder structure and standardized README.
    *   Configure basic CI/CD templates and project metadata.
2.  **Standards & Documentation** ✅
    *   Define `.env.example`, PR templates, and Issue templates.
    *   Establish technical requirement definitions.
3.  **Database Design** ✅
    *   Finalize Prisma schema with 27 core models.
    *   Define relationship constraints and indexing strategy.

---

## 📊 Phase 2: Core Identity & Store Management (ACTIVE)
**Goal:** Authenticate users and define the physical restaurant locations.

### Tasks:
- [ ] **FJH-120: [Multi-Store System Design](../Multi-Store%20System%20Design/README.md)** (In Progress)
    *   Define global vs store-level data overrides.
    *   Implement store selection logic.
- [ ] **FJH-94: [Role & CMS System Design](../Role%20&%20CMS%20System%20Design/README.md)** (In Progress)
    *   Implement RBAC permission mapping.
    *   Setup dashboard views for different roles.
- [ ] **FJH-96: [Authentication & Session Design](../Authentication%20&%20Session%20Design/README.md)** (In Progress)
    *   Define auth methods (Social vs Credentials).
    *   Setup session persistence logic.
- [ ] **FJH-107: [External Services & Communication Design](../External%20Services%20&%20Communication%20Design/README.md)** (In Progress)
    *   Select email provider (Resend).
    *   Setup Map/Geolocation interaction logic.


---

## 🔮 Phase 3: Menu & Ordering (PLANNED)
**Goal:** The core commerce experience.

1. **Global Menu Engine:** Universal items with per-store overrides.
2. **Stateful Cart:** Complex modifier support and persistence.
3. **Checkout Flow:** Payment integration and order number generation.
- [ ] **FJH-102: [Payment & Transaction Flow Design](../Payment%20&%20Transaction%20Flow%20Design/README.md)** (In Progress)
    *   Select payment gateway (Stripe/Square).
    *   Define DoorDash vs Direct payment logic.

---

## 🍱 Deliverables Manifest
- **[DATA_MODEL.md](./DATA_MODEL.md):** The authoritative schema reference.
- **[DELIVERABLES_SUMMARY.md](./DELIVERABLES_SUMMARY.md):** High-level summary of what's built.
- **[GITHUB_STRUCTURE.md](./GITHUB_STRUCTURE.md):** Repository organization guide.

---

## 🎯 Success Metrics
- **Performance:** Sub-100ms API response time for menu fetches.
- **Accuracy:** 100% order traceability via `AuditLog`.
- **Reliability:** 99.9% uptime for the ordering gateway.

---

**Last Updated:** 2026-04-20 | **Version:** 1.0.0 Roadmap

