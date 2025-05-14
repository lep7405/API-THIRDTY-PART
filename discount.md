## API Endpoints

### 1. GET /api/discounts
#### Query Parameters
| Parameter Name   | Data Type   | Required | Description                          | Example Value |
|------------------|-------------|----------|--------------------------------------|---------------|
| `perPageDiscount` | integer     | No       | Number of records per page (pagination) | 10            |
| `pageDiscount`   | integer     | No       | Current page                         | 1             |
| `search`         | string      | No       | Search keyword                       | "sale"        |
| `sortStartedAt`  | string      | No       | Sort by `started_at` field (asc/desc) | "desc"        |
| `withCoupon`     | boolean/int | No       | Include coupon information           | true          |

#### Example Request
```
GET /api/discounts?perPageDiscount=10&pageDiscount=2&search=sale&sortStartedAt=desc&withCoupon=1
```

#### Response Sample
- **Status Code**: 200 (OK)
- **Body**:
  ```json
  {
      "message": "Discounts retrieved successfully",
      "discounts": $discounts
  }
  ```
  - **Note**: `$discounts` belongs to `Illuminate\Pagination\LengthAwarePaginator`.
  - **Image Reference**: ![image](https://github.com/user-attachments/assets/0d791854-dfc2-4916-a409-55cb0fd274e0)

#### Current Logic
```php
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
                        });
                        if (is_numeric($search)) {
                            $q->orWhere(function ($inner) use ($search) {
                                $inner->where('type', 'amount')
                                    ->where('value', $search);
                            });
                            };
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
    ['id', 'name', 'started_at', 'expired_at', 'type', 'value', 'usage_limit', 'trial_days'],
    'pageDiscount',
    $pageDiscount
);

return $result;
```

---

### 2. POST /api/discounts
#### Request Body
| Parameter Name   | Data Type | Required | Description                | Example Value |
|------------------|-----------|----------|----------------------------|---------------|
| `name`           | string    | Yes      | Name of the discount       | "discount 1"  |
| `type`           | string    | Yes      | Discount type (percentage/fixed) | "percentage" |
| `started_at`     | datetime  | No       | Start time                 | null          |
| `expired_at`     | datetime  | No       | End time                   | null          |
| `usage_limit`    | integer   | No       | Usage limit                | null          |
| `value`          | integer   | Yes      | Discount value             | 10            |
| `trial_days`     | integer   | Yes      | Trial days                 | 0             |
| `discount_month` | integer   | Yes      | Discount months            | 12            |

#### Example Request
```
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
```

#### Response Sample
- **Status Code**: 201 (Created)
- **Body**:
  ```json
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
  ```

---

### 3. PUT /api/discount/{id}
#### Request Body
| Parameter Name   | Data Type | Required | Description                | Example Value |
|------------------|-----------|----------|----------------------------|---------------|
| `name`           | string    | No       | Name of the discount       | "discount 1"  |
| `type`           | string    | No       | Discount type (percentage/fixed) | "percentage" |
| `started_at`     | datetime  | No       | Start time                 | null          |
| `expired_at`     | datetime  | No       | End time                   | null          |
| `usage_limit`    | integer   | No       | Usage limit                | null          |
| `value`          | integer   | No       | Discount value             | 10            |
| `trial_days`     | integer   | No       | Trial days                 | 0             |
| `discount_month` | integer   | No       | Discount months            | 12            |

#### Example Request
```
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
```

#### Response Sample
- **Status Code**: 200 (OK)
- **Body**:
  ```json
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
  ```

---

### 4. DELETE /api/discount/{id}
#### Response Sample
- **Status Code**: 200 (OK)
- **Body**:
  ```json
  {
      "message": "Discount deleted successfully"
  }
  ```

---

### 5. GET /api/discount/{id}
#### Query Parameters
| Parameter Name | Data Type | Required | Description          | Example Value |
|----------------|-----------|----------|----------------------|---------------|
| `withCoupon`   | boolean   | No       | Include coupon info  | true          |

#### Response Sample
- **Status Code**: 200 (OK)
- **Body**:
  ```json
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
                  "code": "DISCOUNT10"
              }
          ]
      }
  }
  ```
#### Current Logic
```php
$discount = Discount::query()
            ->when($withCoupon, function ($query) {
                $query->with(['coupon' => function ($query) {
                    $query->select('id', 'times_used', 'discount_id');
                }]);
            })
            ->find($id);
```


