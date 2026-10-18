# HƯỚNG DẪN TẠO GITHUB ISSUES & MẪU BÁO CÁO BUG
## TÍNH NĂNG FR-04: QUẢN LÝ HỒ SƠ CÁ NHÂN

> **Quy định đề bài (Mục 6 & 14):**
> * Báo cáo tất cả các lỗi được phát hiện trong cả báo cáo Markdown và trên trang GitHub Issues của nhóm bạn.
> * Hãy nhớ đính kèm ảnh chụp màn hình lỗi vào mỗi GitHub issue.
> * Chụp lại ảnh màn hình trang GitHub Issue sau khi tạo để lưu vào thư mục nộp bài: `screenshots/github-issues/`.

---

## 🛠️ HƯỚNG DẪN THAO TÁC TRÊN GITHUB

1. Mở trình duyệt, truy cập vào kho lưu trữ GitHub của nhóm bạn (hoặc repo bài tập của bạn).
2. Click vào tab **`Issues`** $\rightarrow$ Click nút xanh **`New issue`**.
3. Copy **Tiêu đề (Title)** và **Nội dung (Body Markdown)** của từng Bug dưới đây dán vào.
4. **Đính kèm ảnh:** Kéo thả file ảnh chụp màn hình tương ứng từ thư mục `screenshots/fr04/` vào khung soạn thảo của Issue.
5. Gắn nhãn (Labels) ở cột bên phải: `bug`, `FR-04`, `feature-A`.
6. Click **`Submit new issue`**.
7. **CHỤP ẢNH MÀN HÌNH ISSUE:** Nhấn `Windows + Shift + S` chụp lại toàn bộ trang Issue vừa tạo trên GitHub $\rightarrow$ Lưu vào thư mục:
   `23120127_HW02_AI_DomainTesting_100/screenshots/github-issues/` với tên dạng `Issue_BUG_FR04_01.png`.

---

## 📋 NỘI DUNG MẪU CHI TIẾT 5 GITHUB ISSUES (COPY & PASTE)

---

### 🐛 ISSUE #1: BUG-FR04-01 (High - Validation Logic Sai Lệch)

* **Title:** `[FR-04][BUG] Hệ thống từ chối số điện thoại 10 và 11 chữ số hợp lệ và thông báo sai lệch 9-10 chữ số`
* **Labels:** `bug`, `high`, `FR-04`, `validation`
* **File ảnh cần kéo thả vào:** `screenshots/fr04/EXE-02_Valid_10Digits_Phone.png` (hoặc `EXE-03`)

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR04-01
- **Tính năng:** FR-04: Quản lý hồ sơ cá nhân
- **Mức độ nghiêm trọng (Severity):** High
- **Mức độ ưu tiên (Priority):** High
- **Môi trường:** Chrome 120 / Windows 11 / EShop Local (Frontend: React, Backend: Node.js)

### 2. Mô tả lỗi
Theo đặc tả nghiệp vụ FR-04: *"Số điện thoại hợp lệ: bắt đầu bằng số 0, từ 10–11 chữ số"*.
Tuy nhiên, khi người dùng nhập số điện thoại 10 chữ số (`0912345678`) hoặc 11 chữ số (`01234567890`), hệ thống từ chối lưu và đưa ra thông báo lỗi mâu thuẫn: *"Số điện thoại không hợp lệ. Vui lòng nhập 9-10 chữ số"*. Điều này khiến người dùng hợp lệ không thể cập nhật được số điện thoại chuẩn của Việt Nam.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Đăng nhập vào hệ thống với tài khoản `test@eshop.com` / `Test1234!`.
2. Điều hướng tới trang **Hồ sơ cá nhân** (`http://localhost:5173/profile`).
3. Nhập Họ Tên: `"Nguyễn Văn A"`, Địa chỉ: `"123 Lê Lợi, Q1"`.
4. Nhập Số điện thoại 10 số hợp lệ: `"0912345678"`.
5. Nhấn nút **Cập nhật**.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống chấp nhận SĐT 10 số (và 11 số), cập nhật thành công vào CSDL SQLite và hiển thị thông báo thành công.

### 5. Kết quả thực tế (Actual Result)
Hệ thống báo lỗi: *"Số điện thoại không hợp lệ. Vui lòng nhập 9-10 chữ số"*. Không cho phép lưu.

