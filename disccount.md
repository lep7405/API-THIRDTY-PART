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

### 2. GET /api/discounts/with-coupons
Lấy danh sách các discount kèm theo thông tin coupon liên quan.

#### Tham số Query
| Tên              | Kiểu     | Bắt buộc | Mô tả                         | Giá trị mẫu |
|------------------|----------|----------|-------------------------------|-------------|
| `perPageDiscount`| integer  | Không    | Số lượng bản ghi mỗi trang    | 10          |
| `pageDiscount`   | integer  | Không    | Số trang hiện tại             | 1           |
| `search`         | string   | Không    | Tìm kiếm theo tên discount    | "summer"    |

#### Ví dụ Request
```
GET /api/discounts/with-coupons?perPageDiscount=10&pageDiscount=1&search=summer
```

#### Ví dụ Response
```json
{
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
        "coupons": [
          {
            "id": 1,
            "code": "SUMMER2024",
            "shop": "example-shop.myshopify.com",
            "times_used": 5,
            "status": 1
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

### 3. POST /api/discounts
Tạo mới một discount.

#### Request Body
| Tên             | Kiểu      | Bắt buộc | Mô tả                      | Giá trị mẫu      |
|-----------------|-----------|----------|----------------------------|------------------|
| `name`          | string    | Có       | Tên của discount           | "Summer Sale 2024" |
| `type`          | string    | Có       | Loại discount              | "percentage"     |
| `value`         | integer   | Có       | Giá trị discount           | 15               |
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

### 4. GET /api/discounts/id-and-name
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

### 5. POST /api/discounts/affiliate-partners
Tạo mới hoặc cập nhật discount trong hệ thống đối tác liên kết.

#### Request Body
| Tên             | Kiểu      | Bắt buộc | Mô tả                      | Giá trị mẫu      |
|-----------------|-----------|----------|----------------------------|------------------|
| `discount_id`   | integer   | Có       | ID của discount            | 1                |
| `partner_id`    | integer   | Có       | ID của đối tác             | 10               |
| `commission`    | number    | Không    | Tỷ lệ hoa hồng             | 10.5             |
| `status`        | integer   | Không    | Trạng thái                 | 1                |

#### Ví dụ Request
```
POST /api/discounts/affiliate-partners
{
  "discount_id": 1,
  "partner_id": 10,
  "commission": 10.5,
  "status": 1
}
```

#### Ví dụ Response
```json
{
  "message": "Discount updated or created in affiliate partner system",
  "data": {
    "discount_id": 1,
    "partner_id": 10,
    "commission": 10.5,
    "status": 1
  }
}
```

---

### 6. GET /api/discounts/{id}
Lấy thông tin chi tiết của discount theo ID.

#### Tham số Path
| Tên  | Kiểu     | Bắt buộc | Mô tả       | Giá trị mẫu |
|------|----------|----------|-------------|-------------|
| `id` | integer  | Có       | ID discount | 1           |

#### Tham số Query
| Tên          | Kiểu    | Bắt buộc | Mô tả                       | Giá trị mẫu |
|--------------|---------|----------|----------------------------|-------------|
| `withCoupons`| boolean | Không    | Bao gồm thông tin coupons  | true        |

#### Ví dụ Request
```
GET /api/discounts/1?withCoupons=true
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
        "code": "SUMMER2024",
        "shop": "example-shop.myshopify.com",
        "times_used": 5,
        "status": 1
      }
    ]
  }
}
```

---

### 7. POST /api/discounts/find-by-ids
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

### 8. PUT /api/discounts/{id}
Cập nhật thông tin discount.

#### Tham số Path
| Tên  | Kiểu     | Bắt buộc | Mô tả       | Giá trị mẫu |
|------|----------|----------|-------------|-------------|
| `id` | integer  | Có       | ID discount | 1           |

#### Request Body
| Tên             | Kiểu      | Bắt buộc | Mô tả                      | Giá trị mẫu      |
|-----------------|-----------|----------|----------------------------|------------------|
| `name`          | string    | Không    | Tên của discount           | "Summer Sale 2024 Updated" |
| `type`          | string    | Không    | Loại discount              | "percentage"     |
| `value`         | integer   | Không    | Giá trị discount           | 20               |
| `started_at`    | datetime  | Không    | Thời gian bắt đầu          | "2024-06-01T00:00:00" |
| `expired_at`    | datetime  | Không    | Thời gian kết thúc         | "2024-09-30T23:59:59" |
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

### 9. DELETE /api/discounts/{id}
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

### Mã lỗi phổ biến
- **400** - Bad Request (tham số không hợp lệ)
- **404** - Not Found (không tìm thấy tài nguyên)
- **422** - Validation Error (lỗi xác thực)
- **500** - Internal Server Error (lỗi máy chủ)
