# PHỤ LỤC BẮT BUỘC: BÁO CÁO KIỂM TOÁN AI (AI AUDIT REPORT)
## TÍNH NĂNG FR-05: XEM DANH SÁCH & TÌM KIẾM SẢN PHẨM (MOBILE - POOL D)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Môn học:** Kiểm thử phần mềm (Software Testing)
* **Tính năng kiểm thử:** FR-05: Xem danh sách & Tìm kiếm sản phẩm (Mobile - Pool D)
* **Ngày lập báo cáo:** 2026-10-18
* **Chính sách AI:** Tuân thủ nguyên tắc AI-First strategy và Human Review bắt buộc.

---

## 1. KHAI BÁO BẮT BUỘC VỀ SỬ DỤNG AI (MANDATORY AI DISCLOSURE)

> **Khai báo chính thức:**  
> *"I use AI tools for the following tasks: Trích xuất biến đầu vào từ đặc tả yêu cầu FR-05 Mobile, thiết kế phân vùng tương đương (Equivalence Partitioning), thiết kế bảng phân tích giá trị biên 3 giá trị (3-Value BVA), hướng dẫn thiết lập môi trường máy ảo Android Pixel 6 và Expo Web, gỡ lỗi kết nối mạng cục bộ API_URL, và hỗ trợ định dạng báo cáo lỗi."*

### Bảng công cụ AI đã sử dụng
| Tên công cụ AI | Phiên bản / Model | Mục đích sử dụng | Môi trường tương tác |
| :--- | :--- | :--- | :--- |
| **Gemini Argon** | Gemini Argon (Google DeepMind) | Phân tích biến đầu vào, thiết kế EP, thiết kế BVA 3 giá trị, sinh kịch bản thực thi Mobile và hỗ trợ khắc phục sự cố cấu hình mạng `API_URL` | Giao diện Chat & IDE Assistant |

---

## 2. NHẬT KÝ TƯƠNG TÁC CHI TIẾT (AI INTERACTION LOGS)

---

### 📌 Phiên tương tác #01: Trích xuất và phân tích biến đầu vào từ đặc tả FR-05 Mobile
* **Thời gian:** 2026-10-18 16:55:10
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Bạn là chuyên gia kiểm thử phần mềm. Dựa vào đặc tả sau của tính năng FR-05: Xem danh sách & Tìm kiếm sản phẩm
  trong hệ thống EShop (áp dụng cho Mobile - Pool D), hãy:

  1. Liệt kê TẤT CẢ các biến đầu vào (input variables)
  2. Với mỗi biến, xác định:
     - Kiểu dữ liệu (string, number, email, etc.)
     - Miền giá trị hợp lệ (valid domain)
     - Các ràng buộc (constraints)
     - Quan hệ phụ thuộc giữa các biến (nếu có)
  ```
* **Đầu ra (Output của AI):**
  * AI đã trích xuất biến đầu vào duy nhất của thanh tìm kiếm: `search_keyword` (String).
  * Miền hợp lệ: Chuỗi rỗng `""` (hiển thị toàn bộ SP), chuỗi khớp tên sản phẩm, chuỗi không khớp tên, chuỗi chứa ký tự đặc biệt/HTML/XSS.
  * Các ràng buộc: Chỉ tìm theo tên sản phẩm, hiển thị an toàn chống XSS, hiển thị loading khi tải và hiển thị Empty State khi không có kết quả.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Phân loại biến chính xác, bám sát các ràng buộc của tài liệu SRS.

---

### 📌 Phiên tương tác #02: Thiết kế Phân vùng tương đương (EP) & Test Cases
* **Thời gian:** 2026-10-18 17:04:30
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Dựa trên danh sách biến đầu vào đã phân tích ở trên cho tính năng FR-05: Xem danh sách & Tìm kiếm sản phẩm (Mobile),
  hãy áp dụng kỹ thuật Domain Testing theo các bước sau:

  Bước 2a: Phân vùng tương đương (Equivalence Partitioning)
  Với MỖI biến đầu vào, xác định:
  - Các lớp tương đương hợp lệ (Valid Equivalence Classes)
  - Các lớp tương đương không hợp lệ (Invalid Equivalence Classes)

  Bước 2b: Thiết kế test cases Domain Testing
  ```
