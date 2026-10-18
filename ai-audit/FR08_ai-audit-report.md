# PHỤ LỤC BẮT BUỘC: BÁO CÁO KIỂM TOÁN AI (AI AUDIT REPORT)
## TÍNH NĂNG FR-08: THANH TOÁN (CHECKOUT) (POOL B)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Môn học:** Kiểm thử phần mềm (Software Testing)
* **Tính năng kiểm thử:** FR-08: Thanh toán (Checkout) (Pool B)
* **Ngày lập báo cáo:** 2026-10-18
* **Chính sách AI:** Tuân thủ nguyên tắc AI-First strategy và Human Review bắt buộc.

---

## 1. KHAI BÁO BẮT BUỘC VỀ SỬ DỤNG AI (MANDATORY AI DISCLOSURE)

> **Khai báo chính thức:**  
> *"I use AI tools for the following tasks: Trích xuất biến đầu vào từ đặc tả yêu cầu FR-08, thiết kế phân vùng tương đương (Equivalence Partitioning), thiết kế bảng phân tích giá trị biên 3 giá trị (3-Value BVA), gỡ lỗi truy vấn CSDL SQLite và hỗ trợ định dạng báo cáo lỗi."*

### Bảng công cụ AI đã sử dụng
| Tên công cụ AI | Phiên bản / Model | Mục đích sử dụng | Môi trường tương tác |
| :--- | :--- | :--- | :--- |
| **Gemini Argon** | Gemini Argon (Google DeepMind) | Phân tích biến đầu vào, thiết kế EP, thiết kế BVA 3 giá trị, sinh kịch bản thực thi và hỗ trợ API testing | Giao diện Chat & IDE Assistant |

---

## 2. NHẬT KÝ TƯƠNG TÁC CHI TIẾT (AI INTERACTION LOGS)

---

