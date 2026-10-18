# HƯỚNG DẪN TẠO GITHUB ISSUES & MẪU BÁO CÁO BUG
## TÍNH NĂNG FR-17: QUẢN LÝ MÃ GIẢM GIÁ (COUPON CRUD) (POOL C)

> **Quy định đề bài (Mục 6 & 14):**
> * Báo cáo tất cả các lỗi được phát hiện trong cả báo cáo Markdown và trên trang GitHub Issues của nhóm bạn.
> * Hãy nhớ đính kèm ảnh chụp màn hình lỗi vào mỗi GitHub issue.
> * Chụp lại ảnh màn hình trang GitHub Issue sau khi tạo để lưu vào thư mục nộp bài: `screenshots/github-issues/`.

---

## 🛠️ HƯỚNG DẪN THAO TÁC TRÊN GITHUB

1. Mở trình duyệt, truy cập vào kho lưu trữ GitHub bài tập của bạn.
2. Click vào tab **`Issues`** $\rightarrow$ Click nút xanh **`New issue`**.
3. Copy **Tiêu đề (Title)** và **Nội dung (Body Markdown)** của từng Bug dưới đây dán vào.
4. **Đính kèm ảnh:** Kéo thả file ảnh chụp màn hình tương ứng từ thư mục `screenshots/fr17/` vào khung soạn thảo của Issue.
5. Gắn nhãn (Labels) ở cột bên phải: `bug`, `FR-17`, `feature-C`.
6. Click **`Submit new issue`**.
7. Chụp lại ảnh màn hình trang GitHub Issue sau khi tạo lưu vào `screenshots/github-issues/` với tên dạng `Issue_BUG_FR17_01.png`.

---

## 📋 NỘI DUNG MẪU CHI TIẾT 5 GITHUB ISSUES (COPY & PASTE)

---

### 🚨 ISSUE #1: BUG-FR17-01 (CRITICAL - SECURITY / BROKEN ACCESS CONTROL)

* **Title:** `[FR-17][SEC-03][CRITICAL] Lỗ hổng Broken Access Control cho phép người dùng thường tự tạo và xóa mã giảm giá`
* **Labels:** `bug`, `critical`, `security`, `FR-17`, `access-control`
* **File ảnh cần kéo thả vào:** `screenshots/fr17/EXE-04_Bug_Critical_NonAdmin_Create_Coupon.png` (kèm `EXE-05_Bug_Critical_NonAdmin_Delete_Coupon.png`)

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR17-01
- **Tính năng:** FR-17: Quản lý Mã Giảm Giá (Coupon CRUD) & SEC-03 / FR-12: Kiểm soát truy cập Quản trị viên (Admin Access Control)
- **Mức độ nghiêm trọng (Severity):** Critical (Lỗ hổng bảo mật nghiêm trọng - Kiểm soát truy cập bị phá vỡ)
- **Mức độ ưu tiên (Priority):** Urgent / Immediate
- **Môi trường:** Chrome / Windows / EShop Local (Backend API `http://localhost:3000`)

### 2. Mô tả lỗi
Theo đặc tả FR-12 và SEC-03:
> *"Tất cả các chức năng quản trị trong Web Admin (bao gồm thêm mới và xóa mã giảm giá) chỉ dành riêng cho tài khoản có vai trò Quản trị viên (role = 'admin'). Người dùng thông thường (role = 'user') hoàn toàn bị cấm truy cập."*

Tuy nhiên, kiểm tra mã nguồn Backend tại `backend/server.js`:
- Endpoint `POST /api/admin/coupons` (dòng 457) chỉ sử dụng middleware `authenticateToken` để kiểm tra đăng nhập mà **hoàn toàn không kiểm tra `req.user.role === 'admin'`**.
- Tương tự, endpoint `DELETE /api/admin/coupons/:id` (dòng 483) cũng chỉ xác thực Token thông thường.

Hậu quả: Một tài khoản người dùng bình thường (`test@eshop.com`, `role = 'user'`) có thể gửi HTTP request tạo mã giảm giá 1.000.000 ₫ hoặc xóa sạch toàn bộ mã giảm giá của hệ thống, gây thiệt hại tài chính và làm gián đoạn vận hành của cửa hàng!