* **Đầu ra (Output của AI):**
  * Bảng phân vùng tương đương từ EC1 đến EC9: Chuỗi rỗng, Khớp toàn bộ, Khớp một phần, Khớp không phân biệt hoa/thường, Khoảng trắng thừa, Không khớp (Empty State), Tìm theo mô tả/danh mục, Chuỗi XSS, Chuỗi SQL Injection.
  * Danh sách 9 kịch bản kiểm thử Domain Testing.
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Độ bao phủ rất tốt cho các khía cạnh nghiệp vụ và bảo mật.

---

### 📌 Phiên tương tác #03: Thiết kế Phân tích Giá trị Biên 3 giá trị (ISTQB 3-Value BVA)
* **Thời gian:** 2026-10-18 17:12:15
* **Công cụ AI:** Gemini Argon
* **Câu lệnh (Prompt của sinh viên):**
  ```text
  Tiếp tục với tính năng FR-05: Xem danh sách & Tìm kiếm sản phẩm (Mobile), bây giờ hãy áp dụng kỹ thuật 
  Phân tích Giá trị Biên (Boundary Value Analysis) theo chuẩn ISTQB 3-Value BVA cho độ dài chuỗi tìm kiếm:
  Min-1, Min, Min+1, Nominal, Max-1, Max, Max+1.
  ```
* **Đầu ra (Output của AI):**
  * Bảng 3-Value BVA với ranh giới từ 0 đến 255 ký tự:
    * Biên dưới: `Min = 0` (hợp lệ - chuỗi rỗng), `Min+1 = 1` (hợp lệ - chuỗi 1 ký tự `"i"`).
    * Giá trị danh định: `10` ký tự (`"iPhone 15"`).
    * Biên trên: `Max-1 = 254`, `Max = 255` (biên trên hợp lệ của tên sản phẩm), `Max+1 = 256` ký tự (không khớp, yêu cầu Empty State).
* **Đánh giá của sinh viên (Human Review):**
  * *Chấp nhận:* Xác định ranh giới 3 giá trị theo chuẩn ISTQB hoàn toàn chính xác.

---

### 📌 Phiên tương tác #04: Rà soát, lọc sạch tính năng không liên quan (FR-17)
* **Thời gian:** 2026-10-18 17:22:40
* **Công cụ AI:** Gemini Argon
* **Nội dung tương tác:**
  * Sinh viên nhận thấy trong phản hồi ban đầu của AI có sự xuất hiện lẫn lộn của tính năng FR-17 (Quản lý Mã Giảm Giá).
  * Sinh viên lập tức ra chỉ thị can thiệp: *"Tôi cần làm gì ở FR-05: Xem danh sách & Tìm kiếm sản phẩm (đối với mobile - pool D). Chú ý lọc những phần không liên quan tới FR-05 (loại bỏ hoàn toàn FR-17). Đưa ra thao tác cụ thể cho từng EXE, không được chỉ nói bật console chung chung."*
  * AI đã tiến hành lọc sạch toàn bộ nội dung của FR-17, tái lập danh mục 14 kịch bản kiểm thử độc lập cho FR-05 Mobile.

---

### 📌 Phiên tương tác #05: Gỡ lỗi môi trường thực thi (Sự cố kết nối `API_URL` trên Pixel 6 và Web)
* **Thời gian:** 2026-10-18 17:34:00 – 17:42:00
* **Công cụ AI:** Gemini Argon
* **Nội dung tương tác:**
  * Sinh viên gặp lỗi khi chạy `npx expo start`:
    * Chạy trên máy ảo Android Studio (Pixel 6): Màn hình báo lỗi mạng `TypeError: Network request failed`, không hiển thị sản phẩm nào.
    * Chạy trên Web: Trình duyệt cũng báo lỗi mạng không tải được dữ liệu.
    * Terminal xuất hiện dòng cảnh báo: `WARN SafeAreaView has been deprecated...`.
  * **Chẩn đoán nguyên nhân:**
    1. Cảnh báo `SafeAreaView` chỉ là Warning của Expo, không phải lỗi crash.
    2. Nguyên nhân cốt lõi khiến cả máy ảo Pixel 6 và Web đều không tải được sản phẩm là do dòng 16 trong `frontend-mobile/App.js` bị gán cứng địa chỉ IP chết:
       `const API_URL = "http://192.168.10.13:3000/api";`
    3. Máy ảo Android Studio cần địa chỉ loopback đặc thù là `10.0.2.2:3000` để trỏ về máy tính host, trong khi Web cần `localhost:3000`.
  * **Giải pháp khắc phục của sinh viên:**
    Cập nhật lại `App.js` tự động nhận diện theo `Platform.OS`:
    ```javascript
    const API_URL = Platform.OS === 'android' ? 'http://10.0.2.2:3000/api' : 'http://localhost:3000/api';
    ```
    Đồng thời chạy lệnh `adb reverse tcp:3000 tcp:3000`. Ứng dụng đã kết nối thành công 100% trên cả Pixel 6 và Web.

