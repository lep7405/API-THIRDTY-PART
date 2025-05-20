# Discount API Documentation

## Overview

This document outlines the API endpoints for the Discount Service, including request parameters, expected responses, and error handling.

## API Endpoints

### 1. GET /api/discounts

Retrieves a list of discounts with optional filtering and pagination.

#### Query Parameters

| Parameter Name     | Data Type | Required | Description                      | Example Value |
|-------------------|-----------|----------|----------------------------------|---------------|
| `perPageDiscount` | integer   | No       | Number of discounts per page     | 10            |
| `pageDiscount`    | integer   | No       | Current page number              | 1             |
| `search`          | string    | No       | Search term for discount name    | "summer"      |
| `sortStartedAt`   | string    | No       | Sort by start date (asc/desc)    | "desc"        |
| `withCoupon`      | boolean   | No       | Include associated coupons       | true          |

#### Example Request
```
GET /api/discounts?perPageDiscount=10&pageDiscount=1&search=summer&sortStartedAt=desc&withCoupon=true
```

#### Response
```json
{
  "message": "Discounts retrieved successfully",
  "data": {
    "discounts": [
      {
        "id": 1,
        "name": "Summer Sale",
        "started_at": "2024-06-01T00:00:00.000000Z",
        "expired_at": "2024-08-31T23:59:59.000000Z",
        "type": "percentage",
        "value": 10,
        "usage_limit": 100,
        "trial_days": 0,
        "coupon": [
          {
            "id": 1,
            "times_used": 5,
            "discount_id": 1
          }
        ]
      }
    ],
    "totalPages": 5,
    "totalItems": 42,
    "currentPage": 1
  }
}
```

---

### 2. POST /api/discounts

Creates a new discount.

Some databases have a discounts table with a discount_month column.

#### Request Body

| Parameter Name   | Data Type | Required | Description              | Example Value |
|------------------|-----------|----------|--------------------------|---------------|
| `name`           | string    | Yes      | Discount name            | "Summer Sale" |
| `type`           | string    | Yes      | Discount type            | "percentage"  |
| `value`          | integer   | No      | Discount value           | 10            |
| `started_at`     | datetime  | No       | Start date and time      | null          |
| `expired_at`     | datetime  | No       | Expiration date and time | null          |
| `usage_limit`    | integer   | No       | Usage limit              | 100           |
| `trial_days`     | integer   | No       | Trial period in days     | 0             |
| `discount_month` | integer   | No       | Discount months          | 12            |

#### Example Request
```
POST /api/discount/create
{
  "name": "Summer Sale",
  "type": "percentage",
  "value": 10,
  "started_at": null,
  "expired_at": null,
  "usage_limit": 100,
  "trial_days": 0,
  "discount_month": 12
}
```

#### Response
```json
{
  "message": "Discount created successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale",
    "type": "percentage",
    "value": 10,
    "started_at": null,
    "expired_at": null,
    "usage_limit": 100,
    "trial_days": 0,
    "discount_month": 12,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T10:00:00.000000Z"
  }
}
```

---

### 3. PUT /api/discounts/{id}

Updates an existing discount.

Some databases have a discounts table with a discount_month column.

#### Path Parameters

| Parameter Name | Data Type | Required | Description   | Example Value |
|----------------|-----------|----------|---------------|---------------|
| `id`           | integer   | Yes      | Discount ID   | 1             |

#### Request Body

| Parameter Name   | Data Type | Required | Description              | Example Value |
|------------------|-----------|----------|--------------------------|---------------|
| `name`           | string    | No       | Discount name            | "Summer Sale" |
| `type`           | string    | No       | Discount type            | "percentage"  |
| `value`          | integer   | No       | Discount value           | 15            |
| `started_at`     | datetime  | No       | Start date and time      | null          |
| `expired_at`     | datetime  | No       | Expiration date and time | null          |
| `usage_limit`    | integer   | No       | Usage limit              | 200           |
| `trial_days`     | integer   | No       | Trial period in days     | 7             |
| `discount_month` | integer   | No       | Discount months          | 12            |

#### Example Request
```
PUT /api/discounts/1
{
  "name": "Summer Sale Extended",
  "value": 15,
  "usage_limit": 200,
  "trial_days": 7
}
```

#### Response
```json
{
  "message": "Discount updated successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale Extended",
    "type": "percentage",
    "value": 15,
    "started_at": null,
    "expired_at": null,
    "usage_limit": 200,
    "trial_days": 7,
    "discount_month": 12,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T10:30:00.000000Z"
  }
}
```

---

### 4. DELETE /api/discounts/{id}

Deletes a discount.

#### Path Parameters

| Parameter Name | Data Type | Required | Description   | Example Value |
|----------------|-----------|----------|---------------|---------------|
| `id`           | integer   | Yes      | Discount ID   | 1             |

#### Example Request
```
DELETE /api/discounts/1
```

#### Response
```json
{
  "message": "Discount deleted successfully"
}
```

---

### 5. GET /api/discounts/{id}

Retrieves a discount by its ID.

#### Path Parameters

| Parameter Name | Data Type | Required | Description   | Example Value |
|----------------|-----------|----------|---------------|---------------|
| `id`           | integer   | Yes      | Discount ID   | 1             |

#### Query Parameters

| Parameter Name | Data Type | Required | Description                   | Example Value |
|----------------|-----------|----------|-------------------------------|---------------|
| `withCoupon`   | boolean   | No       | Include associated coupons    | true          |

#### Example Request
```
GET /api/discounts/1?withCoupon=true
```

#### Response
```json
{
  "message": "Discount retrieved successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale",
    "type": "percentage",
    "value": 10,
    "started_at": "2024-06-01T00:00:00.000000Z",
    "expired_at": "2024-08-31T23:59:59.000000Z",
    "usage_limit": 100,
    "trial_days": 0,
    "discount_month": 12,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T10:00:00.000000Z",
    "coupon": [
      {
        "id": 1,
        "times_used": 5,
        "discount_id": 1
      }
    ]
  }
}
```

---

### 6. GET /api/discounts/id-and-name

Retrieves a list of all discount IDs and names.

#### Example Request
```
GET /api/discounts/id-and-name
```

#### Response
```json
{
  "message": "All discounts retrieved successfully",
  "discounts": [
    {
      "id": 1,
      "name": "Summer Sale"
    },
    {
      "id": 2,
      "name": "Winter Promotion"
    }
  ]
}
```

### Common Error Cases

- **404**: Resource not found
- **500**: Server error