### 6. Bằng chứng hình ảnh (Screenshot)
*(Kéo thả file screenshots/fr04/EXE-02_Valid_10Digits_Phone.png vào đây)*
```

---

### 🐛 ISSUE #2: BUG-FR04-02 (Medium - Data Integrity)

* **Title:** `[FR-04][BUG] Hệ thống cho phép cập nhật và lưu Địa chỉ giao hàng rỗng vào CSDL`
* **Labels:** `bug`, `medium`, `FR-04`, `data-integrity`
* **File ảnh cần kéo thả vào:** `screenshots/fr04/EXE-07_Error_Empty_Address.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR04-02
- **Tính năng:** FR-04: Quản lý hồ sơ cá nhân
- **Mức độ nghiêm trọng (Severity):** Medium
- **Mức độ ưu tiên (Priority):** Medium

### 2. Mô tả lỗi
Trường "Địa chỉ giao hàng mặc định" là thông tin thiết yếu để phục vụ chức năng Thanh toán & Giao hàng (FR-08, FR-18). Tuy nhiên, form cập nhật hồ sơ và Backend API cho phép người dùng xóa trắng địa chỉ và bấm lưu thành công, ghi đè chuỗi rỗng `""` vào CSDL SQLite.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Truy cập vào trang Hồ sơ cá nhân (`http://localhost:5173/profile`).
2. Nhập Họ Tên: `"Nguyễn Văn A"`, SĐT hợp lệ.
3. Xóa sạch ô **Địa chỉ giao hàng mặc định** (để rỗng 0 ký tự).
4. Nhấn nút **Cập nhật**.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống phải chặn lại, hiển thị thông báo lỗi yêu cầu nhập địa chỉ ở phía trên nút bấm (theo FR-22).

### 5. Kết quả thực tế (Actual Result)
Hệ thống hiển thị thông báo "Cập nhật thành công" và CSDL lưu địa chỉ rỗng.

### 6. Bằng chứng hình ảnh (Screenshot)
*(Kéo thả file screenshots/fr04/EXE-07_Error_Empty_Address.png vào đây)*
```

---

### 🐛 ISSUE #3: BUG-FR04-03 (Medium - Boundary Validation)

* **Title:** `[FR-04][BUG] Không giới hạn độ dài tối đa 255 ký tự cho trường Họ Tên và Địa chỉ`
* **Labels:** `bug`, `medium`, `FR-04`, `boundary`
* **File ảnh cần kéo thả vào:** `screenshots/fr04/EXE-08_BVA_MaxPlus1_256Chars.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR04-03
- **Tính năng:** FR-04: Quản lý hồ sơ cá nhân
- **Mức độ nghiêm trọng (Severity):** Medium
- **Mức độ ưu tiên (Priority):** Low/Medium

### 2. Mô tả lỗi
Tại ranh giới kiểm thử Max + 1 (256 ký tự), cả giao diện Frontend và API Backend đều không có cơ chế chặn giới hạn độ dài (thiếu thuộc tính `maxlength="255"` trên thẻ `<input>`), cho phép lưu chuỗi dài tùy ý vào CSDL SQLite.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Mở Console trong F12 gõ: `copy('A'.repeat(256))` để lấy chuỗi 256 ký tự.
2. Dán chuỗi này vào ô **Họ Tên** và **Địa chỉ**.
3. Nhấn nút **Cập nhật**.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống chặn không cho nhập ký tự thứ 256 hoặc Backend trả về mã lỗi HTTP 400 Bad Request từ chối chuỗi quá dài.

### 5. Kết quả thực tế (Actual Result)
Hệ thống chấp nhận và lưu chuỗi 256 ký tự thành công.

### 6. Bằng chứng hình ảnh (Screenshot)
*(Kéo thả file screenshots/fr04/EXE-08_BVA_MaxPlus1_256Chars.png vào đây)*
```

---

### 🐛 ISSUE #4: BUG-FR04-04 (High - Boundary Validation)

* **Title:** `[FR-04][BUG] Hệ thống chấp nhận số điện thoại không bắt đầu bằng số 0`
* **Labels:** `bug`, `high`, `FR-04`, `boundary`
* **File ảnh cần kéo thả vào:** `screenshots/fr04/EXE-12_BVA_Phone_NotStartWithZero.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR04-04
- **Tính năng:** FR-04: Quản lý hồ sơ cá nhân
- **Mức độ nghiêm trọng (Severity):** High
- **Mức độ ưu tiên (Priority):** High

