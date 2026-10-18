# BÁO CÁO TỔNG KẾT BÀI TẬP VỀ NHÀ HW02
# KIỂM THỬ MIỀN GIÁ TRỊ TRÊN HỆ THỐNG ESHOP (DOMAIN TESTING ON ESHOP)

* **Môn học:** CS423 / CSC13003 – Kiểm thử phần mềm (Software Testing · AI-augmented 2026)
* **Khoa:** Công nghệ Thông tin – Trường Đại học Khoa học Tự nhiên, ĐHQG-HCM (FIT@HCMUS)
* **Mã bài tập:** HW02-AI
* **Họ và tên sinh viên:** Dương Trọng Hòa
* **Mã số sinh viên:** 23120127
* **Lớp:** 23CLC01
* **Hệ thống kiểm thử (SUT):** EShop Platform (`frontend-web` React, `frontend-mobile` Expo React Native, `backend` Express Node.js, SQLite `database.sqlite`)
* **Chính sách AI:** Tuân thủ nguyên tắc AI-First Strategy và Human Review bắt buộc (Cấp độ Bloom-AI: G9.2 & G9.3)
* **Tên file nộp bài (.zip):** `23120127_HW02_AI_DomainTesting_090.zip`

---

## 1. BẢNG TỰ ĐÁNH GIÁ ĐIỂM (SELF-ASSESSMENT TABLE)
*(Tuân thủ chính xác biểu mẫu tại Mục 15 của Đề bài)*

| STT (No.) | Tiêu chí đánh giá (Criteria) | Thang điểm (Grade) | Điểm tự đánh giá (Self-Assessed Grade) | Minh chứng hoàn thành |
| :---: | :--- | :---: | :---: | :--- |
| **1** | **Feature A (Domain + Boundary)**<br>*FR-04: Quản lý hồ sơ cá nhân (Pool A)* | 25 | **25** | Hoàn thành phân tích biến, thiết kế 16 test cases EP & 3-Value BVA, thực thi 100%, phát hiện 5 lỗi (1 Critical Privilege Escalation, 2 High, 2 Medium). Có 16 ảnh chụp màn hình bằng chứng. |
| **2** | **Feature B (Domain + Boundary)**<br>*FR-08: Thanh toán - Checkout (Pool B)* | 25 | **25** | Hoàn thành phân tích biến, thiết kế 13 test cases EP & 3-Value BVA, thực thi 100%, phát hiện 7 lỗi (1 Critical Price Tampering, 4 High, 2 Medium). Có 13 ảnh chụp màn hình bằng chứng. |
| **3** | **Feature C (Domain + Boundary)**<br>*FR-17: Quản lý mã giảm giá - Coupon CRUD (Pool C)* | 25 | **25** | Hoàn thành phân tích biến, thiết kế 14 test cases EP & 3-Value BVA, thực thi 100%, phát hiện 5 nhóm lỗi (1 Critical Broken Access Control, 3 High, 1 Medium). Có 14 ảnh chụp màn hình bằng chứng. |
| **4** | **Feature D (Mobile, Domain + Boundary)**<br>*FR-05: Xem danh sách & Tìm kiếm SP Mobile (Pool D)* | 15 | **15** | Hoàn thành phân tích biến, gỡ lỗi IP mạng loopback Android Emulator, thiết kế 14 test cases EP & 3-Value BVA, thực thi 100%, phát hiện 4 nhóm lỗi (1 Critical SQL Injection, 1 High, 1 Medium, 1 Low). Có 14 ảnh chụp màn hình bằng chứng. |
| **5** | **Agent Skills**<br>*(Kèm video demo YouTube)* | 10 | **0** | **Không thực hiện phần Agent Skills** (Sinh viên chủ động tự trừ 10 điểm theo quy định đề bài). |
| **TỔNG** | **Tổng điểm toàn bài (Total)** | **100** | **90** | **Điểm số tự đánh giá: 090/100** |

---

## 2. BÁO CÁO TỔNG HỢP KIỂM THỬ (TEST SUMMARY REPORT)
*(Tuân thủ chính xác yêu cầu tại Mục 14 của Đề bài)*

