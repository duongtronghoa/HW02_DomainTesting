# PHỤ LỤC BẮT BUỘC: BÁO CÁO KIỂM TOÁN AI (AI AUDIT REPORT)
## TÍNH NĂNG FR-17: QUẢN LÝ MÃ GIẢM GIÁ (COUPON CRUD) (POOL C)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Môn học:** Kiểm thử phần mềm (Software Testing)
* **Tính năng kiểm thử:** FR-17: Quản lý Mã Giảm Giá (Coupon CRUD) (Pool C)
* **Ngày lập báo cáo:** 2026-10-18
* **Chính sách AI:** Tuân thủ nguyên tắc AI-First strategy và Human Review bắt buộc.

---

## 1. KHAI BÁO BẮT BUỘC VỀ SỬ DỤNG AI (MANDATORY AI DISCLOSURE)

> **Khai báo chính thức:**  
> *"I use AI tools for the following tasks: Trích xuất biến đầu vào từ đặc tả yêu cầu FR-17, thiết kế phân vùng tương đương (Equivalence Partitioning), thiết kế bảng phân tích giá trị biên 3 giá trị (3-Value BVA), sinh câu lệnh kiểm thử API cURL/PowerShell, phân tích đối chiếu dữ liệu CSDL SQLite và hỗ trợ định dạng báo cáo lỗi."*

### Bảng công cụ AI đã sử dụng
| Tên công cụ AI | Phiên bản / Model | Mục đích sử dụng | Môi trường tương tác |
| :--- | :--- | :--- | :--- |
| **Gemini Argon** | Gemini Argon (Google DeepMind) | Phân tích biến đầu vào, thiết kế EP, thiết kế BVA 3 giá trị, sinh kịch bản thực thi và hỗ trợ kiểm thử API Admin | Giao diện Chat & IDE Assistant |

---

## 2. NHẬT KÝ TƯƠNG TÁC CHI TIẾT (AI INTERACTION LOGS)

---

### 📌 Phiên tương tác #01: Trích xuất và phân tích biến đầu vào từ đặc tả FR-17
* **Thời gian:** 2026-10-18 16:02:10
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Bạn là chuyên gia kiểm thử phần mềm. Dựa vào đặc tả sau của tính năng FR-17: Quản lý Mã Giảm Giá (Coupon CRUD)
  trong hệ thống EShop, hãy:

  1. Liệt kê TẤT CẢ các biến đầu vào (input variables)
  2. Với mỗi biến, xác định:
     - Kiểu dữ liệu (string, number, email, etc.)
     - Miền giá trị hợp lệ (valid domain)
     - Các ràng buộc (constraints)
     - Quan hệ phụ thuộc giữa các biến (nếu có)
  ```
* **Đầu ra (Output của AI):**
  * AI đã trích xuất danh sách biến đầu vào cho các thao tác CRUD của Coupon:
    1. `Authorization` (JWT Token): Bắt buộc cho tài khoản Quản trị viên (`role = 'admin'`) theo FR-12 và SEC-03.
    2. `code` (Mã giảm giá): Chuỗi ký tự, 1–50 ký tự, duy nhất, bắt buộc theo FR-22, chống XSS theo SEC-04.
    3. `discount_type` (Loại giảm giá): Chuỗi enum `'percent'` hoặc `'fixed'`.
    4. `discount_value` (Mức giảm giá): Số dương $> 0$; nếu `percent` thì trong khoảng $1..100$, nếu `fixed` thì $> 0$.
    5. `min_order_amount` (Đơn tối thiểu): Số nguyên $\ge 0$.
    6. `max_uses_per_user` (Lượt dùng tối đa): Số nguyên dương $\ge 1$.
    7. `id` (ID mã giảm giá khi xóa): Số nguyên dương $\ge 1$ đang tồn tại trong bảng `coupons`.
  * AI chỉ ra mối quan hệ phụ thuộc giữa `discount_type` và miền giá trị của `discount_value`.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Nhận diện đầy đủ toàn bộ các trường dữ liệu và ràng buộc của endpoint `/api/admin/coupons`.
  * *Chỉnh sửa:* Yêu cầu nhấn mạnh ràng buộc bảo mật RBAC (chỉ có Admin mới được phép gọi các API này, tài khoản người dùng thường phải bị chặn).

---

### 📌 Phiên tương tác #02: Thiết kế Phân vùng tương đương (EP) & Test Cases cho FR-17
* **Thời gian:** 2026-10-18 16:08:45
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Dựa trên danh sách biến đầu vào đã phân tích ở trên cho tính năng FR-17: Quản lý Mã Giảm Giá (Coupon CRUD),
  hãy áp dụng kỹ thuật Domain Testing theo các bước sau:

  Bước 2a: Phân vùng tương đương (Equivalence Partitioning)
  Với MỖI biến đầu vào, xác định:
  - Các lớp tương đương hợp lệ (Valid Equivalence Classes)
  - Các lớp tương đương không hợp lệ (Invalid Equivalence Classes)

  Trình bày dưới dạng bảng:
  | Biến | Lớp tương đương | Loại (Valid/Invalid) | Giá trị đại diện | Mô tả |

  Bước 2b: Thiết kế test cases Domain Testing
  Dựa trên các lớp tương đương, thiết kế test cases:
  - Mỗi test case bao phủ ít nhất 1 lớp tương đương chưa được bao phủ
  - Ưu tiên kết hợp các lớp Valid trước, sau đó test từng lớp Invalid riêng
  - Format:
  | TC-ID | Mô tả | Input Values | Expected Result | Equivalence Classes Covered |
  ```
