# Multi-Store System Design (FJH-120)

## 📝 Description
As a technical team, we need to define the multi-store system logic so that the application can support multiple franchise locations with proper data separation, overrides, and user experience.
This includes handling store-specific data, global configurations, and store selection behavior.

## ✅ Acceptance Criteria
- [ ] **Multi-store data structure is defined**: Database schema supports multiple `Shop` entities with relational data.
- [ ] **Store-level vs global data behavior is defined**: Clear distinction between global settings (e.g., system roles) and store-specific settings (e.g., menu pricing).
- [ ] **Store selection flow is defined**: UI/UX flow for customers to select their preferred location.
- [ ] **Store selection persistence logic is defined**: Use of cookies/session to remember selected store.
- [ ] **Data override rules (global vs store-level) are defined**: Logic for when store-specific data should supersede global defaults.
- [ ] **Design supports all store-related functional requirements**: Alignment with the overall project architectural goals.

## 📋 Subtasks

### 1. FJH-85: Define multi-store data structure
- Create a `Shop` model in Prisma.
- Link `Menu`, `Order`, and `Staff` to `ShopId`.
- Ensure data isolation between shops.

### 2. FJH-87: Define how multiple store locations are handled
- Implementation of a "Store Switcher" or "Location Picker".
- URL-based or Cookie-based store identification (e.g., `/store-1/menu`).

### 3. FJH-88: Define global vs store-level data overrides
- Implementation of "Item Override" logic.
- A global menu item can have a different price or availability status at a specific shop.

### 4. FJH-89: Define store selection and persistence logic
- Save `selected_shop_id` in local storage or encrypted cookies.
- Redirect logic if no shop is selected upon entering the app.

---
**Status:** In Progress 🏗️
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