### 2.1 Các chỉ số kiểm thử tổng thể (Overall Testing Metrics)
* **Số lượng tính năng kiểm thử (Number of features):** **4 tính năng** (Bao phủ đầy đủ 4 Pools: Pool A, Pool B, Pool C, Pool D).
* **Tổng số ca kiểm thử thiết kế (Designed test cases):** **57 test cases**
* **Tổng số ca kiểm thử đã thực thi (Executed test cases):** **57 test cases** (Tỷ lệ thực thi đạt **100%**)
* **Số ca kiểm thử ĐẠT (Passed test cases):** **27 test cases** (**47.37%**)
* **Số ca kiểm thử THẤT BẠI (Failed test cases - Bắt được lỗi):** **30 test cases** (**52.63%**)
* **Số ca kiểm thử chưa thực thi (Not yet executed test cases):** **0 test cases** (**0.0%**)
* **Tổng số lỗi phát hiện và ghi nhận (Number of bugs):** **21 lỗi / nhóm lỗi** (trong đó có 4 lỗi bảo mật tối nghiêm trọng - Critical)
* **Video trình diễn (Demo videos):** **N/A** *(Sinh viên không thực hiện phần Agent Skills nên không có link video YouTube)*.

### 2.2 Bảng thống kê chi tiết theo từng tính năng

| Mã tính năng | Tên tính năng | Phân hệ (Pool) | Số TC thiết kế | Số TC thực thi | Passed | Failed | Số lỗi phát hiện | Lỗ hổng nghiêm trọng nhất |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **FR-04** | Quản lý hồ sơ cá nhân | Pool A (Web) | 16 | 16 (100%) | 10 (62.50%) | 6 (37.50%) | 5 | **Critical** (Privilege Escalation) |
| **FR-08** | Thanh toán (Checkout) | Pool B (Web) | 13 | 13 (100%) | 5 (38.46%) | 8 (61.54%) | 7 | **Critical** (Price Tampering) |
| **FR-17** | Quản lý mã giảm giá | Pool C (Web Admin) | 14 | 14 (100%) | 5 (35.71%) | 9 (64.29%) | 5 | **Critical** (Broken Access Control) |
| **FR-05** | Tìm kiếm sản phẩm | Pool D (Mobile Expo) | 14 | 14 (100%) | 7 (50.00%) | 7 (50.00%) | 4 | **Critical** (SQL Injection) |
| **TỔNG CỘNG** | **4 Tính năng** | **4 Pools** | **57** | **57 (100%)** | **27 (47.37%)** | **30 (52.63%)** | **21** | **4 Lỗ hổng Critical** |

---

## 3. CƠ CẤU TỔ CHỨC THƯ MỤC THỰC TẾ (DIRECTORY STRUCTURE)

Dưới đây là sơ đồ cấu trúc thư mục hiện tại của bài nộp sau khi đã được rà soát, dọn dẹp và chuẩn hóa:

```text
HW02_DomainTesting/
│
├── README.md                                          # [BẮT BUỘC] Báo cáo tổng kết & Bảng tự đánh giá điểm (90/100)
├── git-commit-log.txt                                 # [BẮT BUỘC] Nhật ký Git commit log xuất định dạng văn bản UTF-8
│
├── reports/                                           # [BẮT BUỘC] Báo cáo Domain Testing & BVA chính thức
│   ├── pool-A/
│   │   └── FR04_Domain_Testing_Report.md              # Báo cáo kỹ thuật tính năng FR-04 (Hồ sơ cá nhân)
│   ├── pool-B/
│   │   └── FR08_Domain_Testing_Report.md              # Báo cáo kỹ thuật tính năng FR-08 (Thanh toán Checkout)
│   ├── pool-C/
│   │   └── FR17_Domain_Testing_Report.md              # Báo cáo kỹ thuật tính năng FR-17 (Mã giảm giá CRUD)
│   └── pool-D/
│       └── FR05_Domain_Testing_Report.md              # Báo cáo kỹ thuật tính năng FR-05 (Mobile Tìm kiếm SP)
│
├── bugs/                                              # [BẮT BUỘC] Danh mục báo cáo lỗi theo chuẩn GitHub Issues
│   ├── FR04_Bug_Report.md                             # Báo cáo chi tiết 5 lỗi của tính năng FR-04
│   ├── FR08_Bug_Report.md                             # Báo cáo chi tiết 7 lỗi của tính năng FR-08
│   ├── FR17_Bug_Report.md                             # Báo cáo chi tiết 5 nhóm lỗi của tính năng FR-17
│   └── FR05_Bug_Report.md                             # Báo cáo chi tiết 4 nhóm lỗi của tính năng FR-05 Mobile
│
├── ai-audit/                                          # [BẮT BUỘC] Phụ lục Kiểm toán AI & Phê bình AI
│   ├── AI Audit Report.pdf                            # Báo cáo AI Audit chuẩn hóa theo Template FIT@HCMUS (PDF)
│   └── AI_Gap_Analysis_and_Critique_Consolidated.md   # Báo cáo Phân tích Khoảng trống AI & Phê bình AI Hợp nhất (263 từ)
│
└── screenshots/                                       # [BẮT BUỘC] Toàn bộ 57 ảnh chụp màn hình bằng chứng thực thi
    ├── pool A/                                        # 16 ảnh PNG minh chứng thực thi FR-04 (EXE-01 đến EXE-16)
    ├── pool B/                                        # 13 ảnh PNG minh chứng thực thi FR-08 (EXE-01 đến EXE-13)
    ├── pool C/                                        # 14 ảnh PNG minh chứng thực thi FR-17 (EXE-01 đến EXE-14)
    └── pool D/                                        # 14 ảnh PNG minh chứng thực thi FR-05 (EXE-01 đến EXE-14)
```