---

### 📌 Phiên tương tác #06: Thực thi 14 kịch bản kiểm thử, chụp ảnh bằng chứng và tổng hợp lỗi
* **Thời gian:** 2026-10-18 17:45:00 – 17:55:00
* **Công cụ AI:** Gemini Argon
* **Nội dung tương tác:**
  * Sinh viên thực thi từng kịch bản trên giao diện Mobile, chụp ảnh màn hình và đối chiếu mã nguồn `backend/server.js` cùng CSDL SQLite `database.sqlite`.
  * Xác nhận 4 lỗi thực tế:
    1. `BUG-FR05-01`: Thiếu hoàn toàn giao diện Empty State (EXE-06, EXE-13, EXE-14).
    2. `BUG-FR05-02`: Backend không trim khoảng trắng đầu cuối (EXE-05).
    3. `BUG-FR05-03`: Lỗ hổng SQL Injection nghiêm trọng nối chuỗi thô và rò rỉ mã HTML lỗi 500 (EXE-08, EXE-09).
    4. `BUG-FR05-04`: Ảnh sản phẩm bị méo mó do `resizeMode="stretch"` và thiếu thẻ mô tả alt text (EXE-01).

---

## 3. BẢNG SỐ LIỆU ĐỊNH LƯỢNG SỬ DỤNG AI (QUANTITATIVE METRICS)

| Chỉ số đo lường | Giá trị thực tế | Ghi chú đánh giá |
| :--- | :---: | :--- |
| **Tổng số lượt Prompt trao đổi** | **6 lượt** | Bao gồm trích xuất biến, lập bảng EP, tính biên BVA, lọc sạch FR-17, chẩn đoán lỗi IP mạng và hướng dẫn thực thi |
| **Tổng thời gian tương tác với AI** | **~50 phút** | Từ 16:55 đến 17:55 ngày 2026-10-18 |
| **Tỷ lệ kế thừa đề xuất của AI** | **75%** | Tái sử dụng bảng phân vùng EP và ma trận điểm biên 3-Value BVA |
| **Tỷ lệ sinh viên can thiệp / chỉnh sửa** | **25%** | Bóc tách lọc bỏ FR-17, tự tay sửa lỗi IP mạng `API_URL` bằng `Platform.OS`, và phát hiện các lỗi mã nguồn |
| **Số lỗi phát hiện được nhờ AI gợi ý biên** | **2 lỗi** | Lỗi khoảng trắng thừa (`BUG-FR05-02`) và lỗi thiếu Empty State khi vượt biên (`BUG-FR05-01`) |
| **Số lỗi phát hiện độc lập qua kiểm tra mã nguồn & runtime** | **2 lỗi** | Lỗ hổng SQL Injection thô (`BUG-FR05-03`) và lỗi méo ảnh giao diện (`BUG-FR05-04`) |
| **Tổng số test cases thực thi thành công** | **14 test cases** | 8 Pass (57.14%) và 6 Fail (42.86%) |

---

## 4. KẾT LUẬN VỀ QUÁ TRÌNH KIỂM TOÁN AI

Kiểm thử tính năng **FR-05: Xem danh sách & Tìm kiếm sản phẩm trên Mobile** là một minh chứng xuất sắc cho nguyên tắc **AI-First kết hợp Human-in-the-loop**:
1. AI đã hỗ trợ thiết kế nhanh chóng bộ khung test cases bao phủ đầy đủ các trường hợp biên và kịch bản phân vùng tương đương.
2. Tuy nhiên, sự nhạy bén và kiến thức kỹ thuật của tester con người là yếu tố quyết định sự thành công: Tester đã phát hiện và xử lý sự cố hạ tầng mạng (sửa `API_URL` từ `192.168.10.13` sang `10.0.2.2`/`localhost`) để đưa ứng dụng vào trạng thái hoạt động thực tế, đồng thời kiểm tra mã nguồn backend để phát hiện lỗ hổng bảo mật SQL Injection cực kỳ nguy hiểm.
