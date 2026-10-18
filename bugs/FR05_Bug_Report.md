# BÁO CÁO CHI TIẾT DANH SÁCH LỖI (BUG REPORT)
## TÍNH NĂNG FR-05: XEM DANH SÁCH & TÌM KIẾM SẢN PHẨM (MOBILE - POOL D)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Hệ thống được kiểm thử (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Tổng số lỗi phát hiện:** 4 nhóm lỗi (1 Critical, 1 High, 1 Medium, 1 Low/Medium)
* **Ngày lập báo cáo:** 2026-10-18

---

## DANH SÁCH CHI TIẾT CÁC LỖI ĐÃ PHÁT HIỆN

---

### 🚨 BUG #1: BUG-FR05-03 (CRITICAL - SECURITY / SQL INJECTION & HTML LEAK)

* **Tiêu đề Issue:** `[FR-05][SEC-01][CRITICAL] Lỗ hổng SQL Injection tại ô tìm kiếm sản phẩm và rò rỉ mã HTML lỗi 500 trên Mobile`
* **Nhãn (Labels):** ``bug`, `critical`, `security`, `FR-05`, `sql-injection``
* **Mức độ nghiêm trọng (Severity):** Critical (Lỗ hổng bảo mật tối nghiêm trọng - Tiết lộ dữ liệu & Sập CSDL)
* **Mức độ ưu tiên (Priority):** Urgent / Immediate
* **Môi trường:** Expo React Native / Android Emulator (Pixel 6) & Web Mobile / Backend Node.js Express / CSDL SQLite

#### 1. Mô tả lỗi
Theo đặc tả SEC-01:
> *"Hệ thống phải áp dụng Parameterized Query cho tất cả các câu truy vấn cơ sở dữ liệu để ngăn chặn triệt để tấn công SQL Injection."*

Tuy nhiên, tại `backend/server.js` (dòng 141-151), API `GET /api/products` nối chuỗi trực tiếp giá trị `req.query.search` vào câu truy vấn SQL:

#### 2. Các bước tái hiện (Steps to Reproduce)


#### 3. Kết quả mong đợi (Expected Result)


#### 4. Kết quả thực tế (Actual Result)


#### 5. Bằng chứng hình ảnh (Screenshot)
* **Ảnh minh chứng:** [`EXE-08_Bug_Critical_SQL_Injection.png`](../screenshots/pool%20D/EXE-08_Bug_Critical_SQL_Injection.png)

![EXE-08_Bug_Critical_SQL_Injection.png](../screenshots/pool%20D/EXE-08_Bug_Critical_SQL_Injection.png)

---

### ⚠️ BUG #2: BUG-FR05-01 (HIGH - MISSING EMPTY STATE UI COMPONENT)

* **Tiêu đề Issue:** `[FR-05][HIGH] Ứng dụng Mobile hoàn toàn thiếu màn hình Empty State khi tìm kiếm không có kết quả`
* **Nhãn (Labels):** ``bug`, `high`, `ui/ux`, `FR-05`, `mobile``
* **Mức độ nghiêm trọng (Severity):** High (Vi phạm trực tiếp yêu cầu giao diện cốt lõi của FR-05)
* **Mức độ ưu tiên (Priority):** High
* **Môi trường:** Expo React Native / Android Emulator (Pixel 6) & Web Mobile

#### 1. Mô tả lỗi
Theo quy định rõ ràng trong đặc tả FR-05:
> *"Khi không có kết quả tìm kiếm phải hiển thị thông báo empty state phù hợp (có hình minh họa và thông báo rõ ràng/thân thiện)."*

Tuy nhiên, kiểm tra mã nguồn `frontend-mobile/App.js` (dòng 518-531):

#### 2. Các bước tái hiện (Steps to Reproduce)


#### 3. Kết quả mong đợi (Expected Result)


#### 4. Kết quả thực tế (Actual Result)


#### 5. Bằng chứng hình ảnh (Screenshot)
* **Ảnh minh chứng:** [`EXE-06_Bug_Missing_Empty_State.png`](../screenshots/pool%20D/EXE-06_Bug_Missing_Empty_State.png)

![EXE-06_Bug_Missing_Empty_State.png](../screenshots/pool%20D/EXE-06_Bug_Missing_Empty_State.png)

---

### ⚠️ BUG #3: BUG-FR05-02 (MEDIUM - UNTRIMMED WHITESPACE IN SEARCH QUERY)

