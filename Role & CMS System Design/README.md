# 🔐 Role & CMS System Design (FJH-94)

## 🏗️ 1. Security & RBAC Structure
We use a **Layered Security Model**. Permissions are checked at both the UI level (hiding buttons) and the API level (blocking unauthorized requests).

### Permission Matrix (Feature Mapping)
| Feature | Guest | Logged-in | Store Admin | Global Admin |
| :--- | :--- | :--- | :--- | :--- |
| Browse Menu & Prices | ✅ | ✅ | ✅ | ✅ |
| Checkout & Payments | ✅ | ✅ | ✅ | ✅ |
| View Personal Orders | ❌ | ✅ | ✅ | ✅ |
| **Manage Store Stock** | ❌ | ❌ | ✅ | ✅ |
| **Override Local Prices**| ❌ | ❌ | ❌ | ✅ |
| **Create New Store** | ❌ | ❌ | ❌ | ✅ |
| **Edit Global Menu** | ❌ | ❌ | ❌ | ✅ |

---

## 🖥️ 2. CMS Approach (Custom Built-in)
Instead of a separate login for an external CMS, the CMS is integrated into the `/dashboard` route group using Next.js Layouts.

### Configurable Content Areas
The following "Slots" are defined for CMS management:
1.  **Home Page Hero**: Main image and marketing text (Global Only).
2.  **Promotion Banner**: Top-bar message (Global with Store-specific override).
3.  **Menu Categories**: Description and visual assets (Global Only).
4.  **Shop Announcements**: Local alerts like "Store closed for renovation" (Store Admin Only).

---

## 🔄 3. Content Update Behavior (Inheritance Logic)
The system follows a **Cascading Content Model**:

1.  **Global Default**: Admin sets a global marketing message.
2.  **Store Inheritance**: All stores display the global message by default.
3.  **Store Override**: If a Store Admin creates a "Local Announcement", it takes priority over the Global message for that shop's specific route.

---

## 🛠️ 4. Technical Implementation Subtasks
*   **FJH-45 & FJH-86**: Extend `NextAuth` session to include roles and implement `PermissionGate` UI wrappers.
*   **FJH-43**: Define the `DynamicContent` model in Prisma to store Key-Value pairs for the CMS slots.
*   **FJH-90**: Logic for `getShopContent(shopId, key)` which falls back to `getGlobalContent(key)` if no local value is found.

---
**Status:** In Progress 🏗️
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