* **Đầu ra (Output của AI):**
  * Bảng phân vùng tương đương EP chi tiết cho 7 biến: Token (Admin vs Non-Admin vs No-Token), Code (Hợp lệ, Rỗng, Trùng lặp, XSS), Type, Discount Value, Min Order, Max Uses, và Coupon ID (Xóa ID tồn tại vs Không tồn tại).
  * Danh sách các test case bao phủ các lớp tương đương.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Cấu trúc phân chia lớp tương đương khoa học, bao quát cả luồng tạo mã, xem danh sách và xóa mã.
  * *Chỉnh sửa:* Bổ sung rõ ca kiểm thử bảo mật với Non-Admin Token để kiểm tra khả năng kiểm soát truy cập (RBAC).

---

### 📌 Phiên tương tác #03: Thiết kế Phân tích Giá trị Biên 3 giá trị (ISTQB 3-Value BVA)
* **Thời gian:** 2026-10-18 16:15:20
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Tiếp tục với tính năng FR-17: Quản lý Mã Giảm Giá (Coupon CRUD), bây giờ hãy áp dụng kỹ thuật 
  Phân tích Giá trị Biên (Boundary Value Analysis) theo chuẩn ISTQB 3-Value BVA:

  Với MỖI biến số hoặc chuỗi có giới hạn độ dài:
  1. Xác định ranh giới (Boundaries)
  2. Xác định 3 giá trị cho mỗi biên:
     - Biên dưới: Min-1 (Invalid), Min (Valid), Min+1 (Valid)
     - Biên trên: Max-1 (Valid), Max (Valid), Max+1 (Invalid)
  3. Giá trị danh định (Nominal Value)

  Trình bày dưới dạng bảng:
  | Biến | Ranh giới | Min-1 | Min | Min+1 | Nominal | Max-1 | Max | Max+1 |
  ```
* **Đầu ra (Output của AI):**
  * Bảng 3-Value BVA chi tiết:
    * `discount_value` (percent): 0 (Invalid), 1 (Valid), 2 (Valid), 20 (Nominal), 99 (Valid), 100 (Valid), 101 (Invalid).
    * `discount_value` (fixed): 0 (Invalid), 1 (Valid), 2 (Valid), 50.000 (Nominal).
    * `min_order_amount`: -1 (Invalid), 0 (Valid), 1 (Valid), 200.000 (Nominal).
    * `max_uses_per_user`: 0 (Invalid), 1 (Valid), 2 (Valid), 3 (Nominal).
    * `coupon_id`: 0 (Invalid), 1 (Valid), 2 (Valid), Existing ID (Nominal), 99999 (Invalid - Không tồn tại).
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Phân tích chuẩn xác các điểm biên 3 giá trị theo lý thuyết ISTQB.

---

### 📌 Phiên tương tác #04: Rà soát, chuẩn hóa và loại bỏ các ca kiểm thử giao diện tĩnh
* **Thời gian:** 2026-10-18 16:20:10
* **Công cụ AI:** Gemini Argon
* **Nội dung tương tác:**
  * Sinh viên phát hiện trong bản phác thảo ban đầu, kịch bản `EXE-01` chứa nội dung kiểm tra màu sắc nút bấm và thẻ `<h1>` của trang Web Admin.
  * Sinh viên ra chỉ thị: *"Xóa kịch bản kiểm tra giao diện vì nó thuộc về GUI Testing chung, không phản ánh kỹ thuật kiểm thử miền giá trị Domain Testing của FR-17. Đồng thời đôn các mã kiểm thử lên để tạo thành bộ 14 test cases hoàn chỉnh từ EXE-01 đến EXE-14."*
  * AI đã tái cấu trúc danh sách, bắt đầu `EXE-01` bằng ca kiểm thử tạo mã phần trăm chuẩn (Nominal Percent) và giữ nguyên 14 kịch bản trọng tâm về Domain/BVA/Security.

---

### 📌 Phiên tương tác #05: Hướng dẫn thực thi thực tế, phân tích lỗi mã nguồn và ghi nhận kết quả
* **Thời gian:** 2026-10-18 16:25:30 – 16:35:00
* **Công cụ AI:** Gemini Argon
* **Nội dung tương tác:**
  * AI cung cấp các câu lệnh PowerShell tương tác trực tiếp với API `http://localhost:3000/api/admin/coupons` và kiểm tra bảng `coupons` trong CSDL SQLite `database.sqlite`.
  * Sinh viên thực thi từng kịch bản, chụp ảnh màn hình minh chứng và phát hiện 5 lỗi thực tế nghiêm trọng:
    1. `BUG-FR17-01`: Tài khoản thường `test@eshop.com` tạo và xóa mã thành công (EXE-04, EXE-05).
    2. `BUG-FR17-02`: Trùng mã văng ngoại lệ SQLite và trả về HTTP 500 (EXE-07).
    3. `BUG-FR17-03`: Cho phép tạo mã có giá trị 0, âm, và lỗi giao diện tự đổi `max_uses = 0` thành `1` (EXE-08, EXE-09, EXE-11, EXE-12).
    4. `BUG-FR17-04`: Chấp nhận mã giảm 150% (EXE-10).
    5. `BUG-FR17-05`: Xóa ID 99999 không tồn tại trả về HTTP 200 OK (EXE-13).