---

## 4. TÓM TẮT 4 LỖ HỔNG BẢO MẬT TỐI NGUY HIỂM ĐÃ PHÁT HIỆN

Trong quá trình thực thi kiểm thử trên hệ thống EShop thực tế, sinh viên đã phát hiện **21 lỗi**, trong đó đặc biệt nghiêm trọng là **4 lỗ hổng bảo mật mức Critical**:

1. **BUG-FR04-05 (Critical - Security / Privilege Escalation - FR-04):**
   * Endpoint `PUT /api/users/me` (dòng 124-127 `backend/server.js`) nhận trực tiếp tham số `role` từ body request và cập nhật thẳng vào CSDL SQLite mà không hề kiểm tra quyền hạn. Một tài khoản người dùng bình thường (`test@eshop.com`, `role = 'user'`) có thể tự gửi payload `{"role": "admin"}` để thăng quyền thành Quản trị viên và chiếm toàn quyền kiểm soát Web Admin.
2. **BUG-FR08-01 (Critical - Security / Price Tampering - FR-08):**
   * Trên giao diện Checkout (`Checkout.jsx`), ô Tổng tiền thanh toán là thẻ `<input type="number">` cho phép người dùng tự do gõ số tiền tùy ý; Backend `server.js` (dòng 299) không tự tính lại tổng tiền từ CSDL mà tin tưởng trực tiếp vào `total_amount` do client gửi lên. Kẻ gian có thể mua đơn hàng 2 sản phẩm cao cấp trị giá **36.000.000 ₫** nhưng chỉ thanh toán với số tiền **1.000 ₫**.
3. **BUG-FR17-01 (Critical - Security / Broken Access Control - FR-17):**
   * Các endpoint quản trị `POST /api/admin/coupons` và `DELETE /api/admin/coupons/:id` hoàn toàn thiếu bước kiểm tra vai trò Quản trị viên (`role === 'admin'`). Bất kỳ người dùng thông thường nào đã đăng nhập đều có thể tự tạo mã giảm giá 1.000.000 ₫ hoặc xóa toàn bộ mã giảm giá đang có trong hệ thống.
4. **BUG-FR05-03 (Critical - Security / SQL Injection & Raw HTML Leak - FR-05 Mobile):**
   * API tìm kiếm `GET /api/products` (dòng 144 `backend/server.js`) nối chuỗi trực tiếp giá trị tìm kiếm vào câu truy vấn SQLite (`WHERE name LIKE '%${searchQuery}%'`). Nhập payload `' OR 1=1 --` làm lộ sạch toàn bộ danh sách sản phẩm; nhập dấu nháy đơn `'` làm sập câu truy vấn văng lỗi HTTP 500 kèm nguyên khối mã HTML lỗi thô `<h1>Database Error</h1>` lên màn hình điện thoại.

---

## 5. HƯỚNG DẪN KIỂM TRA TRƯỚC KHI NỘP BÀI

1. **Quy tắc đặt tên file nén .zip:**
   * Tên file: **`23120127_HW02_AI_DomainTesting_090.zip`**
   * Trong đó: `23120127` là MSSV, `090` là 3 chữ số thể hiện điểm tự đánh giá ($90/100$).
2. **Nội dung bên trong file .zip:**
   * File `.zip` chứa trực tiếp các thư mục và file được liệt kê tại **Mục 3 (Cơ cấu tổ chức thư mục thực tế)**.
   * Sinh viên xuất các file Markdown chính trong `reports/` và `ai-audit/` ra định dạng `.pdf` trước khi nén để đáp ứng đầy đủ yêu cầu "Markdown + PDF" theo Mục 14 đề bài.
