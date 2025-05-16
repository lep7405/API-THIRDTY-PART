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
  "discounts": {
    "discountData": [
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
    "totalPagesDiscount": 5,
    "totalItemsDiscount": 42,
    "currentPagesDiscount": 1
  }
}
```

---

### 2. POST /api/discount/create

Creates a new discount.

#### Request Body

| Parameter Name   | Data Type | Required | Description              | Example Value |
|------------------|-----------|----------|--------------------------|---------------|
| `name`           | string    | Yes      | Discount name            | "Summer Sale" |
| `type`           | string    | Yes      | Discount type            | "percentage"  |
| `value`          | integer   | Yes      | Discount value           | 10            |
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

### 3. PUT /api/discount/{id}

Updates an existing discount.

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
| `discount_for_x_month` | string | No    | Enable monthly discount  | "1"           |

#### Example Request
```
PUT /api/discount/1
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

### 4. DELETE /api/discount/{id}

Deletes a discount.

#### Path Parameters

| Parameter Name | Data Type | Required | Description   | Example Value |
|----------------|-----------|----------|---------------|---------------|
| `id`           | integer   | Yes      | Discount ID   | 1             |

#### Example Request
```
DELETE /api/discount/1
```

#### Response
```json
{
  "message": "Discount deleted successfully"
}
```

---

### 5. GET /api/discount/{id}

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
GET /api/discount/1?withCoupon=true
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

## Error Handling

All endpoints return standardized error responses with appropriate HTTP status codes:

```json
{
  "error": true,
  "message": "The discount service is currently unavailable. Please try again later."
}
```

### Common Error Cases

- **404**: Resource not found
- **409**: Conflict
- **422**: Validation error (invalid data)
- **500**: Server error

## Business Rules

1. Discounts with associated coupons that have been used cannot be deleted
2. Special database types may have unique handling for monthly discount settings
3. When updating a discount, validation rules depend on whether associated coupons have been used
