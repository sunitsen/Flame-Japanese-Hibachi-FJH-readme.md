# 🔐 Role & CMS System Design (FJH-94)

## 📖 Overview
This task defines the security and content management architecture for Flame Japanese Hibachi. We are establishing a **Role-Based Access Control (RBAC)** system and a modular **CMS approach** to handle multi-store content updates.

## 👥 User Roles & Permissions Matrix
| Role | Capabilities | Access Level |
| :--- | :--- | :--- |
| **Guest** | Browse menu, view locations, add to cart. | Public |
| **Logged-in** | Order history, saved addresses, loyalty points. | Private (User) |
| **Store Admin** | Manage store menu, view store orders, update store hours. | Scoped (Store) |
| **Global Admin** | Create stores, manage all users, global system settings. | Full (System) |

## 🏗️ CMS Architecture Approach
**Decision: Custom Built-in CMS**
To ensure tight integration with our multi-store logic and Prisma schema, we are building a custom CMS dashboard rather than using an external headless CMS.

*   **Global Content**: Handled by Global Admins (e.g., promotional banners, base menu items).
*   **Store-Level Content**: Handled by Store Admins (e.g., local announcements, store-specific item availability).

## 📋 Technical Requirements Breakdown

### 1. FJH-43: CMS Approach & Component Mapping
*   Identify configurable areas: Home Page Hero, Promotional Banner, Category Descriptions.
*   Mapping features to UI components in the `(dashboard)` route group.

### 2. FJH-45 & FJH-86: RBAC Structure & Mapping
*   **Session-Based Security**: Using `NextAuth.js` to store roles in the JWT.
*   **Route Gating**: Implementation of a `withRole` high-order component or Middleware route protection.

### 3. FJH-90: Content Update Behavior (Global vs Store-Specific)
*   Define the logic for **Content Inheritance**.
*   Global announcements appear on all store pages unless a store-specific announcement is provided.

---
**Status:** In Progress 🏗️
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
