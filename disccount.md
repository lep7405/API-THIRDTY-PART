# Tài liệu API Discount

## Tổng quan
Tài liệu này mô tả các API endpoint của dịch vụ Discount, bao gồm các tham số yêu cầu và phản hồi mong đợi.

## API Endpoints

### 1. GET /api/discounts
Lấy danh sách các discount với tùy chọn phân trang và lọc.

#### Tham số Query
| Tên              | Kiểu     | Bắt buộc | Mô tả                         | Giá trị mẫu |
|------------------|----------|----------|-------------------------------|-------------|
| `perPageDiscount`| integer  | Không    | Số lượng bản ghi mỗi trang    | 10          |
| `pageDiscount`   | integer  | Không    | Số trang hiện tại             | 1           |
| `searchDiscount` | string   | Không    | Tìm kiếm theo tên discount    | "summer"    |
| `sortStartedAt`  | string   | Không    | Sắp xếp theo ngày bắt đầu (asc/desc) | "desc" |
| `withCoupon`     | boolean  | Không    | Bao gồm thông tin coupon      | true        |

#### Ví dụ Request
```
GET /api/discounts?perPageDiscount=10&pageDiscount=1&searchDiscount=summer&sortStartedAt=desc&withCoupon=true
```

#### Ví dụ Response
```json
{
  "message": "Discounts retrieved successfully",
  "data": {
    "discounts": [
      {
        "id": 1,
        "name": "Summer Sale 2024",
        "type": "percentage",
        "value": 15,
        "started_at": "2024-06-01T00:00:00.000000Z",
        "expired_at": "2024-08-31T23:59:59.000000Z",
        "usage_limit": 100,
        "trial_days": 7,
        "discount_month": 12,
        "coupon": [
          {
            "id": 1,
            "times_used": 5,
            "discount_id": 1
          }
        ],
        "created_at": "2024-05-15T10:00:00.000000Z",
        "updated_at": "2024-05-15T10:00:00.000000Z"
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
Tạo mới một discount.
-affiliate với freegifts_new có cột discount_month

#### Request Body
| Tên             | Kiểu      | Bắt buộc | Mô tả                      | Giá trị mẫu      |
|-----------------|-----------|----------|----------------------------|------------------|
| `name`          | string    | Có       | Tên của discount           | "Summer Sale 2024" |
| `type`          | string    | Có       | Loại discount              | "percentage"     |
| `value`         | integer   | Không    | Giá trị discount           | 15               |
| `started_at`    | datetime  | Không    | Thời gian bắt đầu          | "2024-06-01T00:00:00" |
| `expired_at`    | datetime  | Không    | Thời gian kết thúc         | "2024-08-31T23:59:59" |
| `usage_limit`   | integer   | Không    | Giới hạn sử dụng           | 100              |
| `trial_days`    | integer   | Không    | Số ngày dùng thử           | 7                |
| `discount_month`| integer   | Không    | Số tháng áp dụng discount  | 12               |

#### Ví dụ Request
```
POST /api/discounts
{
  "name": "Summer Sale 2024",
  "type": "percentage",
  "value": 15,
  "started_at": "2024-06-01T00:00:00",
  "expired_at": "2024-08-31T23:59:59",
  "usage_limit": 100,
  "trial_days": 7,
  "discount_month": 12
}
```

#### Ví dụ Response
```json
{
  "message": "Discount created successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale 2024",
    "type": "percentage",
    "value": 15,
    "started_at": "2024-06-01T00:00:00.000000Z",
    "expired_at": "2024-08-31T23:59:59.000000Z",
    "usage_limit": 100,
    "trial_days": 7,
    "discount_month": 12,
    "created_at": "2024-05-15T10:00:00.000000Z",
    "updated_at": "2024-05-15T10:00:00.000000Z"
  }
}
```

---

### 3. PUT /api/discounts/{id}
Cập nhật thông tin discount.

#### Tham số Path
| Tên  | Kiểu     | Bắt buộc | Mô tả       | Giá trị mẫu |
|------|----------|----------|-------------|-------------|
| `id` | integer  | Có       | ID discount | 1           |

#### Request Body
| Tên             | Kiểu      | Bắt buộc | Mô tả                      | Giá trị mẫu      |
|-----------------|-----------|----------|----------------------------|------------------|
| `name`          | string    | Không    | Tên của discount           | "Summer Sale 2024 Updated" |
| `started_at`    | datetime  | Không    | Thời gian bắt đầu          | "2024-06-01T00:00:00" |
| `expired_at`    | datetime  | Không    | Thời gian kết thúc         | "2024-09-30T23:59:59" |
| `value`         | integer   | Không    | Giá trị discount           | 20               |
| `usage_limit`   | integer   | Không    | Giới hạn sử dụng           | 200              |
| `trial_days`    | integer   | Không    | Số ngày dùng thử           | 14               |
| `discount_month`| integer   | Không    | Số tháng áp dụng discount  | 12               |

#### Ví dụ Request
```
PUT /api/discounts/1
{
  "name": "Summer Sale 2024 Updated",
  "value": 20,
  "expired_at": "2024-09-30T23:59:59",
  "usage_limit": 200,
  "trial_days": 14
}
```

#### Ví dụ Response
```json
{
  "message": "Discount updated successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale 2024 Updated",
    "type": "percentage",
    "value": 20,
    "started_at": "2024-06-01T00:00:00.000000Z",
    "expired_at": "2024-09-30T23:59:59.000000Z",
    "usage_limit": 200,
    "trial_days": 14,
    "discount_month": 12,
    "created_at": "2024-05-15T10:00:00.000000Z",
    "updated_at": "2024-05-20T15:30:00.000000Z"
  }
}
```

---

### 4. DELETE /api/discounts/{id}
Xóa một discount.

#### Tham số Path
| Tên  | Kiểu     | Bắt buộc | Mô tả       | Giá trị mẫu |
|------|----------|----------|-------------|-------------|
| `id` | integer  | Có       | ID discount | 1           |

#### Ví dụ Request
```
DELETE /api/discounts/1
```

#### Ví dụ Response
```json
{
  "message": "Discount deleted successfully"
}
```

---

### 5. GET /api/discounts/{id}
Lấy thông tin chi tiết của discount theo ID.

#### Tham số Path
| Tên  | Kiểu     | Bắt buộc | Mô tả       | Giá trị mẫu |
|------|----------|----------|-------------|-------------|
| `id` | integer  | Có       | ID discount | 1           |

#### Tham số Query
| Tên          | Kiểu    | Bắt buộc | Mô tả                       | Giá trị mẫu |
|--------------|---------|----------|----------------------------|-------------|
| `withCoupon`| boolean | Không    | Bao gồm thông tin coupons  | true        |

#### Ví dụ Request
```
GET /api/discounts/1?withCoupon=true
```

#### Ví dụ Response
```json
{
  "message": "Discount retrieved successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale 2024",
    "type": "percentage",
    "value": 15,
    "started_at": "2024-06-01T00:00:00.000000Z",
    "expired_at": "2024-08-31T23:59:59.000000Z",
    "usage_limit": 100,
    "trial_days": 7,
    "discount_month": 12,
    "created_at": "2024-05-15T10:00:00.000000Z",
    "updated_at": "2024-05-15T10:00:00.000000Z",
    "coupons": [
      {
        "id": 1,
        "times_used": "1",
        "discount_id": 1
      }
    ]
  }
}
```

---

### 6. GET /api/discounts/with-coupons
Lấy danh sách các discount id kèm theo thông tin coupon liên quan.

#### Ví dụ Request
```
GET /api/discounts/with-coupons
```

#### Ví dụ Response
```json
{
  "message": "All discounts retrieved successfully",
  "discounts": [
    {
      "id": 1,
      "coupon": [
        {
          "id": 1,
          "times_used": 5,
          "discount_id": 1
        }
      ]
    },
    {
      "id": 2,
      "coupon": [
        {
          "id": 2,
          "times_used": 3,
          "discount_id": 2
        }
      ]
    }
  ]
}
```

---

### 7. GET /api/discounts/id-and-name
Lấy danh sách ID và tên của tất cả discount.

#### Ví dụ Request
```
GET /api/discounts/id-and-name
```

#### Ví dụ Response
```json
{
  "message": "Discount IDs and names retrieved successfully",
  "discounts": [
    {
      "id": 1,
      "name": "Summer Sale 2024"
    },
    {
      "id": 2,
      "name": "Winter Promotion 2024"
    }
  ]
}
```

---

### 8. POST /api/discounts/find-by-ids
Lấy thông tin các discount theo danh sách ID.

#### Request Body
| Tên    | Kiểu       | Bắt buộc | Mô tả                    | Giá trị mẫu   |
|--------|------------|----------|--------------------------|---------------|
| `ids`  | array     | Có       | Mảng các ID discount     | [1, 2, 3]     |

#### Ví dụ Request
```
POST /api/discounts/find-by-ids
{
  "ids": [1, 2, 3]
}
```

#### Ví dụ Response
```json
{
  "message": "Discounts retrieved successfully",
  "discounts": [
    {
      "id": 1,
      "name": "Summer Sale 2024",
      "type": "percentage",
      "value": 15
    },
    {
      "id": 2,
      "name": "Winter Promotion 2024",
      "type": "percentage",
      "value": 20
    },
    {
      "id": 3,
      "name": "Spring Special",
      "type": "fixed_amount",
      "value": 5000
    }
  ]
}
```

---

### 9. POST /api/discounts/affiliate-partners
Tạo mới hoặc cập nhật giảm giá (discount) dựa trên các thuộc tính được cung cấp.

#### Request Body
| Tên           | Kiểu    | Bắt buộc | Mô tả                      | Giá trị mẫu     |
|---------------|---------|----------|----------------------------|-----------------|
| `name`        | string  | Có       | Tên của discount           | "Summer Sale"   |
| `type`        | string  | Có       | Loại discount (luôn là percentage) | "percentage" |
| `value`       | integer | Có       | Giá trị discount (%)       | 10              |
| `trial_days`  | integer | Có       | Số ngày dùng thử           | 7               |

#### Ví dụ Request
```
POST /api/discounts/affiliate-partners
{
  "name": "Summer Sale",
  "type": "percentage",
  "value": 10,
  "trial_days": 7
}
```

#### Ví dụ Response
```json
{
  "message": "Discount created successfully",
  "discount": {
    "id": 1,
    "name": "Summer Sale",
    "type": "percentage",
    "value": 10,
    "usage_limit": 1,
    "trial_days": 7,
    "updated_at": "2024-06-15T10:00:00.000000Z",
    "created_at": "2024-06-15T10:00:00.000000Z"
  }
}
```

### Mã lỗi phổ biến
- **400** - Bad Request (tham số không hợp lệ)
- **404** - Not Found (không tìm thấy tài nguyên)
- **422** - Validation Error (lỗi xác thực)
- **500** - Internal Server Error (lỗi máy chủ)
