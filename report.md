# Report API Documentation

## Overview

This document outlines the API endpoint for retrieving report data, including discount and coupon statistics.

## API Endpoints

### 1. GET /api/reports

Retrieves comprehensive reporting data including discount statistics, coupon usage metrics, and summary data.

#### Query Parameters

| Parameter Name     | Data Type | Required | Description                      | Example Value |
|-------------------|-----------|----------|----------------------------------|---------------|
| `perPageDiscount` | integer   | No       | Discounts per page               | 5             |
| `pageDiscount`    | integer   | No       | Current discount page            | 1             |
| `searchDiscount`  | string    | No       | Search term for discounts        | "summer"      |
| `perPageCoupon`   | integer   | No       | Coupons per page                 | 5             |
| `pageCoupon`      | integer   | No       | Current coupon page              | 1             |
| `searchCoupon`    | string    | No       | Search term for coupons          | "SALE50"      |
| `status`          | string    | No       | Filter by coupon status (0/1)    | "1"           |
| `timesUsed`       | string    | No       | Sort by usage (asc/desc)         | "desc"        |

#### Example Request
```
GET /api/reports?perPageDiscount=10&pageDiscount=1&perPageCoupon=10&pageCoupon=1
```

#### Response
```json
{
  "message": "Report retrieved successfully",
  "report": {
    "discountData": [
      {
        "id": 1,
        "name": "Summer Sale",
        "type": "percentage",
        "value": 10,
        "started_at": "2024-06-01T00:00:00.000000Z",
        "expired_at": "2024-08-31T23:59:59.000000Z",
        "coupon": [
          {
            "id": 1,
            "times_used": 5,
            "discount_id": 1
          }
        ],
        "totalCoupon": 1,
        "totalCouponUsed": 1
      }
    ],
    "totalPagesDiscount": 5,
    "totalItemsDiscount": 42,
    "currentPageDiscount": 1,
    
    "couponData": [
      {
        "id": 1,
        "code": "SUMMER2024",
        "discount_id": 1,
        "times_used": 5,
        "status": 1
      }
    ],
    "totalPagesCoupon": 3,
    "totalItemsCoupon": 25,
    "currentPageCoupon": 1,
    
    "countDiscount": 42,
    "countDiscountUsed": 15,
    "countCoupon": 25,
    "countCouponUsed": 18
  }
}
```

### Common Error Cases

- **404**: Resource not found
- **500**: Server error

