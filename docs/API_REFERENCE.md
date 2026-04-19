# 📚 FJH API Reference

This document provides detailed request and response schemas for the Flame Japanese Hibachi API. All endpoints follow RESTful conventions.

---

## 🔐 Authentication

### POST `/api/auth/login`
Authenticates a user and returns a session token.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword123"
}
```

**Response (200 OK):**
```json
{
  "user": {
    "id": "u_123",
    "email": "user@example.com",
    "role": "CUSTOMER"
  },
  "token": "eyJhbGci..."
}
```

---

## 🛒 Orders

### POST `/api/orders`
Creates a new order for a specific location.

**Request Body:**
```json
{
  "shopId": "shop_001",
  "items": [
    {
      "itemId": "item_001",
      "quantity": 2,
      "modifiers": [
        { "id": "mod_001", "value": "Spicy" }
      ]
    }
  ],
  "fulfillmentType": "DELIVERY",
  "addressId": "addr_123"
}
```

**Response (201 Created):**
```json
{
  "id": "ord_2024_001",
  "orderNumber": "FJH-2024-00001",
  "status": "PENDING",
  "totalAmount": 45.50,
  "createdAt": "2026-04-20T00:00:00Z"
}
```

---

## 📋 Error Response Format

All error responses follow this standard structure:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request payload is invalid.",
    "details": [
      {
        "field": "email",
        "issue": "Invalid email format"
      }
    ]
  }
}
```

**Common Error Codes:**
- `UNAUTHORIZED`: Invalid or missing credentials.
- `FORBIDDEN`: User does not have permission for this action.
- `NOT_FOUND`: Referenced resource does not exist.
- `CONFLICT`: Resource state conflict (e.g., item out of stock).
- `RATE_LIMITED`: Too many requests.