### 3. Các bước tái hiện (Steps to Reproduce)
1. Đăng nhập với tài khoản người dùng thường: `test@eshop.com` / `Test1234!` để lấy JWT Token (`role = 'user'`).
2. Gửi request `POST /api/admin/coupons` với Header `Authorization: Bearer <user_token>` và payload:
   ```json
   {
     "code": "HACKER1000K",
     "discount_type": "fixed",
     "discount_value": 1000000,
     "min_order_amount": 0,
     "max_uses_per_user": 100
   }
   ```
3. Quan sát phản hồi API: Trả về HTTP 200 OK `"Coupon created successfully"`.
4. Gửi request `DELETE /api/admin/coupons/:id` bằng Token người dùng thường: Trả về HTTP 200 OK `"Coupon deleted"`.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống phải kiểm tra vai trò Quản trị viên và từ chối mọi yêu cầu từ tài khoản thường với mã lỗi **HTTP 403 Forbidden**:
```json
{ "error": "Access denied. Admin role required." }
```

### 5. Kết quả thực tế (Actual Result)
Tài khoản thường thực hiện thành công cả 2 thao tác tạo mới và xóa mã giảm giá với **HTTP 200 OK**.

### 6. Bằng chứng kiểm thử (Screenshots)
![NonAdmin Create Coupon](screenshots/fr17/EXE-04_Bug_Critical_NonAdmin_Create_Coupon.png)
![NonAdmin Delete Coupon](screenshots/fr17/EXE-05_Bug_Critical_NonAdmin_Delete_Coupon.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Bổ sung middleware kiểm tra vai trò Admin trước khi thực hiện logic nghiệp vụ trong `backend/server.js`:
```javascript
function requireAdmin(req, res, next) {
  if (!req.user || req.user.role !== 'admin') {
    return res.status(403).json({ error: 'Access denied: Admin role required' });
  }
  next();
}

app.post('/api/admin/coupons', authenticateToken, requireAdmin, (req, res) => { ... });
app.delete('/api/admin/coupons/:id', authenticateToken, requireAdmin, (req, res) => { ... });
```
```

---

### 💥 ISSUE #2: BUG-FR17-02 (HIGH - UNHANDLED EXCEPTION / CRASH ON DUPLICATE CODE)

* **Title:** `[FR-17][HIGH] Backend gặp lỗi 500 Internal Server Error và văng exception SQLite khi tạo mã giảm giá bị trùng lặp`
* **Labels:** `bug`, `high`, `backend`, `FR-17`, `error-handling`
* **File ảnh cần kéo thả vào:** `screenshots/fr17/EXE-07_Bug_Duplicate_Code_500_Error.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR17-02
- **Tính năng:** FR-17: Quản lý Mã Giảm Giá (Create Coupon)
- **Mức độ nghiêm trọng (Severity):** High (Lỗi sập luồng xử lý do không bắt ngoại lệ CSDL)
- **Mức độ ưu tiên (Priority):** High
- **Môi trường:** Chrome / Windows / EShop Local (Backend API `http://localhost:3000`)

### 2. Mô tả lỗi
Trong bảng `coupons` của SQLite, cột `code` được cấu hình ràng buộc `UNIQUE` (`code TEXT UNIQUE NOT NULL`).
Khi Quản trị viên nhập mã giảm giá bị trùng với mã đã có trong hệ thống (ví dụ: `WELCOME10`), Backend `server.js` (dòng 460-475) thực hiện câu lệnh `INSERT` mà không kiểm tra sự tồn tại của mã trước, cũng không bắt mã lỗi ngoại lệ `SQLITE_CONSTRAINT: UNIQUE constraint failed`.

Khi ngoại lệ xảy ra, câu lệnh `if (err) return res.status(500).json({ error: err.message })` đã trả về lỗi **HTTP 500 Internal Server Error** kèm stack trace lộ thông tin cấu trúc CSDL cho client.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Đăng nhập tài khoản Quản trị viên (`admin@eshop.com`).
2. Gửi request tạo mã giảm giá với mã đã tồn tại sẵn trong CSDL:
   ```json
   {
     "code": "WELCOME10",
     "discount_type": "percent",
     "discount_value": 10,
     "min_order_amount": 100000,
     "max_uses_per_user": 1
   }
   ```
3. Quan sát phản hồi API từ Backend.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống phải kiểm tra hoặc bắt lỗi vi phạm ràng buộc UNIQUE, trả về mã lỗi client hợp lệ **HTTP 400 Bad Request** hoặc **HTTP 409 Conflict** kèm thông báo thân thiện:
```json
{ "error": "Mã giảm giá 'WELCOME10' đã tồn tại trong hệ thống." }
```

### 5. Kết quả thực tế (Actual Result)
Backend trả về **HTTP 500 Internal Server Error**:
```json
{ "error": "SQLITE_CONSTRAINT: UNIQUE constraint failed: coupons.code" }
```

### 6. Bằng chứng kiểm thử (Screenshots)
![Duplicate Code 500 Error](screenshots/fr17/EXE-07_Bug_Duplicate_Code_500_Error.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Xử lý lỗi cụ thể trong callback của câu lệnh `db.run`:
```javascript
if (err) {
  if (err.message.includes('UNIQUE constraint failed')) {
    return res.status(409).json({ error: 'Mã giảm giá đã tồn tại' });
  }
  return res.status(500).json({ error: 'Lỗi máy chủ nội bộ' });
}
```
```

---

### ⚠️ ISSUE #3: BUG-FR17-03 (HIGH - MISSING BOUNDARY & INPUT VALIDATION FOR NUMERIC FIELDS)

* **Title:** `[FR-17][HIGH] Backend không kiểm tra ràng buộc số dương cho discount_value, min_order_amount và max_uses_per_user`
* **Labels:** `bug`, `high`, `backend`, `FR-17`, `data-validation`
* **File ảnh cần kéo thả vào:** `screenshots/fr17/EXE-08_Bug_BVA_DiscountValue_Zero_Accepted.png` (kèm `EXE-09`, `EXE-11`, `EXE-12`)

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR17-03
- **Tính năng:** FR-17: Quản lý Mã Giảm Giá (Coupon Validation)
- **Mức độ nghiêm trọng (Severity):** High (Lỗi tính toàn vẹn dữ liệu và vi phạm phân tích biên)
- **Mức độ ưu tiên (Priority):** High
- **Môi trường:** Chrome / Windows / EShop Local (Backend API `http://localhost:3000`)

### 2. Mô tả lỗi
Theo đặc tả nghiệp vụ FR-17:
- `discount_value`: Phải là số dương $> 0$ (mã giảm giá không thể giảm 0 ₫ hoặc giảm số tiền âm).
- `min_order_amount`: Phải là số không âm $\ge 0$ (không thể có đơn hàng tối thiểu âm).
- `max_uses_per_user`: Phải là số nguyên dương $\ge 1$ (mã phải dùng được ít nhất 1 lần).

Tuy nhiên, Backend `server.js` nhận toàn bộ dữ liệu từ body mà không có bất kỳ lệnh kiểm tra điều kiện nào:
1. Cho phép tạo mã giảm giá với `discount_value = 0` (EXE-08).
2. Cho phép tạo mã giảm giá với `discount_value = -50000` (EXE-09).
3. Cho phép tạo mã giảm giá với `min_order_amount = -10000` (EXE-11).
4. Cho phép tạo mã giảm giá với `max_uses_per_user = 0` (EXE-12). Nghiêm trọng hơn, khi tải lại trang Web Admin (F5), giao diện lại tự động hiển thị số `1`, dẫn đến sự bất nhất tai hại giữa dữ liệu thực trong CSDL (lưu 0) và giao diện hiển thị cho Quản trị viên (hiển thị 1).

### 3. Các bước tái hiện (Steps to Reproduce)
1. Dùng tài khoản Admin gọi API `POST /api/admin/coupons` với payload chứa giá trị biên vi phạm:
   - Case 1: `discount_value: 0`
   - Case 2: `discount_value: -50000`
   - Case 3: `min_order_amount: -10000`
   - Case 4: `max_uses_per_user: 0`
2. Quan sát phản hồi API và kiểm tra dữ liệu trong bảng `coupons` của SQLite.
3. Đối với Case 4, mở Web Admin xem danh sách coupon và nhấn F5 để kiểm tra hiển thị.

### 4. Kết quả mong đợi (Expected Result)
Backend phải validate dữ liệu và từ chối các giá trị không hợp lệ với mã lỗi **HTTP 400 Bad Request**:
- `discount_value` phải là số dương $> 0$.
- `min_order_amount` phải là số nguyên $\ge 0$.
- `max_uses_per_user` phải là số nguyên $\ge 1$.

### 5. Kết quả thực tế (Actual Result)
Tất cả các request trên đều được Backend chấp nhận với **HTTP 200 OK** và lưu thẳng vào SQLite. Tại Web Admin, mã có `max_uses_per_user = 0` bị render thành `1`.

### 6. Bằng chứng kiểm thử (Screenshots)
![Discount Value Zero](screenshots/fr17/EXE-08_Bug_BVA_DiscountValue_Zero_Accepted.png)
![Negative Discount Value](screenshots/fr17/EXE-09_Bug_Negative_DiscountValue_Accepted.png)
![Negative Min Order Amount](screenshots/fr17/EXE-11_Bug_BVA_MinOrderAmount_Negative_Accepted.png)
![Max Uses Zero](screenshots/fr17/EXE-12_Bug_BVA_MaxUses_Zero_Accepted.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Thêm kiểm tra tính hợp lệ trước khi thực hiện câu lệnh Insert:
```javascript
if (!discount_value || Number(discount_value) <= 0) {
  return res.status(400).json({ error: 'Mức giảm giá phải lớn hơn 0' });
}
if (min_order_amount !== undefined && Number(min_order_amount) < 0) {
  return res.status(400).json({ error: 'Đơn hàng tối thiểu không được âm' });
}
if (max_uses_per_user !== undefined && Number(max_uses_per_user) < 1) {
  return res.status(400).json({ error: 'Số lượt dùng tối đa phải từ 1 trở lên' });
}
```
```

---

### ⚠️ ISSUE #4: BUG-FR17-04 (HIGH - PERCENT DISCOUNT EXCEEDS 100%)

* **Title:** `[FR-17][HIGH] Cho phép tạo mã giảm giá theo phần trăm vượt quá 100% (discount_value = 150%)`
* **Labels:** `bug`, `high`, `backend`, `FR-17`, `business-logic`
* **File ảnh cần kéo thả vào:** `screenshots/fr17/EXE-10_Bug_BVA_Percent_Over100_Accepted.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR17-04
- **Tính năng:** FR-17: Quản lý Mã Giảm Giá (Percent Discount Business Logic)
- **Mức độ nghiêm trọng (Severity):** High (Lỗi quy tắc nghiệp vụ tài chính)
- **Mức độ ưu tiên (Priority):** High
- **Môi trường:** Chrome / Windows / EShop Local (Backend API `http://localhost:3000`)

### 2. Mô tả lỗi
Theo nguyên lý thương mại điện tử và đặc tả FR-17:
> *"Đối với loại mã giảm giá theo phần trăm (discount_type = 'percent'), mức giảm giá hợp lệ phải nằm trong đoạn [1..100]%."*

Mức giảm phần trăm vượt quá 100% (ví dụ: 150%) sẽ dẫn tới việc số tiền giảm giá lớn hơn toàn bộ giá trị đơn hàng, làm tổng tiền thanh toán bị âm nếu hệ thống không có lớp chặn phụ trợ.
Tuy nhiên, Backend `POST /api/admin/coupons` hoàn toàn không kiểm tra giới hạn trên (Max Boundary = 100) khi `discount_type === 'percent'`.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Gửi request tạo coupon loại phần trăm với giá trị 150%:
   ```json
   {
     "code": "OVER100",
     "discount_type": "percent",
     "discount_value": 150,
     "min_order_amount": 100000,
     "max_uses_per_user": 1
   }
   ```
2. Quan sát kết quả phản hồi của API.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống phải từ chối yêu cầu với mã lỗi **HTTP 400 Bad Request** kèm thông báo lỗi:
```json
{ "error": "Mức giảm giá theo phần trăm không được vượt quá 100%" }
```

### 5. Kết quả thực tế (Actual Result)
Backend chấp nhận và lưu mã giảm giá 150% vào CSDL SQLite với mã phản hồi **HTTP 200 OK**.

### 6. Bằng chứng kiểm thử (Screenshots)
![Percent Over 100 Accepted](screenshots/fr17/EXE-10_Bug_BVA_Percent_Over100_Accepted.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Bổ sung kiểm tra logic phần trăm trong `backend/server.js`:
```javascript
if (discount_type === 'percent' && (Number(discount_value) < 1 || Number(discount_value) > 100)) {
  return res.status(400).json({ error: 'Mức giảm phần trăm phải nằm trong khoảng từ 1 đến 100' });
}
```
```

---

### ℹ️ ISSUE #5: BUG-FR17-05 (MEDIUM - REST API NON-EXISTENT COUPON DELETION RETURNS 200 OK)

* **Title:** `[FR-17][MEDIUM] Xóa mã giảm giá với ID không tồn tại trả về HTTP 200 OK thay vì HTTP 404 Not Found`
* **Labels:** `bug`, `medium`, `backend`, `FR-17`, `restful-api`
* **File ảnh cần kéo thả vào:** `screenshots/fr17/EXE-13_Bug_Delete_NonExistent_ID_200OK.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR17-05
- **Tính năng:** FR-17: Quản lý Mã Giảm Giá (Delete Coupon API)
- **Mức độ nghiêm trọng (Severity):** Medium (Vi phạm chuẩn thiết kế RESTful API)
- **Mức độ ưu tiên (Priority):** Medium
- **Môi trường:** Chrome / Windows / EShop Local (Backend API `http://localhost:3000`)

### 2. Mô tả lỗi
Khi thực hiện xóa một tài nguyên không tồn tại thông qua phương thức `DELETE /api/admin/coupons/:id`, chuẩn RESTful API quy định hệ thống phải phản hồi mã **HTTP 404 Not Found** để thông báo cho phía client biết tài nguyên cần thao tác không có trong hệ thống.

Tuy nhiên, tại `backend/server.js` dòng 483-490:
```javascript
app.delete('/api/admin/coupons/:id', authenticateToken, (req, res) => {
  db.run('DELETE FROM coupons WHERE id = ?', [req.params.id], function(err) {
    if (err) return res.status(500).json({ error: err.message });
    res.json({ message: 'Coupon deleted' });
  });
});
```
Hàm callback hoàn toàn không kiểm tra `this.changes`. Do đó dù `id = 99999` không tồn tại và không có bản ghi nào bị xóa (`this.changes === 0`), API vẫn trả về thông báo thành công `Coupon deleted` kèm mã HTTP 200 OK.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Gửi request `DELETE /api/admin/coupons/99999` với JWT Token hợp lệ.
2. Quan sát Status Code và thông điệp trả về từ API.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống phải kiểm tra số dòng bị ảnh hưởng và trả về **HTTP 404 Not Found**:
```json
{ "error": "Không tìm thấy mã giảm giá với ID 99999" }
```

### 5. Kết quả thực tế (Actual Result)
API trả về **HTTP 200 OK** kèm thông báo:
```json
{ "message": "Coupon deleted" }
```

### 6. Bằng chứng kiểm thử (Screenshots)
![Delete NonExistent ID 200 OK](screenshots/fr17/EXE-13_Bug_Delete_NonExistent_ID_200OK.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Kiểm tra `this.changes` sau khi thực thi câu lệnh SQL DELETE:
```javascript
app.delete('/api/admin/coupons/:id', authenticateToken, requireAdmin, function(req, res) {
  db.run('DELETE FROM coupons WHERE id = ?', [req.params.id], function(err) {
    if (err) return res.status(500).json({ error: err.message });
    if (this.changes === 0) {
      return res.status(404).json({ error: 'Mã giảm giá không tồn tại' });
    }
    res.json({ message: 'Coupon deleted' });
  });
});
```
```
