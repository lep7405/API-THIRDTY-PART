
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
| value | string | Có | Giá trị discount | "10" |
| trial_days | string | Có | Số ngày dùng thử | "0" |
| discount_month | string | Có | Số tháng áp dụng discount | "12" |

---

## Ví dụ query gửi lên

POST /api/discounts
{
    "name": "discount 1",
    "type": "percentage",
    "started_at": null,
    "expired_at": null,
    "usage_limit": null,
    "value": "10",
    "trial_days": "0",
    "discount_month": "12"
}
