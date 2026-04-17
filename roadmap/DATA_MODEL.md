# Data Model Documentation

**Flame Japanese Hibachi - Complete Prisma Data Model**

This document is the authoritative reference for the system's data architecture, domain design, and database schema. Use this as your guide for understanding relationships, constraints, and design decisions.

---

## 📖 Table of Contents

1. [System Architecture](#system-architecture)
2. [Core Domains](#core-domains)
3. [Entity Relationship Overview](#entity-relationship-overview)
4. [Complete Model Reference](#complete-model-reference)
5. [Relationship Design](#relationship-design)
6. [Enums Reference](#enums-reference)
7. [Access Patterns](#access-patterns)
8. [Soft Delete Strategy](#soft-delete-strategy)
9. [Indexes & Performance](#indexes--performance)
10. [Common Queries](#common-queries)

---

## System Architecture

### Design Pattern: Modular Monolith

The system is built as a **modular monolith** with clear domain boundaries but unified deployment. Each domain is independently testable and maintainable but uses the same database and API gateway.

```
┌─────────────────────────────────────────────────┐
│         API Gateway / Route Layer                │
├─────────────────────────────────────────────────┤
│  Auth    │  Shop    │  Menu    │  Order  │ ...  │
│ Service  │ Service  │ Service  │ Service │      │
├─────────────────────────────────────────────────┤
│           Prisma ORM (Data Access Layer)        │
├─────────────────────────────────────────────────┤
│    PostgreSQL Database (Single Instance)        │
└─────────────────────────────────────────────────┘
```

### Key Principles

1. **Domain Isolation** - Each domain has independent tables; cross-domain queries are minimal
2. **Role-Based Segregation** - Access control enforced at API layer, not DB constraints
3. **Soft Deletes** - Data never hard-deleted; deletedAt field marks inactive records
4. **Audit Trail** - All state changes logged in AuditLog for compliance
5. **Scalable Normalization** - Proper normalization with flexibility where needed (JSON fields)

---

## Core Domains

### 1. Authentication Domain
**Models:** User, UserProfile, AuthAccount, Session

**Purpose:** Identity management and authentication provider integration

**Key Relationships:**
- User (1) → UserProfile (1)
- User (1) → AuthAccount (many) - supports multiple auth providers per user
- User (1) → Session (many) - active sessions

**Design Notes:**
- Supports local auth + multiple OAuth providers
- Sessions stored in DB (can be migrated to Redis for scale)
- AuthAccount stores provider-specific data (access tokens, etc.)

---

### 2. Shop & Location Domain
**Models:** Shop, Address, StoreOperatingHours, StoreStatus

**Purpose:** Multi-location restaurant operations management

**Key Relationships:**
- Shop (1) → Address (1)
- Shop (1) → StoreOperatingHours (1)
- Shop (1) → StoreStatus (1)
- User (Shop Admin) → Shop (1)

**Design Notes:**
- Each shop is independent operational unit
- Address stores geographic data for location services
- StoreOperatingHours uses JSON for flexibility (hours vary by day)
- StoreStatus tracks real-time operational state (open/closed/maintenance)
- Soft delete on Shop preserves historical orders

---

### 3. Menu & Inventory Domain
**Models:** MenuItem, MenuCategory, ShopMenuItem, ModifierGroup, ModifierOption

**Purpose:** Global menu definitions with store-level customization

**Key Relationships:**
- MenuCategory (1) → MenuItem (many)
- MenuItem (1) → ModifierGroup (many) - options like "Sauce", "Temperature"
- ModifierGroup (1) → ModifierOption (many) - e.g., "Spicy", "Mild"
- MenuItem (1) → ShopMenuItem (many) - store-specific overrides

**Design Notes:**
- **MenuItem** is the global source-of-truth (created by Super Admin)
- **ShopMenuItem** allows per-location customization without duplicating MenuItem
- ModifierGroup/Option enables complex product customization
- availabilityScope enum controls scope: ALL_SHOPS, SPECIFIC_SHOPS, or STORE_OVERRIDE
- Shop can override price, availability, and stock without modifying global item

**Example Flow:**
```
Global MenuItem "Hibachi Chicken" ($18.99)
    ↓
Shop A overrides: $19.99, 30 in stock
Shop B overrides: UNAVAILABLE (out of stock)
Shop C uses global price and unlimited stock
```

---

### 4. Cart & Checkout Domain
**Models:** ShoppingCart, CartItem, CustomerAddress, SavedPaymentMethod

**Purpose:** Stateful shopping experience before order placement

**Key Relationships:**
- ShoppingCart (1) → CartItem (many)
- CartItem references MenuItem (not stored; fetched for display)
- User (1) → CustomerAddress (many)
- User (1) → SavedPaymentMethod (many)

**Design Notes:**
- ShoppingCart has expiresAt (24-hour expiration)
- CartItem stores quantity + selected modifiers as JSON
- Modifiers re-validated at checkout to prevent price manipulation
- CustomerAddress links User to Address (allows address sharing/reuse)
- SavedPaymentMethod stores tokenized payment info (never full credit card)

---

### 5. Order & Fulfillment Domain
**Models:** Order, OrderItem, OrderItemModifier, GuestOrderProfile, DoorDashDispatch

**Purpose:** Order lifecycle from placement to delivery handoff

**Key Relationships:**
- Order (1) → OrderItem (many)
- OrderItem (1) → OrderItemModifier (many)
- Order (1) → GuestOrderProfile (0..1) - only for guest orders
- Order (1) → DoorDashDispatch (0..1) - created at handoff time

**Order Status Flow:**
```
PENDING → CONFIRMED → PREPARING → READY_FOR_PICKUP → 
HANDOFF_TO_DOORDASH → COMPLETED

Alternative paths:
PENDING → CANCELLED (customer cancels)
PREPARING → FAILED (order error)
```

**Design Notes:**
- Order is immutable after creation (status updates only)
- OrderItem captures exact state at order time (price, modifiers)
- GuestOrderProfile stores guest contact info (no User record needed)
- DoorDashDispatch handles external order sync with retry logic
- Soft delete NOT used; orders persist forever for audit trail

---

### 6. Promotions & Discounts Domain
**Models:** Promotion, Coupon

**Purpose:** Flexible discount system with global/store-specific scope

**Key Relationships:**
- Promotion can be global (shopId = null) or store-specific
- Coupon applicableShops/applicableItems stored as JSON (flexible targeting)

**Design Notes:**
- **Promotion:** Internal marketing offers with scheduling
  - Type: SEASONAL, FLASH_SALE, LOYALTY, REFERRAL, BUNDLE, FIRST_TIME_USER, STORE_OPENING
  - Scope: Global or shop-specific
  - Time-bounded: startsAt, endsAt
  - Usage-limited: maxUsagePerCustomer, maxTotalUsages

- **Coupon:** Redeemable codes customers enter
  - Code: unique, cryptographically random (prevent enumeration)
  - Eligibility: minOrderAmount, applicableShops, applicableItems
  - Limited: maxUsagePerCustomer, maxTotalUsages

**Example:**
```
Global Promotion: "Welcome 20% Off" (for first-time customers)
Store Promotion: "Happy Hour 10% Off" (6pm-8pm, Shop A only)
Coupon: "WELCOME20" (redeemable, single-use per customer)
```

---

### 7. Access Control & Audit Domain
**Models:** EmployeeAssignment, AuditLog

**Purpose:** Role-based access control and compliance logging

**Key Relationships:**
- User (1) → EmployeeAssignment (many) - one per shop
- EmployeeAssignment stores permissions as JSON array
- AuditLog captures all state changes with actor/timestamp/changes

**Design Notes:**
- **EmployeeAssignment:**
  - Links User to Shop with a role (e.g., "Order Manager")
  - Permissions are flexible JSON array: ["orders.manage", "menu.view", "inventory.update"]
  - employmentStatus tracks: ACTIVE, INACTIVE, SUSPENDED, TERMINATED
  - Soft deletable for historical records

- **AuditLog:**
  - Immutable log of all changes
  - Captures: action, entityType, entityId, oldValues, newValues
  - Stores IP + user agent for forensics
  - Never deleted (compliance requirement)

**Permission Examples:**
```json
Super Admin permissions: ["shops.create", "shops.delete", "menu.manage", "promotions.create", "analytics.view"]
Shop Admin permissions: ["orders.manage", "menu.availability", "inventory.update", "employees.manage", "analytics.view"]
Employee permissions: ["orders.view", "orders.status-update", "menu.availability"]
```

---

### 8. Analytics Domain
**Models:** AnalyticsSnapshot

**Purpose:** Pre-computed metrics for dashboard performance

**Key Relationships:**
- One snapshot per shop per date per period (unique constraint)

**Design Notes:**
- Snapshots computed daily/weekly/monthly via background job
- Stores aggregated metrics: totalOrders, totalRevenue, topItems, etc.
- topItems stored as JSON array for flexibility
- Null shopId = system-wide metrics
- Enables fast dashboard queries without scanning millions of orders

**Example Snapshot:**
```json
{
  "shopId": "shop_001",
  "date": "2024-01-15",
  "period": "daily",
  "totalOrders": 42,
  "totalRevenue": 1125.50,
  "averageOrderValue": 26.80,
  "totalCustomers": 38,
  "newCustomers": 3,
  "topItems": [
    { "itemId": "item_001", "name": "Hibachi Chicken", "count": 12, "revenue": 227.88 },
    { "itemId": "item_002", "name": "Hibachi Shrimp", "count": 10, "revenue": 189.90 }
  ]
}
```

---

## Entity Relationship Overview

### High-Level Diagram

```
┌──────────────┐
│    USER      │ (Identity anchor)
│  (id, email) │
└──────┬───────┘
       │
       ├──→ UserProfile (1:1) - Bio, preferences, loyalty points
       ├──→ AuthAccount (1:many) - OAuth/local auth
       ├──→ Session (1:many) - Active sessions
       ├──→ Shop (1:1) - If shop admin
       ├──→ EmployeeAssignment (1:many) - Staff roles
       ├──→ Order (1:many) - Customer orders
       ├──→ ShoppingCart (1:many) - Shopping state
       ├──→ CustomerAddress (1:many) - Delivery addresses
       └──→ SavedPaymentMethod (1:many) - Payment methods

┌──────────────┐
│    SHOP      │ (Location)
│  (id, name)  │
└──────┬───────┘
       │
       ├──→ Address (1:1) - Geographic data
       ├──→ StoreOperatingHours (1:1) - Business hours
       ├──→ StoreStatus (1:1) - Real-time state
       ├──→ ShopMenuItem (1:many) - Menu overrides
       ├──→ EmployeeAssignment (1:many) - Staff
       ├──→ Order (1:many) - Received orders
       ├──→ Promotion (1:many) - Store offers
       └──→ AnalyticsSnapshot (1:many) - Metrics

┌────────────────┐
│   MENUITEM     │ (Global)
│  (id, name)    │
└────────┬───────┘
         │
         ├──→ MenuCategory (many:1) - Categorization
         ├──→ ModifierGroup (1:many) - Options
         ├──→ ShopMenuItem (1:many) - Store overrides
         └──→ OrderItem (1:many) - In orders

┌─────────────┐
│   ORDER     │ (Purchase)
│ (id, total) │
└──────┬──────┘
       │
       ├──→ OrderItem (1:many) - Line items
       ├──→ OrderItemModifier (1:many) - Customizations
       ├──→ GuestOrderProfile (1:1) - If guest
       └──→ DoorDashDispatch (1:1) - External order
```

---

## Complete Model Reference

### User
Core identity record. Every actor in the system (Super Admin, Shop Admin, Employee, Customer) has a User record.

```prisma
model User {
  id            String    @id @default(cuid())
  email         String    @unique           // Unique identifier
  phoneNumber   String?
  firstName     String?
  lastName      String?
  role          UserRole  @default(CUSTOMER)
  
  // Relations
  profile       UserProfile?
  authAccounts  AuthAccount[]
  sessions      Session[]
  shopAdminOf   Shop?
  employmentRecords EmployeeAssignment[]
  addresses     CustomerAddress[]
  orders        Order[]
  carts         ShoppingCart[]
  paymentMethods SavedPaymentMethod[]
  auditLogs     AuditLog[]
  
  // Metadata
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  deletedAt     DateTime?  // Soft delete
  
  @@index([email])
  @@index([role])
}
```

**Key Fields:**
- `role`: UserRole enum (SUPER_ADMIN, SHOP_ADMIN, EMPLOYEE, CUSTOMER, GUEST)
- `email`: Unique identifier for login
- `deletedAt`: Soft delete marker (null = active, set = deleted)

**Access Patterns:**
- Find user by email: `User.findUnique({ where: { email } })`
- Find all admins: `User.findMany({ where: { role: 'SHOP_ADMIN' } })`
- Find user with session: `User.include({ sessions: true })`

---

### Shop
Represents a restaurant location. Each shop operates independently but shares global menu.

```prisma
model Shop {
  id            String    @id @default(cuid())
  name          String    // e.g., "Flame Tokyo"
  description   String?
  adminId       String?   @unique  // FK to User (SHOP_ADMIN)
  
  // Settings
  status        ShopStatus @default(ACTIVE)
  phoneNumber   String?
  email         String?
  timezone      String    @default("UTC")
  currency      String    @default("USD")
  minOrderValue Float     @default(0)
  deliveryFee   Float     @default(0)
  
  // External Integration
  doorDashStoreId String?
  
  // Relations
  address       Address?
  operatingHours StoreOperatingHours?
  storeStatus   StoreStatus?
  employees     EmployeeAssignment[]
  menuItems     ShopMenuItem[]
  orders        Order[]
  promotions    Promotion[]
  analyticsSnapshots AnalyticsSnapshot[]
  auditLogs     AuditLog[]
  
  // Metadata
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  deletedAt     DateTime?  // Soft delete for closed shops
  
  @@index([status])
  @@index([adminId])
}
```

**Key Fields:**
- `adminId`: Foreign key to User (shop admin)
- `status`: ShopStatus enum (ACTIVE, INACTIVE, SUSPENDED, ARCHIVED)
- `doorDashStoreId`: External reference for DoorDash API
- `timezone`: For correct order timestamps and business hours

**Design Rationale:**
- Soft delete preserves historical orders even when shop closes
- Each shop has its own settings (delivery fee, min order) independent of others
- adminId is unique (one admin per shop in current design; can extend)

---

### MenuItem
Global menu item definition shared across all shops. Defines what customers can order.

```prisma
model MenuItem {
  id            String    @id @default(cuid())
  name          String
  description   String?
  imageUrl      String?
  
  // Categorization
  categoryId    String
  
  // Pricing (global base)
  basePrice     Float
  
  // Properties
  isVegetarian  Boolean   @default(false)
  isGlutenFree  Boolean   @default(false)
  isSpicy       Boolean   @default(false)
  spicyLevel    Int?      // 1-5
  
  // Availability
  availabilityScope ItemAvailabilityScope @default(STORE_OVERRIDE)
  globallyAvailable Boolean @default(true)
  
  // Relations
  category      MenuCategory @relation(fields: [categoryId], references: [id])
  modifierGroups ModifierGroup[]
  shopItems     ShopMenuItem[]
  orderItems    OrderItem[]
  promotions    Promotion[]
  
  // Metadata
  createdAt     DateTime  @default(now())
  updatedAt     DateTime @updatedAt
  deletedAt     DateTime?
  
  @@index([categoryId])
  @@index([globallyAvailable])
}
```

**Key Fields:**
- `basePrice`: Global fallback price (can be overridden per shop)
- `availabilityScope`: Controls override behavior (ALL_SHOPS, SPECIFIC_SHOPS, STORE_OVERRIDE)
- `globallyAvailable`: Quick flag for querying available items
- `modifierGroups`: Links to customization options

**Design Pattern:**
- Super Admin creates MenuItem once
- Each Shop can override via ShopMenuItem (price, availability, stock)
- No duplication; one source of truth per item

---

### ShopMenuItem
Store-specific override for a MenuItem. Allows customization without duplicating the global item.

```prisma
model ShopMenuItem {
  id            String    @id @default(cuid())
  
  // Composite FK
  shopId        String
  itemId        String
  
  // Store-specific data
  price         Float?    // Null = use MenuItem.basePrice
  availabilityStatus AvailabilityStatus @default(AVAILABLE)
  stockQuantity Int?      // Null = unlimited
  
  storeSpecificNotes String?
  
  // Relations
  shop          Shop      @relation(fields: [shopId], references: [id])
  item          MenuItem  @relation(fields: [itemId], references: [id])
  
  // Metadata
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  @@unique([shopId, itemId])  // Prevent duplicates
  @@index([shopId])
  @@index([availabilityStatus])
}
```

**Key Fields:**
- `price`: Null means use MenuItem.basePrice; set to override
- `availabilityStatus`: AVAILABLE, UNAVAILABLE, OUT_OF_STOCK
- `stockQuantity`: Null = unlimited; integer = tracked inventory

**Example:**
```
MenuItem "Hibachi Chicken" basePrice=$18.99 (global)
  ShopMenuItem for Shop A: price=$19.99, stock=30
  ShopMenuItem for Shop B: availabilityStatus=OUT_OF_STOCK
  ShopMenuItem for Shop C: (uses all global values)
```

---

### Order
Represents a customer purchase. Immutable after creation (only status updates).

```prisma
model Order {
  id            String    @id @default(cuid())
  orderNumber   String    @unique  // Human-readable
  
  // References
  shopId        String
  customerId    String?   // Null for guest orders
  
  // Status
  status        OrderStatus @default(PENDING)
  fulfillmentType FulfillmentType @default(DELIVERY)
  paymentStatus PaymentStatus @default(PENDING)
  
  // Amounts
  subtotal      Float
  tax           Float
  deliveryFee   Float     @default(0)
  discountAmount Float    @default(0)
  totalAmount   Float
  
  // Promotions
  appliedCoupons String?   // JSON: ["WELCOME20"]
  appliedPromotions String? // JSON: ["promo_001"]
  
  // Delivery
  deliveryAddressId String?
  specialInstructions String?
  
  // Payment
  paymentMethod String?   // "credit_card", "apple_pay", etc.
  
  // Relations
  shop          Shop      @relation(fields: [shopId], references: [id])
  customer      User?     @relation("OrderCustomer", fields: [customerId], references: [id])
  guestProfile  GuestOrderProfile?
  items         OrderItem[]
  doorDashDispatch DoorDashDispatch?
  
  // Timestamps (status progression)
  confirmedAt   DateTime?
  prepStartedAt DateTime?
  readyForPickupAt DateTime?
  handoffToDoorDashAt DateTime?
  completedAt   DateTime?
  cancelledAt   DateTime?
  
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  @@index([customerId])
  @@index([shopId])
  @@index([status])
  @@index([paymentStatus])
  @@index([createdAt])
}
```

**Order Status Flow:**
```
PENDING
  ↓ (shop confirms)
CONFIRMED
  ↓ (kitchen starts)
PREPARING
  ↓ (food ready)
READY_FOR_PICKUP
  ↓ (give to DoorDash)
HANDOFF_TO_DOORDASH
  ↓ (delivered)
COMPLETED

Alternative endings:
PENDING → CANCELLED (customer cancels before confirmation)
PREPARING → FAILED (kitchen error, cannot continue)
```

**Design Notes:**
- `orderNumber`: Human-readable (e.g., "FJH-2024-00001") for easy reference
- `customerId`: Null if guest order; uses GuestOrderProfile instead
- `appliedCoupons`/`appliedPromotions`: JSON arrays for flexible tracking
- Timestamp fields track status progression; immutable once set
- Never soft-deleted; maintains full history for analytics

---

### OrderItem
Line item in an order. Captures exact state at order time.

```prisma
model OrderItem {
  id            String    @id @default(cuid())
  
  // References
  orderId       String
  itemId        String
  
  // Amounts (captured at order time)
  quantity      Int
  unitPrice     Float     // Price at time of order
  subtotal      Float     // quantity * unitPrice
  
  specialInstructions String?
  
  // Relations
  order         Order     @relation(fields: [orderId], references: [id])
  item          MenuItem  @relation(fields: [itemId], references: [id])
  modifiers     OrderItemModifier[]
  
  createdAt     DateTime  @default(now())
  
  @@index([orderId])
  @@index([itemId])
}
```

**Key Fields:**
- `unitPrice`: Locked in at order time (prevents historical disputes)
- `subtotal`: quantity × unitPrice (pre-calculated)
- `modifiers`: Related OrderItemModifier records for customizations

**Purpose:** Preserves exact order state; if menu prices change later, orders retain historical pricing.

---

### GuestOrderProfile
Stores contact info for guest (non-authenticated) orders.

```prisma
model GuestOrderProfile {
  id            String    @id @default(cuid())
  orderId       String    @unique
  
  guestEmail    String
  guestPhoneNumber String?
  guestName     String?
  
  order         Order     @relation(fields: [orderId], references: [id])
  
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  @@index([guestEmail])
}
```

**Design Notes:**
- Only created if Order.customerId is null (guest checkout)
- Enables order tracking via email + order number
- No account required; contact info captured for notifications

---

### DoorDashDispatch
Tracks external order sync with DoorDash. Separate from Order for clean integration boundary.

```prisma
model DoorDashDispatch {
  id            String    @id @default(cuid())
  
  orderId       String    @unique
  shopId        String
  
  // DoorDash References
  doorDashOrderId String? @unique  // External order ID
  doorDashDeliveryId String?       // Delivery ID
  
  // Status
  handoffStatus String  @default("pending")  // pending, sent, confirmed, completed, failed
  
  // Retry Logic
  errorMessage  String?
  retryCount    Int     @default(0)
  lastRetryAt   DateTime?
  
  // Debug Info
  handoffPayload Json?   // Full request sent to DoorDash
  doorDashResponse Json?  // Full response received
  
  // Relations
  order         Order    @relation(fields: [orderId], references: [id])
  shop          Shop     @relation(fields: [shopId], references: [id])
  
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  
  @@index([orderId])
  @@index([shopId])
  @@index([handoffStatus])
}
```

**Design Rationale:**
- Separate from Order for clean decoupling
- doorDashOrderId uniqueness prevents duplicate sends (idempotent)
- Stores full request/response for debugging
- Retry logic with exponential backoff

**Handoff Flow:**
```
1. Shop marks order READY_FOR_PICKUP
2. System creates DoorDashDispatch (handoffStatus="pending")
3. Call DoorDash API with order data
4. Store doorDashOrderId in response
5. Set handoffStatus="sent"
6. DoorDash webhook updates status as delivery progresses
7. Finally set handoffStatus="completed"

Retry logic:
- If API call fails → increment retryCount, set lastRetryAt
- Exponential backoff: 1 min, 5 min, 15 min, 1 hour
- Max 3 attempts before manual intervention
```

---

### Promotion
Internal marketing offer. Can be global or shop-specific.

```prisma
model Promotion {
  id            String    @id @default(cuid())
  
  // Scope
  shopId        String?   // Null = global
  
  // Details
  name          String
  description   String?
  type          PromotionType
  
  // Discount
  discountType  CouponType  // PERCENTAGE, FIXED_AMOUNT, etc.
  discountValue Float
  
  // Eligibility
  minOrderAmount Float?
  applicableItems String?  // JSON: itemIds or null = all
  
  // Scheduling
  status        PromotionStatus @default(DRAFT)
  startsAt      DateTime?
  endsAt        DateTime?
  
  // Limits
  maxUsagePerCustomer Int?
  maxTotalUsages Int?
  currentUsageCount Int @default(0)
  
  // Relations
  shop          Shop?     @relation(fields: [shopId], references: [id])
  
  // Metadata
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  deletedAt     DateTime?
  
  @@index([shopId])
  @@index([status])
}
```

**Usage Examples:**
```
Global: "Welcome 20% Off" for first-time users (shopId=null)
Store: "Happy Hour 10% Off" 6pm-8pm at Shop A only (shopId="shop_001")
Limited: "BOGO Shrimp" - buy one, get one free (maxTotalUsages=50)
```

---

### Coupon
Redeemable discount code. Created by Super Admin.

```prisma
model Coupon {
  id            String    @id @default(cuid())
  code          String    @unique  // e.g., "WELCOME20"
  
  // Discount
  type          CouponType
  discountValue Float
  
  // Eligibility
  applicableShops String?   // JSON or null = all
  applicableItems String?   // JSON or null = all
  minOrderAmount Float?
  
  // Scheduling
  startsAt      DateTime?
  endsAt        DateTime?
  
  // Limits
  maxUsagePerCustomer Int?
  maxTotalUsages Int?
  currentUsageCount Int @default(0)
  
  isActive      Boolean   @default(true)
  
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  deletedAt     DateTime?
  
  @@index([code])
  @@index([isActive])
}
```

**Validation on Apply:**
```typescript
// Pseudo-code for coupon validation
if (!coupon.isActive) throw InvalidCoupon();
if (coupon.startsAt > now()) throw CouponNotStarted();
if (coupon.endsAt < now()) throw CouponExpired();
if (orderAmount < coupon.minOrderAmount) throw MinimumNotMet();
if (coupon.applicableShops && !coupon.applicableShops.includes(shopId)) throw NotApplicableToShop();
if (coupon.maxTotalUsages && coupon.currentUsageCount >= coupon.maxTotalUsages) throw MaxUsagesReached();
if (coupon.maxUsagePerCustomer && customerUsageCount >= coupon.maxUsagePerCustomer) throw CustomerMaxReached();
// All checks passed; apply discount
```

---

### EmployeeAssignment
Links an employee (User) to a shop with a role and permissions.

```prisma
model EmployeeAssignment {
  id            String    @id @default(cuid())
  
  // Composite FK
  userId        String
  shopId        String
  
  // Role & Status
  employmentStatus EmploymentStatus @default(ACTIVE)
  role          String    // e.g., "Order Manager", "Kitchen Staff"
  
  // Permissions (flexible JSON array)
  permissions   String[]  @default([])
  // Examples:
  // ["orders.manage", "menu.availability", "inventory.update"]
  // ["orders.view", "orders.status-update"]
  
  // Relations
  user          User      @relation(fields: [userId], references: [id])
  shop          Shop      @relation(fields: [shopId], references: [id])
  
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  deletedAt     DateTime?
  
  @@unique([userId, shopId])  // One assignment per user per shop
  @@index([userId])
  @@index([shopId])
  @@index([employmentStatus])
}
```

**Permission Examples:**
```json
[
  "orders.view",
  "orders.manage",
  "menu.availability",
  "inventory.update"
]
```

---

### AuditLog
Immutable compliance log. Captures every state change.

```prisma
model AuditLog {
  id            String    @id @default(cuid())
  
  // Actor
  actorId       String?   // Null for system actions
  
  // Action
  action        AuditAction
  entityType    String    // "Order", "MenuItem", "Shop", etc.
  entityId      String
  
  // Context
  shopId        String?
  
  // State Change
  oldValues     Json?     // Previous state
  newValues     Json?     // New state
  changeDescription String?
  
  // Forensics
  ipAddress     String?
  userAgent     String?
  
  // Relations
  actor         User?     @relation("AuditActorUser", fields: [actorId], references: [id])
  shop          Shop?     @relation(fields: [shopId], references: [id])
  
  createdAt     DateTime  @default(now())
  
  @@index([actorId])
  @@index([entityType])
  @@index([entityId])
  @@index([shopId])
  @@index([createdAt])
}
```

**Example Entries:**
```json
{
  "action": "UPDATE",
  "entityType": "Order",
  "entityId": "order_001",
  "actorId": "user_456",
  "oldValues": { "status": "PENDING" },
  "newValues": { "status": "CONFIRMED" },
  "changeDescription": "Order confirmed by shop admin",
  "ipAddress": "192.168.1.1",
  "createdAt": "2024-01-15T11:35:00Z"
}
```

---

## Relationship Design

### One-to-One Relationships

| Parent | Child | FK Location | Delete Behavior | Notes |
|--------|-------|------------|-----------------|-------|
| User | UserProfile | Child | Cascade | Profile auto-deleted with user |
| Shop | Address | Child | Cascade | Shop address unique |
| Shop | StoreOperatingHours | Child | Cascade | Hours unique per shop |
| Shop | StoreStatus | Child | Cascade | Status unique per shop |
| Order | GuestOrderProfile | Child | Cascade | Only for guest orders |
| Order | DoorDashDispatch | Child | Cascade | Created at handoff time |

### One-to-Many Relationships

| Parent | Child | Cardinality | Notes |
|--------|-------|-------------|-------|
| User | AuthAccount | 1:many | Multiple auth providers per user |
| User | Session | 1:many | Multiple active sessions |
| User | EmployeeAssignment | 1:many | One per shop (unique constraint) |
| User | Order | 1:many | Customer's purchases |
| User | ShoppingCart | 1:many | One per shop typically |
| User | CustomerAddress | 1:many | Multiple saved addresses |
| Shop | ShopMenuItem | 1:many | Override for each global item |
| Shop | EmployeeAssignment | 1:many | Multiple staff per shop |
| Shop | Order | 1:many | Orders received by shop |
| Shop | Promotion | 1:many | Store-specific offers |
| MenuCategory | MenuItem | 1:many | Items grouped by category |
| MenuItem | ModifierGroup | 1:many | e.g., sauce, temperature, size |
| MenuItem | ShopMenuItem | 1:many | Store-specific overrides |
| ModifierGroup | ModifierOption | 1:many | e.g., spicy, mild, no sauce |
| Order | OrderItem | 1:many | Line items in order |
| OrderItem | OrderItemModifier | 1:many | Customizations selected |
| ShoppingCart | CartItem | 1:many | Items in cart |

### Many-to-Many Relationships (Implicit via JSON)

| Entity A | Entity B | Storage | Notes |
|----------|----------|---------|-------|
| Promotion | MenuItem | applicableItems JSON | Flexible item targeting |
| Coupon | Shop | applicableShops JSON | Flexible shop targeting |
| Coupon | MenuItem | applicableItems JSON | Flexible item targeting |
| EmployeeAssignment | Permission | permissions JSON array | Flexible permission model |

---

## Enums Reference

### UserRole
```prisma
enum UserRole {
  SUPER_ADMIN    // System-wide control
  SHOP_ADMIN     // Single shop control
  EMPLOYEE       // Shop operations
  CUSTOMER       // End user
  GUEST          // Unauthenticated
}
```

---

**Last Updated:** January 2024 | **Version:** 1.0.0
