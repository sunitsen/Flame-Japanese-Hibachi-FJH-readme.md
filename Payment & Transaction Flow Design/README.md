# 💳 Payment & Transaction Flow Design (FJH-102)

## 📝 Description
Define how payments are handled within the system so that checkout flow is seamless, secure, and aligned with business requirements.

---

## ✅ Acceptance Criteria
- [ ] Payment gateway is selected (Direct vs Integration)
- [ ] Payment flow is defined for all order types
- [ ] Success/failure states are defined (webhooks, retries)
- [ ] Payment integrates with order system and KDS

---

## 🛠️ Technical Subtasks
| ID | Task | Status |
| :--- | :--- | :--- |
| **FJH-103** | Select payment gateway (Stripe / Square / other) | ⏳ TO DO |
| **FJH-104** | Define checkout payment flow | ⏳ TO DO |
| **FJH-105** | Define success and failure states | ⏳ TO DO |
| **FJH-106** | Define order confirmation trigger | ⏳ TO DO |

---

## 🏗️ Design Strategies (Dual Flow)

We are implementing a hybrid payment architecture to support both direct pickup and DoorDash delivery orders.

### 1. Direct Payment Setup (Internal Checkout)
*   **Gateway**: Stripe (Recommended)
*   **Workflow**:
    1.  Customer selects "Pickup" or "Dine-in".
    2.  Website presents the Stripe Elements payment sheet.
    3.  Payment is processed immediately before the order hits the kitchen.
    4.  **Backend**: `PaymentIntent` is created and confirmed via Webhook.

### 2. DoorDash Integration Flow (External Checkout)
*   **Gateway**: DoorDash Drive / MarketPlace Integration
*   **Workflow**:
    1.  Customer selects "Delivery".
    2.  Order is passed to DoorDash via the API.
    3.  **Payment Handling**:
        *   *Option A (DoorDash MarketPlace)*: User pays on DoorDash app/site. Our system receives the order with `payment_status: paid_externally`.
        *   *Option B (DoorDash Drive)*: Order is placed on our site, but we use DoorDash for delivery only. In this case, we still process the payment via Stripe but include the delivery fee calculated by DoorDash.
*   **Extra Setup**: Requires DoorDash API Key, Location ID mapping, and Webhook configuration to receive "Order Picked up" or "Delivery Failed" statuses.

---

## 🔄 Success & Failure Handling
| State | Behavior |
| :--- | :--- |
| **Payment Success** | Redirect to `/orders/[id]/confirmation`, trigger `print-ticket` to KDS. |
| **Payment Declined** | Stay on checkout, show inline error, do not clear cart. |
| **Webhook Delay** | Use a "Pending" UI state while waiting for 3-5 seconds before asking user to check order history. |

---
**Status:** 🏗️ Planning Phase
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
