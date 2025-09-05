# Coupon API Documentation

## Overview
Tài liệu này mô tả chi tiết các API endpoint cho dịch vụ Coupon, bao gồm các tham số yêu cầu và phản hồi mong đợi.

## API Endpoints

### 1. GET /coupons
Lấy danh sách các coupon với tùy chọn phân trang và lọc.

#### Tham số Query
| Tên           | Kiểu    | Bắt buộc | Mô tả                            | Giá trị mẫu  |
|---------------|---------|----------|----------------------------------|--------------|
| `perPageCoupon` | integer | Không    | Số lượng bản ghi mỗi trang       | 10           |
| `pageCoupon`        | integer | Không    | Số trang hiện tại                | 1            |
| `searchCoupon` | string  | Không    | Tìm kiếm theo nhiều trường          | "SUMMER"     |
| `status`      | string  | Không    | Lọc theo trạng thái (0=không hoạt động, 1=hoạt động) | "1"  |
| `timeUsed`    | string  | Không    | Sắp xếp theo lượt sử dụng (asc/desc) | "desc"     |
| `discountId`  | integer | Không    |              | 123          |

#### Ví dụ Request
```
GET /coupons?perPageCoupon=10&pageCoupon=1&searchCoupon=SUMMER&status=1&timeUsed=desc
```

#### Ví dụ Response
```json
{
  "data": {
    "coupons": [
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
          "name": "Summer Discount"
        }
      }
    ],
    "totalPages": 5,
    "totalItems": 42,
    "currentPages": 1
  }
}
```

---

### 2. POST /coupons
Tạo mới một coupon.

#### Request Body
| Tên           | Kiểu      | Bắt buộc | Mô tả                      | Giá trị mẫu                |
|---------------|-----------|----------|----------------------------|----------------------------|
| `code`        | string    | Có       | Mã coupon                  | "SUMMER2024"              |
| `shop`        | string    | Không    | Tên miền shop               | "example-shop.myshopify.com" |
| `discount_id` | integer   | Có       |    | 123                        |
| `automatic`   | boolean | Không   |    | 0                          |
| `status`   | boolean | Không   |    | 1                          |
| `times_used`   | integer | Không   | Số lần dùng coupon   | 0                          |

#### Ví dụ Request
```
POST /coupons
{
  "code": "SUMMER2024",
  "shop": "example-shop.myshopify.com",
  "discount_id": 123,
  "automatic": 0
}
```

