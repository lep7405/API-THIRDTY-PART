# Coupon API Documentation

## Overview
Documentation for Coupon Service API endpoints, including request parameters and expected responses.

## API Endpoints

### 1. GET /api/coupons
Retrieves a list of coupons with pagination and filtering options.

#### Query Parameters
| Parameter Name    | Data Type | Required | Description                          | Example Value |
|-------------------|-----------|----------|--------------------------------------|---------------|
| `perPageCoupon`   | integer   | No       | Number of records per page           | 5             |
| `page`            | integer   | No       | Current page number                  | 1             |
| `searchCoupon`    | string    | No       | Search keyword for coupon code       | "SUMMER"      |
| `status`          | string    | No       | Filter by status (0=inactive, 1=active) | "1"        |
| `timeUsed`        | string    | No       | Sort by times used (asc/desc)        | "desc"        |
| `discountId`      | integer   | No       | Filter by discount ID                | 123           |

#### Example Request
```
GET /api/coupons?perPageCoupon=10&page=1&searchCoupon=SUMMER&status=1&timeUsed=desc
```

#### Response Sample
```json
{
  "coupons": {
    "couponData": [
      {
        "id": 1,
        "code": "SUMMER2024",
        "shop": "example-shop.myshopify.com",
        "discount_id": 123,
        "automatic": 0,
        "times_used": 5,
        "status": 1,
        "created_at": "2024-06-01T10:00:00.000000Z",
        "updated_at": "2024-06-05T15:30:00.000000Z",
        "discount": {
          "id": 123,
          "name": "Summer Discount",
          "type": "percentage",
          "value": 10
        }
      }
    ],
    "totalPagesCoupon": 5,
    "totalItemsCoupon": 42,
    "currentPagesCoupon": 1
  }
}
```

---

### 2. GET /api/coupon/code/{code}
Retrieves a coupon by its code.

#### Path Parameters
| Parameter Name | Data Type | Required | Description     | Example Value |
|----------------|-----------|----------|-----------------|---------------|
| `code`         | string    | Yes      | Coupon code     | "SUMMER2024"  |

#### Example Request
```
GET /api/coupon/code/SUMMER2024
```

#### Response Sample
```json
{
  "coupon": {
    "id": 1,
    "code": "SUMMER2024",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 5,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-05T15:30:00.000000Z"
  }
}
```

---

### 3. GET /api/coupon/id/{id}
Retrieves a coupon by its ID.

#### Path Parameters
| Parameter Name | Data Type | Required | Description    | Example Value |
|----------------|-----------|----------|----------------|---------------|
| `id`           | integer   | Yes      | Coupon ID      | 1             |

#### Query Parameters
| Parameter Name  | Data Type | Required | Description                   | Example Value |
|-----------------|-----------|----------|-------------------------------|---------------|
| `withDiscount`  | boolean   | No       | Include discount information  | true          |

#### Example Request
```
GET /api/coupon/id/1?withDiscount=true
```

#### Response Sample
```json
{
  "coupon": {
    "id": 1,
    "code": "SUMMER2024",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 5,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-05T15:30:00.000000Z",
    "discount": {
      "id": 123,
      "name": "Summer Discount",
      "type": "percentage",
      "value": 10,
      "started_at": "2024-06-01T00:00:00.000000Z",
      "expired_at": "2024-08-31T23:59:59.000000Z"
    }
  }
}
```

---

### 4. POST /api/coupon/create
Creates a new coupon.

#### Request Body
| Parameter Name | Data Type | Required | Description            | Example Value |
|----------------|-----------|----------|------------------------|---------------|
| `code`         | string    | Yes      | Coupon code           | "SUMMER2024"  |
| `shop`         | string    | No       | Shop domain           | "example-shop.myshopify.com" |
| `discount_id`  | integer   | Yes      | Associated discount ID | 123           |
| `automatic`    | boolean/int | No     | Is coupon automatic   | 0             |

#### Example Request
```
POST /api/coupon/create
{
  "code": "SUMMER2024",
  "shop": "example-shop.myshopify.com",
  "discount_id": 123,
  "automatic": 0
}
```

