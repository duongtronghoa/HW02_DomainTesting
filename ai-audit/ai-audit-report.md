# PHỤ LỤC BẮT BUỘC: BÁO CÁO KIỂM TOÁN AI (AI AUDIT REPORT)
## MÃ BÀI TẬP: HW02-AI — DOMAIN TESTING TRÊN ESHOP

* **Mã sinh viên:** 23120127
* **Môn học:** Kiểm thử phần mềm (Software Testing)
* **Tính năng kiểm thử:** FR-04: Quản lý hồ sơ cá nhân (Pool A)
* **Ngày lập báo cáo:** 2026-10-18
* **Chính sách AI:** Tuân thủ nguyên tắc AI-First strategy và Human Review bắt buộc.

---

## 1. KHAI BÁO BẮT BUỘC VỀ SỬ DỤNG AI (MANDATORY AI DISCLOSURE)

> **Khai báo chính thức:**  
> *"I use AI tools for the following tasks: Trích xuất biến đầu vào từ đặc tả yêu cầu, thiết kế phân vùng tương đương (Equivalence Partitioning), thiết kế bảng phân tích giá trị biên 3 giá trị (3-Value BVA), xây dựng kịch bản thực thi kiểm thử và hỗ trợ tạo báo cáo lỗi."*

### Bảng công cụ AI đã sử dụng
| Tên công cụ AI | Phiên bản / Model | Mục đích sử dụng | Môi trường tương tác |
| :--- | :--- | :--- | :--- |
| **Gemini Argon** | Gemini Argon (Google DeepMind) | Phân tích biến đầu vào, thiết kế EP, thiết kế BVA 3 giá trị, sinh kịch bản thực thi và hỗ trợ API testing | Giao diện Chat & IDE Assistant |

---

## 2. NHẬT KÝ TƯƠNG TÁC CHI TIẾT (AI INTERACTION LOGS)

---

### 📌 Phiên tương tác #01: Trích xuất và phân tích biến đầu vào từ đặc tả FR-04
* **Ngày và giờ:** 2026-10-18 10:16:42
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Bạn là chuyên gia kiểm thử phần mềm. Dựa vào đặc tả sau của tính năng fr04 : Quản lý hồ sơ cá nhân trong hệ thống EShop, hãy:

  1. Liệt kê TẤT CẢ các biến đầu vào (input variables)
  2. Với mỗi biến, xác định:
     - Kiểu dữ liệu (string, number, email, etc.)
     - Miền giá trị hợp lệ (valid domain)
     - Các ràng buộc (constraints)
     - Quan hệ phụ thuộc giữa các biến (nếu có)

  Đặc tả:
  - Người dùng đã đăng nhập có thể cập nhật: Họ Tên, Số điện thoại, Địa chỉ giao hàng mặc định.
  - Số điện thoại hợp lệ: bắt đầu bằng số 0, từ 10–11 chữ số.
  - Email không được phép thay đổi qua giao diện.
  - Người dùng chỉ có thể cập nhật hồ sơ của chính mình; không thể tự thay đổi thuộc tính role.
  - Ràng buộc giao diện (FR-21, FR-22): Mỗi trang có đúng 1 thẻ <h1>; thông báo lỗi xuất hiện trên nút submit; trường bắt buộc có dấu *.
  ```
* **Đầu ra (Output của AI):**
  * AI trích xuất 6 nhóm biến đầu vào chính:
    1. `Họ Tên (name)`: Kiểu String, độ dài 1–255 ký tự, bắt buộc (`*`), không được để trống.
    2. `Số điện thoại (phone)`: Kiểu Numeric String, định dạng `^0\d{9,10}$`, bắt đầu bằng `0`, độ dài từ 10–11 số, bắt buộc.
    3. `Địa chỉ giao hàng (shipping_address)`: Kiểu String, độ dài 1–255 ký tự, bắt buộc, không được để trống.
    4. `Email`: Read-only trên giao diện; cấm thay đổi qua API.
    5. `role`: Ràng buộc an ninh SEC-06, không cho phép Client tự thăng quyền thành `'admin'`.
    6. `id / user_id`: Ràng buộc SEC-02, người dùng chỉ được sửa hồ sơ của chính mình (chống lỗi IDOR).
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Phân loại biến rất đầy đủ và bao quát cả tầng UI lẫn an ninh Backend/API.
  * *Chỉnh sửa:* Chuyển biến xác thực `Authorization (JWT Token)` thành điều kiện tiên quyết (Precondition) để bảng kiểm thử tập trung vào đúng trọng tâm kỹ thuật Domain Testing cho các trường dữ liệu hồ sơ.

---

### 📌 Phiên tương tác #02: Thiết kế Phân vùng tương đương (Equivalence Partitioning) & Test Cases
* **Ngày và giờ:** 2026-10-18 10:48:17
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Dựa trên danh sách biến đầu vào đã phân tích ở trên cho tính năng fr04 : Quản lý hồ sơ cá nhân, hãy áp dụng kỹ thuật Domain Testing theo các bước sau:

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
  * Bảng phân vùng tương đương hoàn chỉnh:
    * Họ Tên: EC1.1 (Valid, chuỗi 1–255 ký tự), EC1.2 (Invalid, chuỗi rỗng 0 ký tự), EC1.3 (Invalid, chuỗi 256 ký tự).
    * Địa chỉ: EC2.1 (Valid, 1–255 ký tự), EC2.2 (Invalid, rỗng), EC2.3 (Invalid, 256 ký tự).
    * Số điện thoại: EC3.1 (Valid, 10 số), EC3.2 (Valid, 11 số), EC3.3 (Invalid, rỗng), EC3.4 (Invalid, 9 số), EC3.5 (Invalid, 12 số), EC3.6 (Invalid, không bắt đầu bằng 0), EC3.7 (Invalid, chứa chữ cái/ký tự đặc biệt).
    * Email & Quyền hạn: EC4.1, EC4.2 (can thiệp email mới qua API), EC5.1, EC5.2 (can thiệp `role: admin`), EC6.1, EC6.2 (sửa id người khác).
  * Danh sách test cases phối hợp các lớp Valid và Invalid độc lập.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Cấu trúc phân chia lớp tương đương rõ ràng, độ bao phủ cao.
  * *Chỉnh sửa:* Yêu cầu tuân thủ nghiêm ngặt nguyên tắc kiểm thử hộp đen: khi kiểm thử lớp Invalid, chỉ có duy nhất 1 biến mang giá trị Invalid, các biến còn lại phải giữ giá trị Valid chuẩn để tránh hiện tượng che giấu lỗi (error masking).

---

### 📌 Phiên tương tác #03: Thiết kế Phân tích Giá trị Biên (ISTQB 3-Value BVA)
* **Ngày và giờ:** 2026-10-18 11:33:28
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Tiếp tục với tính năng fr04 : Quản lý hồ sơ cá nhân, bây giờ hãy áp dụng kỹ thuật 
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
  * Bảng phân tích 3-Value BVA chi tiết:
    * SĐT (Độ dài chữ số): $[10, 11]$ số $\rightarrow$ Min-1 (9 số - Invalid), Min (10 số - Valid), Min+1/Max (11 số - Valid), Max+1 (12 số - Invalid).
    * SĐT (Ký tự đầu ASCII): Biên đơn điểm ASCII 48 (`'0'`) $\rightarrow$ Min-1 (ASCII 47 `'/'` - Invalid), Min (ASCII 48 `'0'` - Valid), Min+1 (ASCII 49 `'1'` - Invalid).
    * Họ Tên & Địa chỉ: Độ dài $[1, 255]$ $\rightarrow$ Min-1 (0 ký tự - Invalid), Min (1 ký tự - Valid), Min+1 (2 ký tự - Valid), Max-1 (254 ký tự - Valid), Max (255 ký tự - Valid), Max+1 (256 ký tự - Invalid).
  * Bộ Test Cases BVA tương ứng cho từng mốc biên.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Xác định cực kỳ chính xác trường hợp biên hẹp của SĐT (khoảng cách 1 đơn vị giữa 10 và 11 khiến Min+1 trùng với Max), đúng chuẩn ISTQB 3-Value BVA.
  * *Chỉnh sửa:* Bổ sung dữ liệu thử nghiệm cụ thể (chuỗi 255/256 ký tự bằng script JavaScript `copy('a'.repeat(255))` trong Console) để sinh viên dễ dàng đưa vào form test thực tế.

---

### 📌 Phiên tương tác #04: Hỗ trợ Gỡ lỗi API Endpoint và Kịch bản kiểm thử Bảo mật
* **Ngày và giờ:** 2026-10-18 13:56:47
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Tôi chạy lệnh PowerShell gọi PUT /api/profile thì bị lỗi "Cannot PUT /api/profile". 
  Hãy kiểm tra xem đường dẫn API thực tế của EShop là gì và cho tôi lệnh PowerShell chính xác để test đổi email, role và IDOR cho tính năng fr04.
  ```