#### Ví dụ Response
```json
{
  "message": "Coupon created successfully",
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

### 3. GET /coupons/{id}
Lấy thông tin coupon theo ID.

#### Tham số Path
| Tên  | Kiểu    | Bắt buộc | Mô tả     | Giá trị mẫu |
|------|---------|----------|-----------|-------------|
| `id` | integer | Có       | ID coupon | 1           |

#### Tham số Query
| Tên           | Kiểu    | Bắt buộc | Mô tả                        | Giá trị mẫu |
|---------------|---------|----------|------------------------------|-------------|
| `withDiscount`| boolean | Không    | Bao gồm thông tin discount   | true        |

#### Ví dụ Request
```
GET /coupons/id/1?withDiscount=true
```

#### Ví dụ Response
```json
{
  "message": "Coupon retrieved successfully",
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
      "name": "Summer Discount"
    }
  }
}
```

---

### 4. GET /coupons/code/{code}
Lấy thông tin coupon theo mã code.

#### Tham số Path
| Tên    | Kiểu   | Bắt buộc | Mô tả     | Giá trị mẫu  |
|--------|--------|----------|-----------|--------------|
| `code` | string | Có       | Mã coupon | "SUMMER2024" |

#### Ví dụ Request
```
GET /coupons/code/SUMMER2024
```

#### Ví dụ Response
```json
{
  "message": "Coupon retrieved successfully",
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

### 5. GET /coupons/discount/{id}/shop/{shop}
Lấy coupon theo ID giảm giá và shop.

#### Tham số Path
| Tên    | Kiểu    | Bắt buộc | Mô tả          | Giá trị mẫu                |
|--------|---------|----------|----------------|----------------------------|
| `id`   | integer | Có       | ID giảm giá    | 123                        |
| `shop` | string  | Có       | Tên miền shop  | "example-shop.myshopify.com" |

#### Ví dụ Request
```
GET /coupons/discount/123/shop/example-shop.myshopify.com/
```

#### Ví dụ Response
```json
{
  "message": "Coupons retrieved successfully",
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

### 6. GET /coupons/discount/{id}
Lấy coupon theo discount ID và có mã bắt đầu bằng "GENAUTO".

#### Tham số Path
| Tên  | Kiểu    | Bắt buộc | Mô tả       | Giá trị mẫu |
|------|---------|----------|-------------|-------------|
| `id` | integer | Có       | ID giảm giá | 123         |

#### Ví dụ Request
```
GET /coupons/discount/123
```

#### Ví dụ Response
```json
{
  "message": "Coupons retrieved successfully",
  "coupon": [
    {
      "id": 1,
      "code": "GENAUTO2024",
      "shop": "example-shop.myshopify.com",
      "discount_id": 123,
      "automatic": 1,
      "times_used": 5,
      "status": 1,
      "created_at": "2024-06-01T10:00:00.000000Z",
      "updated_at": "2024-06-05T15:30:00.000000Z"
    }
  ]
}
```

---

### 7. PUT /coupons/{id}
Cập nhật thông tin coupon.

#### Tham số Path
| Tên  | Kiểu    | Bắt buộc | Mô tả     | Giá trị mẫu |
|------|---------|----------|-----------|-------------|
| `id` | integer | Có       | ID coupon | 1           |

#### Request Body
| Tên           | Kiểu    | Bắt buộc | Mô tả                    | Giá trị mẫu                |
|---------------|---------|----------|--------------------------|----------------------------|
| `code`        | string  | Có    | Mã coupon                | "SUMMER2024_UPDATED"      |
| `shop`        | string  | Không    | Tên miền shop            | "example-shop.myshopify.com" |
| `discount_id` | integer | Có    | ID của discount liên kết | 123                        |

#### Ví dụ Request
```
PUT /coupons/1
{
  "code": "SUMMER2024_UPDATED",
  "shop": "example-shop.myshopify.com",
  "discount_id": 123
}
```

#### Ví dụ Response
```json
{
  "message": "Coupon updated successfully",
  "coupon": {
    "id": 1,
    "code": "SUMMER2024_UPDATED",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 5,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-01T11:30:00.000000Z"
  }
}
```

---

### 8. PUT /coupons/{id}/status
Thay đổi trạng thái của coupon (hoạt động/không hoạt động).

#### Tham số Path
| Tên  | Kiểu    | Bắt buộc | Mô tả     | Giá trị mẫu |
|------|---------|----------|-----------|-------------|
| `id` | integer | Có       | ID coupon | 1           |

#### Ví dụ Request
```
PUT /coupons/1/status
```

#### Ví dụ Response
```json
{
  "message": "Coupon status updated successfully",
  "coupon": {
    "id": 1,
    "code": "SUMMER2024",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 5,
    "status": 0,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-05T15:30:00.000000Z"
  }
}
```

---

### 9. PUT /coupons/{id}/decre-times-used
Giảm số lượt đã sử dụng của coupon.

#### Tham số Path
| Tên  | Kiểu    | Bắt buộc | Mô tả     | Giá trị mẫu |
|------|---------|----------|-----------|-------------|
| `id` | integer | Có       | ID coupon | 1           |

#### Request Body
| Tên           | Kiểu    | Bắt buộc | Mô tả              | Giá trị mẫu |
|---------------|---------|----------|--------------------|-------------|
| `numDecrement`| integer | Có       | Số lượng muốn giảm | 2           |

#### Ví dụ Request
```
PUT /coupons/1/decre-times-used
{
  "numDecrement": 2
}
```

#### Ví dụ Response
```json
{
  "message": "Coupon times used decremented successfully",
  "coupon": {
    "id": 1,
    "code": "SUMMER2024",
    "shop": "example-shop.myshopify.com",
    "discount_id": 123,
    "automatic": 0,
    "times_used": 3,
    "status": 1,
    "created_at": "2024-06-01T10:00:00.000000Z",
    "updated_at": "2024-06-05T15:30:00.000000Z"
  }
}
```

---

### 10. DELETE /coupons/{id}
Xóa một coupon.

#### Tham số Path
| Tên  | Kiểu    | Bắt buộc | Mô tả     | Giá trị mẫu |
|------|---------|----------|-----------|-------------|
| `id` | integer | Có       | ID coupon | 1           |

#### Ví dụ Request
```
DELETE /coupons/1
```

#### Ví dụ Response
```json
{
  "message": "Coupon deleted successfully"
}
```


### Mã lỗi phổ biến
- **400** - Bad Request (tham số không hợp lệ)
- **404** - Resource Not Found (không tìm thấy tài nguyên)
- **422** - Validation Error (lỗi xác thực)
- **500** - Internal Server Error (lỗi máy chủ)