### 2. Mô tả lỗi
Đặc tả FR-04 quy định rõ ràng: *"Số điện thoại hợp lệ: bắt đầu bằng số 0"*. Tuy nhiên, hệ thống cho phép người dùng nhập số điện thoại bắt đầu bằng chữ số khác 0 (ví dụ: `1912345678`) và lưu thành công vào CSDL.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Tại trang Hồ sơ cá nhân, nhập Họ Tên và Địa chỉ hợp lệ.
2. Nhập Số điện thoại: `"1912345678"` (bắt đầu bằng số 1 thay vì số 0).
3. Nhấn nút **Cập nhật**.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống từ chối cập nhật, hiển thị lỗi yêu cầu số điện thoại phải bắt đầu bằng số 0.

### 5. Kết quả thực tế (Actual Result)
Hệ thống báo cập nhật thành công và lưu số điện thoại sai định dạng vào CSDL.

### 6. Bằng chứng hình ảnh (Screenshot)
*(Kéo thả file screenshots/fr04/EXE-12_BVA_Phone_NotStartWithZero.png vào đây)*
```

---

### 🚨 ISSUE #5: BUG-FR04-05 (CRITICAL - SECURITY VULNERABILITY)

* **Title:** `[FR-04][SEC-06][CRITICAL] Lỗ hổng Leo thang đặc quyền (Privilege Escalation) cho phép User tự đổi role="admin"`
* **Labels:** `bug`, `critical`, `security`, `FR-04`, `SEC-06`
* **File ảnh cần kéo thả vào:** `screenshots/fr04/EXE-15_Bug_Privilege_Escalation.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR04-05
- **Tính năng:** FR-04: Quản lý hồ sơ cá nhân & SEC-06: Phân quyền tài khoản
- **Mức độ nghiêm trọng (Severity):** Critical (Nghiêm trọng nhất)
- **Mức độ ưu tiên (Priority):** Urgent / Immediate

### 2. Mô tả lỗi
Đặc tả FR-04 và SEC-06 nghiêm cấm: *"Người dùng chỉ có thể cập nhật hồ sơ của chính mình; không thể tự thay đổi thuộc tính role"*.
Tuy nhiên, trong mã nguồn Backend tại `backend/server.js` (dòng 124-127):
```javascript
if (role) {
  query += ", role = ?";
  params.push(role);
}
```
API `PUT /api/users/me` đã nhận trực tiếp tham số `role` từ body request và cập nhật thẳng vào CSDL SQLite mà không hề kiểm tra quyền hạn. Điều này tạo điều kiện cho bất kỳ người dùng thông thường nào cũng có thể tự cấp quyền Quản trị viên (`admin`) cho chính mình và chiếm quyền điều khiển toàn bộ hệ thống Web Admin.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Đăng nhập bằng tài khoản người dùng thường `test@eshop.com`.
2. Mở PowerShell hoặc Postman, gửi request HTTP PUT tới `http://localhost:3000/api/users/me` với Header `Authorization: Bearer <token_user>` và body JSON:
```json
{
  "name": "Nguyễn Văn A",
  "phone": "0912345678",
  "shipping_address": "123 Lê Lợi",
  "role": "admin"
}
```
3. Kiểm tra CSDL SQLite: Cột `role` của user chuyển thành `'admin'`.
4. Mở trình duyệt truy cập Web Admin `http://localhost:5174/` bằng tài khoản `test@eshop.com` $\rightarrow$ Đăng nhập thành công và truy cập toàn bộ trang quản trị!

### 4. Kết quả mong đợi (Expected Result)
Backend phải từ chối request có chứa thuộc tính `role` (HTTP 400 Bad Request hoặc 403 Forbidden) hoặc phớt lờ trường `role`, giữ nguyên quyền hạn ban đầu của người dùng.

### 5. Kết quả thực tế (Actual Result)
Backend cập nhật thành công và trao quyền Admin cho tài khoản người dùng thường.

### 6. Bằng chứng hình ảnh (Screenshot)
*(Kéo thả file screenshots/fr04/EXE-15_Bug_Privilege_Escalation.png vào đây)*
```
