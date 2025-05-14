## Endpoint , chú ý cái tham số withCoupon nữa 

GET url/api/discounts

---

## Query Parameters

| Tên tham số       | Kiểu dữ liệu | Bắt buộc | Mô tả                                          | Giá trị mẫu |
|-------------------|--------------|----------|------------------------------------------------|-------------|
| perPageDiscount   | integer      | Không    | Số lượng bản ghi trên 1 trang (phân trang)     | 10          |
| pageDiscount      | integer      | Không    | Trang hiện tại                                 | 1           |
| search            | string       | Không    | Từ khóa tìm kiếm                               | "sale"      |
| sortStartedAt     | string       | Không    | Sắp xếp theo trường `started_at` (asc/desc)    | "desc"      |
| withCoupon        |boolean/int   | Không    | Lấy kèm thông tin coupon                       | true        |

---

## Ví dụ query gửi lên

GET url/api/discounts?perPageDiscount=10&pageDiscount=2&search=sale&sortStartedAt=desc&withCoupon=1

## Response mẫu
discounts thuộc Illuminate\AwareLenth\Paginator
```jsx
{
    "message": "Discounts retrieved successfully",
    "discounts": $discounts
}
```
![image](https://github.com/user-attachments/assets/0d791854-dfc2-4916-a409-55cb0fd274e0)

## Logic hiện tại 

```jsx
$query = $this->getModel();

if ($withCoupon) {
    $query = $query->with(['coupon' => function ($query) {
        $query->select('id', 'times_used', 'discount_id');
    }]);
}

if ($search) {
    $query = $query->where(function ($sub) use ($search) {
        $escapedSearch = addcslashes($search, '%_');
        $sub->where('name', 'like', "%{$escapedSearch}%")
            ->orWhere('started_at', 'like', "%{$escapedSearch}%")
            ->orWhere('expired_at', 'like', "%{$escapedSearch}%");

        if (is_numeric($search)) {
            $sub->orWhere('id', $search);
        }

        $sub->orWhere(function ($q) use ($search) {
            $q->where(function ($inner) use ($search) {
                $inner->where('type', 'percentage')
                      ->where('value', str_replace('%', '', $search));
            })
            ->orWhere(function ($inner) use ($search) {
                $inner->where('type', 'amount')
                      ->where('value', $search);
            });
        });
    });
}

$sortStartedAt = strtolower($sortStartedAt);
if ($sortStartedAt && in_array($sortStartedAt, ['asc', 'desc'])) {
    $query = $query->orderBy('started_at', $sortStartedAt);
}

$query = $query->orderBy('id', 'desc');

$result = $query->paginate(
    $perPage,
    ['id','name','started_at','expired_at','type','value','usage_limit','trial_days'],
    'pageDiscount',
    $pageDiscount
);

return $result;
```

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


## Endpoint 
Ví dụ
PUT url/api/discount/{id}

---

## Request Body (POST)
| Tên tham số | Kiểu dữ liệu | Bắt buộc | Mô tả | Giá trị mẫu |
|------------------|--------------|----------|------------------------------------------------|-------------|
| name | string | Không | Tên của discount | "discount 1"|
| type | string | Không | Loại discount (percentage/fixed) | "percentage"|
| started_at | datetime | Không | Thời gian bắt đầu | null |
| expired_at | datetime | Không | Thời gian kết thúc | null |
| usage_limit | integer | Không | Giới hạn số lần sử dụng | null |
| value | integer | Không | Giá trị discount | 10 |
| trial_days | integer | Không | Số ngày dùng thử | 0 |
| discount_month | integer | Không | Số tháng áp dụng discount | 12 |

---

## Ví dụ query gửi lên

PUT /api/discount/{id}
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
Status code : 200
json()
{
    "message": "Discount updated successfully",
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


## Endpoint 
Ví dụ
DELETE url/api/discount/{id}

---
## Response mẫu
Status code : 200
json()
{
    "message": "Discount deleted successfully",
}

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
                }
        ]
    }
}
