# 📡 External Services & Communication Design (FJH-107)

## 📝 Description
Define integration strategy for external services such as email notifications and maps so that communication and location-based features work correctly.

---

## ✅ Acceptance Criteria
- [ ] Email service is selected (Resend / SendGrid)
- [ ] Notification flow is defined (Trigger points)
- [ ] Map/geolocation service is selected (Google Maps / Mapbox)
- [ ] Data flow between system and services is defined

---

## 🛠️ Technical Subtasks
| ID | Task | Status |
| :--- | :--- | :--- |
| **FJH-108** | Select email service (SendGrid / Resend) | ⏳ TO DO |
| **FJH-109** | Define order confirmation email flow | ⏳ TO DO |
| **FJH-110** | Define form submission email flow (franchise/contact) | ⏳ TO DO |
| **FJH-111** | Select map API (Google Maps or alternative) | ⏳ TO DO |
| **FJH-112** | Define location/map interaction logic | ⏳ TO DO |

---

## 🏗️ Design Strategies

### 1. Communication Layer (Email)
*   **Provider**: **Resend** (Recommended for Next.js/React compatibility).
*   **Templating**: Using `React Email` to build responsive and brand-aligned templates.
*   **Flows**:
    *   **Order Confirmation**: Sent immediately after `PaymentSuccess` webhook.
    *   **Staff Alerts**: Real-time notification for new 86'd items or critical stock issues.
    *   **Marketing/CRM**: Opt-in newsletter for loyalty members.

### 2. Location & Maps
*   **Provider**: **Google Maps API**.
*   **Key Features**:
    *   **Store Locator**: Dynamic pins for all FJH locations with "Distance from me" calculation (Browser Geolocation).
    *   **Radius Limitation**: Prevent ordering if the user is outside the store's delivery/service zone.
    *   **Autocomplete**: Used in checkout to ensure accurate delivery addresses for DoorDash handoff.

---

## 🔄 Interaction Logic
1.  **User Enters Site**: Browser requests geolocation permission.
2.  **Mapping**: User is automatically pinned to the nearest FJH location.
3.  **Cross-Check**: System verifies if the selected shop is active and within service hours.
4.  **Handoff**: On checkout, the address is validated via Maps API before being sent to DoorDash.

---
**Status:** 🏗️ Planning Phase
**Parent Task:** [FJH-120 Planning Phase](../roadmap/README.md)
