## Endpoint

GET https://laraveldeploy-master-clgksm.laravel.cloud/api/discounts

---

## Query Parameters

| Tên tham số       | Kiểu dữ liệu | Bắt buộc | Mô tả                                           | Giá trị mẫu |
|-------------------|--------------|----------|------------------------------------------------|-------------|
| perPageDiscount   | integer      | Không    | Số lượng bản ghi trên 1 trang (phân trang)     | 10          |
| pageDiscount      | integer      | Không    | Trang hiện tại                                 | 1           |
| search            | string       | Không    | Từ khóa tìm kiếm                               | "sale"      |
| sortStartedAt     | string       | Không    | Sắp xếp theo trường `started_at` (asc/desc)    | "desc"      |

---

## Ví dụ query gửi lên

GET https://laraveldeploy-master-clgksm.laravel.cloud/api/discounts?perPageDiscount=10&pageDiscount=2&search=sale&sortStartedAt=desc
