# 📋 Business Logic – Promotions & Catering (FJH-113)

## 📝 Description
Define business rules for promotions and catering so that these features behave consistently and align with business expectations.

---

## ✅ Acceptance Criteria
- [ ] Promotions logic is defined (Automatic vs Coupon)
- [ ] Discount behavior is defined (Percentage vs Flat)
- [ ] Catering flow is clearly defined (Pre-order vs Real-time)
- [ ] Integration with ordering system is clarified

---

## 🛠️ Technical Subtasks
| ID | Task | Status |
| :--- | :--- | :--- |
| **FJH-114** | Define promotions logic (auto / code-based / time-based) | ⏳ TO DO |
| **FJH-115** | Define discount application behavior | ⏳ TO DO |
| **FJH-116** | Define how promotions interact with cart | ⏳ TO DO |
| **FJH-117** | Define catering flow (form vs ordering system) | ⏳ TO DO |
| **FJH-118** | Define catering submission/processing flow | ⏳ TO DO |

---

## 🏗️ Design Strategies

### 1. Promotions & Discount Engine
We will implement an extensible **Promotion Engine** that checks for validity at the Cart level:
*   **Automatic (Time-based)**: e.g., "Hibachi Happy Hour" (3 PM - 5 PM). System automatically applies discounts based on Server Time.
*   **Coupon-based**: User enters a string (e.g., `FLAME10`). Validated against the `Promotion` model.
*   **Conflict Resolution**: Only one discount can be applied per order (Highest value wins) unless marked as "Stackable".

### 2. Catering & Bulk Orders
Catering requires a separate workflow due to size and prep-time requirements:
*   **Flow**: Users fill out a **Catering Request Form** for orders over $300 or for special events.
*   **Processing**:
    1.  User submits request.
    2.  Store Admin receives notification via **FJH-107 Communication Layer**.
    3.  Admin reviews availability and confirms/rejects.
    4.  If confirmed, a custom payment link is generated for the user.

---

## 🔄 Interaction Logic
1.  **Cart Validation**: On every cart update, the `PromotionEngine` runs a check.
2.  **Discount Injection**: Discounts are applied as a negative line item in the `OrderSummary`.
3.  **Catering Modal**: If cart value exceeds a "Bulk Threshold", the system suggests switching to the Catering Flow for better pricing and scheduling.

---
**Status:** 🏗️ Planning Phase
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
