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
| withCoupon        |boolean/int   | Không    | Lấy kèm thông tin coupon (1: có, 0: không)     | 1           |

---

## Ví dụ query gửi lên

GET url/api/discounts?perPageDiscount=10&pageDiscount=2&search=sale&sortStartedAt=desc&withCoupon=1

## Response mẫu
discounts thuộc Illuminate\AwareLenth\Paginator
```jsx
{
    "message": "Discounts retrieved successfully",
    "discounts": {
        "current_page": 2,
        "data": [
            { "id": 11, "name": "Sale 11", "started_at": "2024-05-01", "expired_at": "2024-05-31" },
            { "id": 12, "name": "Sale 12", "started_at": "2024-06-01", "expired_at": "2024-06-30" }
        ],
        "last_page": 5,
        "per_page": 10,
        "total": 50
    }
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
