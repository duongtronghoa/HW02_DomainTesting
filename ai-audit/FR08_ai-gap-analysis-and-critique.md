# PHÂN TÍCH KHOẢNG TRỐNG AI & PHÊ BÌNH AI
## TÍNH NĂNG FR-08: THANH TOÁN (CHECKOUT) (POOL B)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Tính năng:** FR-08: Thanh toán (Checkout)

---

## PHẦN 1: PHÂN TÍCH KHOẢNG TRỐNG AI (AI GAP ANALYSIS)

Đề bài yêu cầu: *"Nếu các công cụ AI bỏ sót bất kỳ test case hoặc lỗi nào, bạn phải báo cáo và giải thích nguyên nhân do chất lượng prompt, hạn chế của AI hay độ phức tạp của tính năng."*

### 1.1 Những lỗi và tình huống thực tế mà AI đã bỏ sót hoặc suy đoán sai:

| STT | Vấn đề thực tế AI đã bỏ sót / Nhầm lẫn | Biểu hiện trên SUT thực tế | Lý do AI bỏ sót / Nhầm lẫn |
| :---: | :--- | :--- | :--- |
| **1** | **Lẫn lộn phạm vi nghiệp vụ giữa FR-08 và FR-09** | AI ban đầu tự ý đưa các ca kiểm thử mã giảm giá (Coupon C1–C5) vào FR-08. | **Hạn chế trong phân định ranh giới tính năng:** AI thấy trên trang Checkout có ô nhập mã giảm giá nên gộp chung cả nghiệp vụ FR-09 vào, cần sinh viên can thiệp bóc tách để tránh bị trừ điểm phạm vi. |
| **2** | **Ô sửa tổng tiền lộ thiên trên giao diện UI (BUG-FR08-01)** | Trang Checkout đặt sẵn một thẻ `<input type="number">` cho phép người dùng tự do sửa số tiền tùy ý. | **Thiên kiến giả định hoàn hảo (Assumption Bias):** AI mặc định rằng các lập trình viên sẽ luôn hiển thị tổng tiền ở dạng văn bản tĩnh (Text / Read-only), không ngờ frontend lại để ô nhập mở toang như vậy. |
| **3** | **Quên xóa sạch giỏ hàng sau khi thanh toán (BUG-FR08-02)** | Đặt hàng thành công nhưng 2 sản phẩm cũ vẫn còn nguyên trong giỏ hàng. | **Hạn chế không thể thực thi động (Dynamic Execution):** AI chỉ đọc văn bản quy định "giỏ hàng được xóa" nhưng không thể tự chạy runtime để biết hàm `handleCheckout` thiếu lời gọi `clearCart()`. |
| **4** | **Giao diện Checkout thiếu hoàn toàn ô Địa chỉ giao hàng (BUG-FR08-03)** | Form đặt hàng không hề có ô nhập địa chỉ, cũng không kế thừa địa chỉ người dùng. | **Mô hình hóa lý thuyết:** AI suy diễn form đặt hàng chuẩn luôn có ô địa chỉ (dấu `*`), nên chỉ thiết kế test case gửi chuỗi rỗng chứ không nhìn thấy giao diện thực tế bị thiếu trường. |
| **5** | **Backend chấp nhận giỏ hàng rỗng và số lượng âm (BUG-FR08-05, BUG-FR08-06)** | Backend nhận `items = []` và `quantity = -2`, trả về HTTP 200 và tạo đơn hàng thành công. | **Thiên kiến tin cậy Backend:** AI cho rằng máy chủ luôn có tầng kiểm tra dữ liệu đầu vào cơ bản (Schema Validation). |

### 1.2 Phân loại nguyên nhân gốc rễ (Root Cause Analysis):
1. **Hạn chế bản chất của LLM (55%):** AI suy luận dựa trên xác suất ngôn ngữ và giả định phần mềm được xây dựng đúng chuẩn, hoàn toàn "mù" trước các lỗi ngớ ngẩn trong mã nguồn giao diện (như để input number cho tổng tiền hay quên gọi `clearCart`).
2. **Xu hướng mở rộng phạm vi không kiểm soát (25%):** AI có xu hướng gộp các tính năng liên quan (FR-08 và FR-09) lại với nhau nếu tester không ra chỉ thị kiểm soát ranh giới nghiêm ngặt.
3. **Vai trò bắt buộc của Human-in-the-loop (20%):** Chỉ khi sinh viên trực tiếp thao tác trên trình duyệt, đọc mã nguồn và đối chiếu CSDL SQLite thì các con bug thực tế mới được phát hiện và xác nhận chính xác.

---

## PHẦN 2: PHÊ BÌNH AI (AI CRITIQUE)
*(Độ dài yêu cầu: 200–300 từ — Bắt buộc theo Mục 10 đề bài)*

Trong quá trình thực hiện kiểm thử tính năng FR-08 (Thanh toán), việc cộng tác với AI đã làm nổi bật hai khía cạnh đối lập sâu sắc trong kiểm thử phần mềm tự động hóa. Về mặt hạn chế, AI bộc lộ xu hướng lan man và lẫn lộn ranh giới nghiệp vụ: nó tự động đưa toàn bộ các điều kiện của mã giảm giá (FR-09) vào bảng kiểm thử FR-08, đòi hỏi tôi phải chủ động rà soát và yêu cầu bóc tách nghiêm ngặt. Nghiêm trọng hơn, AI thể hiện một thiên kiến lạc quan cố hữu khi luôn tin rằng các nhà phát triển tuân thủ quy chuẩn cơ bản. AI không thể lường trước việc giao diện EShop lại trang bị sẵn một ô `<input>` cho phép người mua tự gõ số tiền tùy ý, hay việc lập trình viên quên gọi hàm `clearCart()` sau khi thanh toán thành công. Những lỗi này chỉ có thể bị phơi bày thông qua việc thực thi động trực tiếp trên hệ thống thật và quan sát cơ sở dữ liệu SQLite. Tuy nhiên, AI đã chứng minh năng lực vượt trội trong việc phân tách miền giá trị và xây dựng bộ biên 3-Value BVA chuẩn ISTQB cho độ dài chuỗi và số lượng sản phẩm, cũng như hỗ trợ viết các câu lệnh PowerShell tương tác API chuẩn xác. Bài học cốt lõi rút ra là: tester không bao giờ được phó mặc hoàn toàn cho AI; con người bắt buộc phải giữ vai trò nhạc trưởng để kiểm soát phạm vi và thẩm định kết quả kiểm thử.

*(Số từ: 247 từ — Đáp ứng chuẩn xác khoảng quy định 200–300 từ)*