* **Tiêu đề Issue:** `[FR-05][MEDIUM] Backend không cắt bỏ khoảng trắng thừa (untrimmed whitespace), khiến tìm kiếm thất bại`
* **Nhãn (Labels):** ``bug`, `medium`, `backend`, `FR-05`, `data-handling``
* **Mức độ nghiêm trọng (Severity):** Medium (Lỗi xử lý dữ liệu đầu vào người dùng)
* **Mức độ ưu tiên (Priority):** Medium
* **Môi trường:** Backend Node.js Express / Mobile App

#### 1. Mô tả lỗi
Người dùng trên thiết bị di động thường vô tình gõ thêm dấu cách (khoảng trắng) ở đầu hoặc cuối từ khóa tìm kiếm (ví dụ do bàn phím ảo tự động thêm space sau từ).
Theo chuẩn xử lý dữ liệu biểu mẫu (Form Data Handling), hệ thống phải tự động cắt tỉa khoảng trắng ở hai đầu (`trim()`) trước khi thực hiện truy vấn.

Tuy nhiên, tại `backend/server.js` (dòng 142), giá trị `req.query.search` được gán trực tiếp vào câu lệnh SQL mà không gọi `.trim()`. 
Do đó, khi tìm kiếm chuỗi `"  iPhone  "`, câu truy vấn trở thành `WHERE name LIKE '%  iPhone  %'`. Vì tên sản phẩm trong CSDL không có khoảng trắng thừa, hệ thống trả về 0 kết quả dù sản phẩm "iPhone 15 Pro Max" đang tồn tại.

#### 2. Các bước tái hiện (Steps to Reproduce)
1. Mở ứng dụng Mobile, nhập từ khóa có 2 dấu cách đầu và 2 dấu cách cuối:
   `"  iPhone  "`
2. Bấm nút **"Tìm"**.
3. Quan sát kết quả tìm kiếm.

#### 3. Kết quả mong đợi (Expected Result)
Hệ thống tự động cắt bỏ khoảng trắng thừa, tìm kiếm từ khóa `"iPhone"` và hiển thị sản phẩm `iPhone 15 Pro Max`.

#### 4. Kết quả thực tế (Actual Result)
Hệ thống không tìm thấy sản phẩm nào (trả về danh sách rỗng 0 sản phẩm).

#### 5. Bằng chứng hình ảnh (Screenshot)
* **Ảnh minh chứng:** [`EXE-05_Bug_Untrimmed_Whitespace_Fail.png`](../screenshots/pool%20D/EXE-05_Bug_Untrimmed_Whitespace_Fail.png)

![EXE-05_Bug_Untrimmed_Whitespace_Fail.png](../screenshots/pool%20D/EXE-05_Bug_Untrimmed_Whitespace_Fail.png)

#### 6. Đề xuất khắc phục (Suggested Fix)
Gọi `.trim()` trên biến `search` ở cả Frontend lẫn Backend:

---

### ℹ️ BUG #4: BUG-FR05-04 (LOW/MEDIUM - IMAGE ASPECT RATIO DISTORTION & MISSING ALT)

* **Tiêu đề Issue:** `[FR-05][GUI] Ảnh sản phẩm bị co giãn méo tỷ lệ (resizeMode stretch) và thiếu thuộc tính alt text mô tả`
* **Nhãn (Labels):** ``bug`, `low`, `ui/ux`, `accessibility`, `FR-05``
* **Mức độ nghiêm trọng (Severity):** Low / Medium (Vi phạm chuẩn giao diện và khả năng tiếp cận Accessibility)
* **Mức độ ưu tiên (Priority):** Medium
* **Môi trường:** Expo React Native / Mobile App

#### 1. Mô tả lỗi
Đặc tả FR-05 nêu rõ:
> *"Mỗi sản phẩm hiển thị: Ảnh (tỷ lệ chuẩn, có alt text mô tả), Tên sản phẩm, Giá (đơn vị: ₫, định dạng phân cách hàng nghìn)."*

Tuy nhiên, kiểm tra component `<Image>` tại `frontend-mobile/App.js` dòng 457-461:

#### 2. Các bước tái hiện (Steps to Reproduce)


#### 3. Kết quả mong đợi (Expected Result)


#### 4. Kết quả thực tế (Actual Result)


#### 5. Bằng chứng hình ảnh (Screenshot)
* **Ảnh minh chứng:** [`EXE-01_GUI_Default_Product_List.png`](../screenshots/pool%20D/EXE-01_GUI_Default_Product_List.png)

![EXE-01_GUI_Default_Product_List.png](../screenshots/pool%20D/EXE-01_GUI_Default_Product_List.png)

---
