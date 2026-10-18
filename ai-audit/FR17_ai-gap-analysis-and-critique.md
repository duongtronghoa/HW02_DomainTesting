# PHÂN TÍCH KHOẢNG TRỐNG AI & PHÊ BÌNH AI
## TÍNH NĂNG FR-17: QUẢN LÝ MÃ GIẢM GIÁ (COUPON CRUD) (POOL C)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Tính năng:** FR-17: Quản lý Mã Giảm Giá (Coupon CRUD)

---

## PHẦN 1: PHÂN TÍCH KHOẢNG TRỐNG AI (AI GAP ANALYSIS)

Đề bài yêu cầu: *"Nếu các công cụ AI bỏ sót bất kỳ test case hoặc lỗi nào, bạn phải báo cáo và giải thích nguyên nhân do chất lượng prompt, hạn chế của AI hay độ phức tạp của tính năng."*

### 1.1 Những lỗi và tình huống thực tế mà AI đã bỏ sót hoặc suy đoán sai:

| STT | Vấn đề thực tế AI đã bỏ sót / Nhầm lẫn | Biểu hiện trên SUT thực tế | Lý do AI bỏ sót / Nhầm lẫn |
| :---: | :--- | :--- | :--- |
| **1** | **Lỗ hổng phân quyền Broken Access Control (BUG-FR17-01)** | Tài khoản thường (`test@eshop.com`) gọi thẳng API quản trị tạo và xóa mã giảm giá thành công với HTTP 200 OK. | **Thiên kiến giả định an ninh (Security Assumption Bias):** AI mặc định endpoint có tiền tố `/api/admin/*` đã được bảo vệ bởi middleware kiểm tra quyền Admin (`role === 'admin'`), không ngờ Backend chỉ xác thực token thông thường. |
| **2** | **Sập máy chủ với lỗi HTTP 500 khi trùng mã (BUG-FR17-02)** | Nhập mã `WELCOME10` đã có sẵn trong CSDL SQLite khiến Backend văng lỗi `SQLITE_CONSTRAINT: UNIQUE constraint failed` và trả về HTTP 500. | **Thiên kiến tin cậy Error Handling:** AI cho rằng hệ thống luôn xử lý ngoại lệ nghiệp vụ và trả về HTTP 400 hoặc 409, không lường trước Backend để lộ unhandled exception. |
| **3** | **Bỏ qua kiểm tra giá trị số và cận biên (BUG-FR17-03, BUG-FR17-04)** | Backend cho phép lưu `discount_value = 0`, `-50000`, `150%` và `min_order_amount = -10000` mà không hề validate. | **Hạn chế không thể đọc mã nguồn runtime:** AI chỉ thiết kế kịch bản lý thuyết nhưng không biết mã nguồn `server.js` hoàn toàn thiếu tầng kiểm tra dữ liệu đầu vào. |
| **4** | **Lỗi bất nhất giữa CSDL và giao diện hiển thị (BUG-FR17-03)** | Khi tạo mã với `max_uses_per_user = 0`, CSDL lưu đúng số 0 nhưng khi F5 Web Admin thì giao diện lại tự động hiển thị số 1. | **Hạn chế không thể thực thi giao diện (Dynamic Rendering Execution):** AI không thể phát hiện lỗi logic chuyển đổi dữ liệu ngầm (fallback value: `max_uses || 1`) trong mã nguồn React nếu không chạy thực tế trên trình duyệt. |
| **5** | **Nhầm lẫn giữa kiểm thử giao diện tĩnh và kiểm thử miền giá trị** | Kịch bản ban đầu của AI có kiểm tra màu sắc nút bấm và thẻ `<h1>` (vốn thuộc GUI Testing). | **Xu hướng bao quát quá rộng:** AI có thói quen đưa cả các yêu cầu kiểm thử giao diện chung (FR-21) vào kiểm thử miền giá trị, cần sinh viên rà soát loại bỏ để tập trung đúng trọng tâm bài tập. |

### 1.2 Phân loại nguyên nhân gốc rễ (Root Cause Analysis):
1. **Hạn chế bản chất của LLM (50%):** AI suy luận dựa trên xác suất và mô hình lý thuyết chuẩn, luôn giả định hệ thống có các cơ chế phòng vệ cơ bản (như RBAC, try-catch CSDL), do đó không thể tự suy đoán ra các lỗ hổng sơ đẳng trong mã nguồn.
2. **Thiếu khả năng thực thi động (30%):** Các lỗi bất nhất giữa CSDL SQLite và giao diện React (như lỗi hiển thị số 1 khi `max_uses = 0`) chỉ có thể được tìm thấy khi sinh viên trực tiếp thao tác F5 và đối chiếu dữ liệu.
3. **Vai trò quyết định của Human-in-the-loop (20%):** Tester con người đóng vai trò sống còn trong việc bóc tách ranh giới kiểm thử (loại bỏ GUI testing), nắn chỉnh bộ test cases theo 3-Value BVA và trực tiếp bắt các con bug bảo mật nghiêm trọng.

---

## PHẦN 2: PHÊ BÌNH AI (AI CRITIQUE)
*(Độ dài yêu cầu: 200–300 từ — Bắt buộc theo Mục 10 đề bài)*

Trong quá trình thực hiện kiểm thử tính năng FR-17 (Quản lý Mã Giảm Giá), sự phối hợp giữa người kiểm thử và AI đã bộc lộ rõ nét những ranh giới giữa lý thuyết và thực tiễn kiểm thử phần mềm. Về mặt hạn chế, AI thể hiện một "thiên kiến lạc quan" rất rõ rệt khi luôn giả định rằng các lập trình viên đã cài đặt đầy đủ các lớp bảo vệ tiêu chuẩn. AI không thể lường trước rằng một endpoint quản trị trọng yếu như `/api/admin/coupons` lại hoàn toàn bỏ quên bước kiểm tra vai trò Quản trị viên, cho phép người dùng thường tự ý tạo mã giảm giá một triệu đồng. Tương tự, AI cũng không lường trước việc CSDL SQLite bị sập văng lỗi 500 do trùng khóa duy nhất hay hiện tượng giao diện React tự động sửa giá trị hiển thị `max_uses_per_user` từ 0 thành 1 khi tải lại trang. Những khuyết tật nghiêm trọng này chỉ có thể bị vạch trần khi tôi trực tiếp kiểm tra mã nguồn Backend và thao tác trên môi trường runtime thực tế. Ở chiều ngược lại, AI đã phát huy năng lực vượt bậc trong việc cấu trúc hóa ma trận phân vùng tương đương và xác định chuẩn xác các điểm biên 3-Value BVA cho các biến số và tỷ lệ phần trăm. Bài học rút ra là: AI là một trợ lý đắc lực trong việc sinh khung kiểm thử nhanh chóng, nhưng tester con người bắt buộc phải giữ vai trò thẩm định tối hậu để phát hiện các lỗ hổng nghiệp vụ và an ninh cốt lõi.

*(Số từ: 258 từ — Đáp ứng chuẩn xác khoảng quy định 200–300 từ)*