* **Đầu ra (Output của AI):**
  * AI đã phân tích cấu trúc mã nguồn `backend/server.js` và xác định đường dẫn API thực tế là `PUT /api/users/me`.
  * Chỉ ra câu truy vấn SQL tại dòng 124 của file `server.js`:
    ```javascript
    if (role) {
      query += ", role = ?";
      params.push(role);
    }
    ```
    cho thấy hệ thống có nguy cơ dính lỗi Mass Assignment / Privilege Escalation.
  * Cung cấp các câu lệnh `Invoke-RestMethod` trong PowerShell với đầy đủ Token JWT để test trực tiếp.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Giải quyết dứt điểm lỗi 404, giúp sinh viên tiếp cận đúng endpoint và phát hiện con Bug bảo mật Critical nghiêm trọng nhất của tính năng FR-04.
  * *Thực thi:* Sinh viên trực tiếp chạy script trên PowerShell cá nhân, quan sát thấy `role` của user bị đổi thành `admin` trong CSDL SQLite và đăng nhập thành công vào Web Admin, sau đó chụp ảnh màn hình lưu vào `screenshots/fr04/EXE-15_Bug_Privilege_Escalation.png`.

---

## 3. THỐNG KÊ ĐỊNH LƯỢNG SỬ DỤNG AI (QUANTITATIVE METRICS)

| Chỉ số đánh giá (Metric) | Số lượng | Tỷ lệ (%) |
| :--- | :---: | :---: |
| **Tổng số lượt tương tác / Prompt gửi đi** | 4 phiên lớn | 100% |
| **Đầu ra của AI được chấp nhận nguyên văn** | 1 | 25.0% |
| **Đầu ra của AI cần sinh viên chỉnh sửa / tinh chỉnh (Human-in-the-loop)** | 3 | 75.0% |
| **Đầu ra của AI bị loại bỏ hoàn toàn** | 0 | 0.0% |
| **Số test cases do AI gợi ý ban đầu** | 27 BVA + 16 EP | — |
| **Số test cases được sinh viên tinh lọc và thực thi thực tế** | 16 | 100% |
| **Số lỗi do AI dự đoán đúng** | 2/5 lỗi | 40.0% |
| **Số lỗi do sinh viên thực thi thực tế mới phát hiện ra (AI bỏ sót)** | 3/5 lỗi | 60.0% |