#### Response Sample
```json
{
  "coupon": {
    "id": 1,
    "code": "SUMMER2024",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 0,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T10:00:00.000000Z"
  }
}
```

---

### 5. PUT /api/coupon/{id}
Updates an existing coupon.

#### Path Parameters
| Parameter Name | Data Type | Required | Description    | Example Value |
|----------------|-----------|----------|----------------|---------------|
| `id`           | integer   | Yes      | Coupon ID      | 1             |

#### Request Body
| Parameter Name | Data Type | Required | Description            | Example Value |
|----------------|-----------|----------|------------------------|---------------|
| `code`         | string    | No       | Coupon code           | "SUMMER2024"  |
| `shop`         | string    | No       | Shop domain           | "example-shop.myshopify.com" |
| `discount_id`  | integer   | No       | Associated discount ID | 123           |
| `automatic`    | boolean/int | No     | Is coupon automatic   | 0             |

#### Example Request
```
PUT /api/coupon/1
{
  "code": "SUMMER2024_UPDATED",
  "shop": "example-shop.myshopify.com",
  "discount_id": 123,
  "automatic": 1
}
```

#### Response Sample
```json
{
  "coupon": {
    "id": 1,
    "code": "SUMMER2024_UPDATED",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 1,
    "times_used": 0,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T11:30:00.000000Z"
  }
}
```

---

### 6. PUT /api/coupon/{id}/status
Changes the status of a coupon (active/inactive).

#### Path Parameters
| Parameter Name | Data Type | Required | Description    | Example Value |
|----------------|-----------|----------|----------------|---------------|
| `id`           | integer   | Yes      | Coupon ID      | 1             |

#### Example Request
```
PUT /api/coupon/1/status
```

#### Response Sample
```json
{
  "success": true,
  "message": "Coupon status changed successfully"
}
```

---

### 7. PUT /api/coupon/{id}/decrement-times-used/{numDecrement}
Decrements the times used counter for a coupon.

#### Path Parameters
| Parameter Name  | Data Type | Required | Description              | Example Value |
|-----------------|-----------|----------|--------------------------|---------------|
| `id`            | integer   | Yes      | Coupon ID                | 1             |
| `numDecrement`  | integer   | Yes      | Number to decrement by   | 2             |

#### Example Request
```
PUT /api/coupon/1/decrement-times-used/2
```

#### Response Sample
```json
{
  "success": true,
  "message": "Times used decremented successfully"
}
```

---

### 8. DELETE /api/coupon/{id}
Deletes a coupon.

#### Path Parameters
| Parameter Name | Data Type | Required | Description    | Example Value |
|----------------|-----------|----------|----------------|---------------|
| `id`           | integer   | Yes      | Coupon ID      | 1             |

#### Example Request
```
DELETE /api/coupon/1
```

#### Response Sample
```json
{
  "success": true,
  "message": "Coupon deleted successfully"
}
```

---

### 9. POST /api/coupon
Creates a coupon associated with a discount.

#### Request Body
| Parameter Name | Data Type | Required | Description            | Example Value |
|----------------|-----------|----------|------------------------|---------------|
| `code`         | string    | Yes      | Coupon code           | "SUMMER2024"  |
| `shop`         | string    | No       | Shop domain           | "example-shop.myshopify.com" |
| `discount_id`  | integer   | Yes      | Associated discount ID | 123           |
| `automatic`    | boolean/int | No     | Is coupon automatic   | 0             |

#### Example Request
```
POST /api/coupon
{
  "code": "SUMMER2024",
  "shop": "example-shop.myshopify.com",
  "discount_id": 123,
  "automatic": 0
}
```

#### Response Sample
```json
{
  "coupon": {
    "id": 1,
    "code": "SUMMER2024",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 0,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T10:00:00.000000Z"
  }
}
```

## Error Responses

All API endpoints return standardized error responses:

```json
{
  "error": true,
  "message": "Error message details"
}
```

### Common Error Codes
- **400** - Bad Request (invalid parameters)
- **404** - Resource Not Found
- **409** - Conflict (e.g., coupon code already exists)
- **422** - Validation Error
- **500** - Internal Server Error