### 📌 Phiên tương tác #01: Trích xuất và phân tích biến đầu vào từ đặc tả FR-08
* **Thời gian:** 2026-10-18 14:32:15
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Bạn là chuyên gia kiểm thử phần mềm. Dựa vào đặc tả sau của tính năng FR-08: Thanh toán (Checkout) trong hệ thống EShop, hãy:

  1. Liệt kê TẤT CẢ các biến đầu vào (input variables)
  2. Với mỗi biến, xác định:
     - Kiểu dữ liệu (string, number, email, etc.)
     - Miền giá trị hợp lệ (valid domain)
     - Các ràng buộc (constraints)
     - Quan hệ phụ thuộc giữa các biến (nếu có)
  ```
* **Đầu ra (Output của AI):**
  * AI đã trích xuất danh sách biến đầu vào gồm:
    1. `Authorization` (JWT Token): Bắt buộc cho người dùng đã đăng nhập (FR-08, SEC-02).
    2. `items` (Danh sách sản phẩm): Mảng chứa $\ge 1$ đối tượng sản phẩm, không thể thanh toán nếu giỏ trống.
    3. `quantity` (Số lượng SP): Số nguyên dương $\ge 1$.
    4. `shipping_address` (Địa chỉ giao hàng): Chuỗi ký tự khác rỗng, độ dài 1–255 ký tự (FR-22).
    5. `total_amount` (Tổng tiền): Read-only trên UI, Backend bắt buộc phải tự tính lại từ CSDL (FR-08).
  * AI cũng liệt kê các quan hệ phụ thuộc giữa Token, giỏ hàng và công thức tính tổng tiền.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Phân loại biến rất đầy đủ và bao quát cả tầng UI lẫn an ninh Backend/API.
  * *Chỉnh sửa:* Yêu cầu nhấn mạnh ràng buộc bảo mật của biến `total_amount` (Read-only trên UI, Backend không được chấp nhận giá trị do client gửi lên).

---

### 📌 Phiên tương tác #02: Thiết kế Phân vùng tương đương (EP) & Test Cases cho FR-08
* **Thời gian:** 2026-10-18 14:48:40
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Dựa trên danh sách biến đầu vào đã phân tích ở trên cho tính năng FR-08: Thanh toán (Checkout), hãy áp dụng kỹ thuật Domain Testing theo các bước sau:

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
  * Bảng phân vùng tương đương EP chi tiết cho 5 nhóm biến:
    * Token: EC1.1 (Valid token), EC1.2 (Không gửi token).
    * Giỏ hàng: EC2.1 (Mảng có $\ge 1$ item), EC2.2 (Mảng rỗng `[]`).
    * Số lượng: EC3.1 (Số nguyên dương), EC3.2 (`quantity = 0`), EC3.3 (`quantity = -2`).
    * Địa chỉ: EC4.1 (Địa chỉ hợp lệ 1–255 chars), EC4.2 (Chuỗi rỗng), EC4.3 (Chuỗi 256 chars), EC4.4 (Payload XSS).
    * Tổng tiền: EC5.1 (Backend tự tính), EC5.2 (Client can thiệp sửa giá rẻ mạt).
  * Danh sách test cases Domain Testing bao phủ đầy đủ các lớp tương đương trên.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Cấu trúc phân chia lớp tương đương rõ ràng, độ bao phủ cao.
  * *Chỉnh sửa:* Lưu ý AI phải giữ nguyên tắc: khi test lớp Invalid thì các biến còn lại phải giữ giá trị Valid chuẩn để tránh hiện tượng che giấu lỗi (error masking).

---

### 📌 Phiên tương tác #03: Thiết kế Phân tích Giá trị Biên 3 giá trị (ISTQB 3-Value BVA)
* **Thời gian:** 2026-10-18 15:02:18
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Tiếp tục với tính năng FR-08: Thanh toán (Checkout), bây giờ hãy áp dụng kỹ thuật 
  Phân tích Giá trị Biên (Boundary Value Analysis) theo chuẩn ISTQB 3-Value BVA:

  Với MỖI biến đầu vào có boundary rõ ràng, xác định:

  1. Bảng giá trị biên:
  | Biến | Boundary | Min-1 | Min | Min+1 | Nominal | Max-1 | Max | Max+1 |
  (Chỉ rõ Expected: Valid/Invalid cho mỗi giá trị)

  2. Test Cases BVA:
  | TC-ID | Mô tả | Biến kiểm thử | Giá trị | Expected Result | Loại Boundary |

  Lưu ý:
  - Áp dụng cho CẢ boundaries số (length, value) VÀ boundaries phi số (date, ordered sets)
  - Với string: boundary là độ dài (min length, max length)
  - Với mật khẩu: boundary là 7 ký tự (invalid), 8 ký tự (valid boundary)
  - Sử dụng 3-Value BVA: test boundary, boundary-1, boundary+1
  ```
* **Đầu ra (Output của AI):**
  * Bảng phân tích 3-Value BVA hoàn chỉnh:
    * Số mặt hàng trong giỏ (`items.length`): Min-1 (0 item - Invalid), Min (1 item - Valid), Min+1 (2 items - Valid).
    * Số lượng từng mặt hàng (`quantity`): Min-1 (0 - Invalid), Min (1 - Valid), Min+1 (2 - Valid).
    * Độ dài địa chỉ (`shipping_address`): Min-1 (0 ký tự - Invalid), Min (1 ký tự - Valid), Min+1 (2 ký tự - Valid), Max-1 (254 ký tự - Valid), Max (255 ký tự - Valid), Max+1 (256 ký tự - Invalid).
    * Tổng tiền đơn hàng (`total_amount`): Min-1 (0 ₫ - Invalid), Min (1.000 ₫ - Valid), Nominal (36.000.000 ₫).
  * Bộ Test Cases BVA tương ứng.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Xác định đúng các mốc biên 3 giá trị chuẩn ISTQB.
  * *Chỉnh sửa:* Hướng dẫn cách tạo chuỗi 256 ký tự bằng PowerShell (`"A" * 256`) để sinh viên dễ dàng đưa vào payload API lúc test thực tế.

---

