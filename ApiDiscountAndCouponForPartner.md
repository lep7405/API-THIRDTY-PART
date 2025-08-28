### 9. POST /discounts/affiliate-partners
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
POST /discounts/affiliate-partners
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