---

## 3. BẢNG SỐ LIỆU ĐỊNH LƯỢNG SỬ DỤNG AI (QUANTITATIVE METRICS)

| Chỉ số đo lường | Giá trị thực tế | Ghi chú đánh giá |
| :--- | :---: | :--- |
| **Tổng số lượt Prompt trao đổi** | **5 lượt** | Bao gồm trích xuất biến, lập bảng EP, tính biên BVA, lọc phạm vi GUI và hướng dẫn thực thi |
| **Tổng thời gian tương tác với AI** | **~35 phút** | Từ 16:02 đến 16:35 ngày 2026-10-18 |
| **Tỷ lệ kế thừa đề xuất của AI** | **78%** | Tái sử dụng bảng phân vùng EP và ma trận điểm biên 3-Value BVA |
| **Tỷ lệ sinh viên can thiệp / chỉnh sửa** | **22%** | Bóc tách loại bỏ GUI testing, phát hiện lỗi RBAC và lỗi bất nhất CSDL vs UI |
| **Số lỗi phát hiện được nhờ AI gợi ý biên** | **3 lỗi** | Các lỗi biên 0, số âm, và biên trên 150% (`BUG-FR17-03`, `BUG-FR17-04`) |
| **Số lỗi phát hiện độc lập qua kiểm tra mã nguồn & runtime** | **2 lỗi** | Lỗ hổng Broken Access Control (`BUG-FR17-01`) và sập 500 do SQLite constraint (`BUG-FR17-02`) |
| **Tổng số test cases thực thi thành công** | **14 test cases** | 5 Pass (35.71%) và 9 Fail (64.29%) |

---

## 4. KẾT LUẬN VỀ QUÁ TRÌNH KIỂM TOÁN AI

Quá trình kiểm thử tính năng **FR-17: Quản lý Mã Giảm Giá** đã áp dụng thành công chiến lược **AI-First kết hợp Human-in-the-loop**:
1. AI đóng vai trò như một cỗ máy gia tốc tư duy, giúp cấu trúc hóa bài toán kiểm thử miền giá trị và tính toán các cận biên chuẩn xác chỉ trong vài phút.
2. Tuy nhiên, sự giám sát chặt chẽ của con người là điều kiện tiên quyết: nếu không có sự can thiệp của sinh viên, bài tập sẽ bị loãng do lẫn lộn kiểm thử giao diện tĩnh, đồng thời sẽ bỏ lỡ hoàn toàn lỗ hổng bảo mật tối nghiêm trọng (Broken Access Control) đang tồn tại trong mã nguồn Backend của hệ thống SUT.
