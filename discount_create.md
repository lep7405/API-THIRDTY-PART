
## Endpoint 
Ví dụ
POST url/api/discounts

---

## Request Body (POST)
| Tên tham số | Kiểu dữ liệu | Bắt buộc | Mô tả | Giá trị mẫu |
|------------------|--------------|----------|------------------------------------------------|-------------|
| name | string | Có | Tên của discount | "discount 1"|
| type | string | Có | Loại discount (percentage/fixed) | "percentage"|
| started_at | datetime | Không | Thời gian bắt đầu | null |
| expired_at | datetime | Không | Thời gian kết thúc | null |
| usage_limit | integer | Không | Giới hạn số lần sử dụng | null |
| value | integer | Có | Giá trị discount | 10 |
| trial_days | integer | Có | Số ngày dùng thử | 0 |
| discount_month | integer | Có | Số tháng áp dụng discount | 12 |

---

## Ví dụ query gửi lên

POST /api/discounts
{
    "name": "discount 1",
    "type": "percentage",
    "started_at": null,
    "expired_at": null,
    "usage_limit": null,
    "value": 10,
    "trial_days": 0,
    "discount_month": 12
}

## Response mẫu
Status code : 201
json()
{
    "message": "Discount created successfully",
    "discount": {
        "id": 1,
        "name": "discount 1",
        "type": "percentage",
        "value": 10,
        "started_at": null,
        "expired_at": null,
        "usage_limit": null,
        "trial_days": 0,
        "discount_month": 12,
        "created_at": "2024-03-20T10:00:00.000000Z",
        "updated_at": "2024-03-20T10:00:00.000000Z"
    }
}
