##
GET /api/discount/{id}

## Query Parameters
| Tên tham số | Kiểu dữ liệu | Bắt buộc | Mô tả | Giá trị mẫu |
|-------------|--------------|----------|-------|-------------|
| withCoupon | boolean | Không | Lấy kèm thông tin coupon | true |

## Response
Status Code: 200 (OK)
json()
{
    "data": {
        "id": 1,
        "name": "discount 1",
        "type": "percentage",
        "value": "10",
        "started_at": null,
        "expired_at": null,
        "usage_limit": null,
        "trial_days": "0",
        "discount_month": "12",
        "created_at": "2024-03-20T10:00:00.000000Z",
        "updated_at": "2024-03-20T10:00:00.000000Z",
        "coupons": [
            {
                "id": 1,
                "code": "DISCOUNT10",
                "status": "active"
            }
        ]
    }
}