### 📌 Phiên tương tác #04: Hỗ trợ kỹ thuật kiểm tra bảng `orders` trong CSDL SQLite
* **Thời gian:** 2026-10-18 15:13:59
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Làm sao biết sqlite bảng order đã ghi nhận,...
  ```
* **Đầu ra (Output của AI):**
  * AI phân tích mã nguồn `backend/database.js` và cảnh báo bẫy kỹ thuật: hàm `initDatabase()` tự động xóa sạch dữ liệu (`DROP TABLE IF EXISTS orders`) mỗi khi gọi `require('./database')`.
  * Cung cấp câu lệnh truy vấn an toàn kết nối trực tiếp vào `database.sqlite` mà không làm mất dữ liệu:
    ```powershell
    node -e "const sqlite3 = require('sqlite3'); const db = new sqlite3.Database('./database.sqlite'); db.all('SELECT id, user_id, total_amount, status, shipping_address, created_at FROM orders ORDER BY id DESC LIMIT 5', (e, rows) => console.table(rows));"
    ```
  * Hướng dẫn cách xem trực quan bằng extension SQLite Viewer trên VS Code.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận và thực thi:* Sinh viên chạy lệnh thành công trong PowerShell, quan sát trực tiếp bản ghi đơn hàng với `total_amount = 1000 ₫` và chụp ảnh làm bằng chứng cho `EXE-12`.

---

### 📌 Phiên tương tác #05: Bóc tách ranh giới tính năng (Loại bỏ nhầm lẫn sang FR-09)
* **Thời gian:** 2026-10-18 15:22:43
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Kiểm tra lại câu trả lời sau, có vài phần qua tới fr09 rồi, hãy lọc lại chỉ phần fr 08 dùm tôi.
  ```
* **Đầu ra (Output của AI):**
  * AI đã thừa nhận sai sót do nhầm lẫn giữa chức năng Thanh toán (FR-08) và Mã giảm giá (FR-09).
  * Lọc sạch toàn bộ 9 ca kiểm thử liên quan đến coupon (mã `SAVE10`, `BIGBUY`, điều kiện C1–C5), chỉ giữ lại các kịch bản thuần túy của Checkout (giỏ hàng, địa chỉ, tổng tiền, token).
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Phạm vi nghiệp vụ được bóc tách dứt khoát, đảm bảo bài nộp đúng trọng tâm và không bị trừ điểm vi phạm phạm vi tính năng.

---

### 📌 Phiên tương tác #06: Tinh gọn ca kiểm thử, đôn số thứ tự và tổng hợp báo cáo
* **Thời gian:** 2026-10-18 15:47:12
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Xóa 9 và 8 vì trùng 7 6. Các số khác sẽ đôn lên các số bị xóa. Ví dụ như exe-10 sẽ thành exe-8. Xóa 14. 
  Dưới đây là những gì tôi đã thực hiện (13 test cases)... Hãy viết tổng kết dùm tôi.
  ```
* **Đầu ra (Output của AI):**
  * Chuẩn hóa danh sách ca kiểm thử thành đúng **13 ca thực thi thực tế (`EXE-01` đến `EXE-13`)**.
  * Khớp 100% với tên file ảnh trong thư mục `screenshots/fr08/` và kết quả thực tế (5 Pass, 8 Fail, 7 Bugs).
  * Tự động sinh báo cáo `FR08_Domain_Testing_Report.md`, hướng dẫn 7 GitHub Issues và tài liệu AI Audit.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận hoàn toàn:* Bộ tài liệu đạt độ hoàn thiện cao nhất, logic chặt chẽ từ lý thuyết đến thực thi.

---

## 3. THỐNG KÊ ĐỊNH LƯỢNG SỬ DỤNG AI (QUANTITATIVE METRICS)

| Chỉ số đánh giá (Metric) | Số lượng | Tỷ lệ (%) |
| :--- | :---: | :---: |
| **Tổng số lượt tương tác / Prompt gửi đi** | 6 phiên lớn | 100% |
| **Đầu ra của AI được chấp nhận nguyên văn** | 2 | 33.33% |
| **Đầu ra của AI cần sinh viên chỉnh sửa / can thiệp (Human-in-the-loop)** | 4 | 66.67% |
| **Đầu ra của AI bị loại bỏ hoàn toàn** | 0 | 0.0% |
| **Số test cases do AI gợi ý ban đầu** | 20 EP + 29 BVA | — |
| **Số test cases được sinh viên tinh lọc và thực thi thực tế** | 13 | 100% |
| **Số lỗi do AI dự đoán đúng** | 3/7 lỗi | 42.86% |
| **Số lỗi do sinh viên thực thi thực tế mới phát hiện ra (AI bỏ sót)** | 4/7 lỗi | 57.14% |
