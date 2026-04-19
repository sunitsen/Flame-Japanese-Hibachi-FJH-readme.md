# 🏢 Multi-Store System Design (FJH-120)

## 📖 Overview
The Flame Japanese Hibachi platform is designed as a **Multi-Tenant / Multi-Location** application. This design ensures that while we maintain a single codebase and database, each franchise location (Shop) can operate independently with its own menu prices, staff, and operating hours.

## 📐 System Architecture
```mermaid
graph TD
    subgraph Global_Level
        GA[Global Admin] --> GM[Global Menu Items]
        GM --> GP[Base Pricing]
    end

    subgraph Store_Level
        S1[Store 1: Dallas]
        S2[Store 2: Houston]
    end

    GP -- Fallback --> S1
    GP -- Fallback --> S2

    SO1[Store 1 Overrides] --> S1
    SO2[Store 2 Overrides] --> S2

    Customer --> SP{Store Picker}
    SP --> S1
    SP --> S2
```

## 📋 Technical Requirements Breakdown

### 1. FJH-85: Data Structure Definition
To support multi-tenancy, the following relational rules are enforced:
*   **Primary Key Isolation**: All operational models (`Order`, `CartItem`, `Staff`, `MenuCategory`) must include a `shopId` foreign key.
*   **Model Relations**:
    *   `Shop`: The root entity containing location headers, addresses, and status.
    *   `ShopSettings`: Specific configurations like `taxRate`, `deliveryRadius`, and `isAcceptingOrders`.

### 2. FJH-87: Multiple Location Handling
*   **Routing Strategy**: We utilize **dynamic segment routing** (e.g., `fjh.com/[shop-slug]/menu`).
*   **Middleware Detection**: A Next.js Middleware intercepts requests to check for a `selected_shop` cookie. If missing, it triggers a redirect to the `/location-picker`.

### 3. FJH-88: Data Override Engine (Global vs Store-Level)
This is the most critical logic for franchise operations.
*   **Priority Logic**: `Store_Override` > `Global_Default`.
*   **Implementation**: An `ItemOverride` table will store specific price/availability values.
    *   *Example*: If "Steak Hibachi" is $15 globally but $17 in NYC, the NYC store ID will have a record in `ItemOverride` linked to that item ID.

### 4. FJH-89: Selection & Persistence Logic
*   **Storage**: `localStorage` for UI state and **Encrypted HttpOnly Cookies** for server-side availability.
*   **Persistence**: Once a store is selected, the cart is "locked" to that location to prevent cross-location order errors.

---

## 👨‍💻 Developer Implementation Guide

### Store Context Provider
Developers should use the `useShop` hook to access the current shop context across the application:
```typescript
const { currentShop, isLoading } = useShop();
```

### Data Fetching Pattern
Always include the `shopId` in your Prisma queries:
```typescript
const items = await prisma.menuItem.findMany({
  where: {
    shopId: currentShop.id,
    isAvailable: true,
  }
});
```

---
**Status:** Architecture Defined ✅ | **Implementation Stage:** Phase 2 (Shop Management)
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
