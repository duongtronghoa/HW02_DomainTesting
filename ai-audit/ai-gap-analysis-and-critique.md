# PHÂN TÍCH KHOẢNG TRỐNG AI & PHÊ BÌNH AI
## TÍNH NĂNG FR-04: QUẢN LÝ HỒ SƠ CÁ NHÂN

---

## PHẦN 1: PHÂN TÍCH KHOẢNG TRỐNG AI (AI GAP ANALYSIS)

Đề bài yêu cầu: *"Nếu các công cụ AI bỏ sót bất kỳ test case hoặc lỗi nào, bạn phải báo cáo và giải thích nguyên nhân do chất lượng prompt, hạn chế của AI hay độ phức tạp của tính năng."*

### 1.1 Những lỗi và tình huống thực tế mà AI đã bỏ sót:

| STT | Lỗi / Vấn đề thực tế AI đã bỏ sót | Biểu hiện trên SUT thực tế | Lý do AI bỏ sót |
| :---: | :--- | :--- | :--- |
| **1** | **Lỗi logic mâu thuẫn trong regex SĐT (BUG-FR04-01)** | EShop từ chối SĐT 10 số và 11 số hợp lệ và yêu cầu nhập "9-10 chữ số". | **Hạn chế của AI (Không có quyền thực thi SUT):** AI chỉ suy luận thuần túy theo văn bản đặc tả lý tưởng, không thể tự chạy code thực tế để biết lập trình viên của EShop đã viết sai regex trong mã nguồn frontend. |
| **2** | **Bỏ quên validation cho Địa chỉ rỗng (BUG-FR04-02)** | Hệ thống cho phép bấm lưu địa chỉ rỗng thành công vào SQLite. | **Thiên kiến giả định hoàn hảo (Assumption Bias):** AI có xu hướng mặc định rằng backend của một hệ thống luôn có tầng Data Validation cơ bản cho các trường bắt buộc, nên không cảnh báo sinh viên về sự thiếu vắng hoàn toàn của logic kiểm tra ở phía máy chủ. |
| **3** | **Không giới hạn độ dài 255 ký tự (BUG-FR04-03)** | Form và API cho phép nhập và lưu chuỗi 256 ký tự thành công. | **Hạn chế trong việc đọc DOM/CSS:** AI chỉ thiết kế giá trị biên $Max+1=256$, nhưng không thể tự kiểm tra xem thẻ `<input>` trên giao diện thực tế có thuộc tính `maxlength="255"` hay không. |
| **4** | **Lệch URL API thực tế (`/api/profile` vs `/api/users/me`)** | Gọi `PUT /api/profile` bị lỗi 404 Cannot PUT. | **Chất lượng đầu vào của Prompt ban đầu:** Prompt ban đầu của sinh viên chỉ đưa nội dung đặc tả tổng quan, không cung cấp file mã nguồn `server.js`, khiến AI suy đoán URL RESTful theo quán tính thông thường. |

### 1.2 Phân loại nguyên nhân gốc rễ (Root Cause Analysis):
1. **Do hạn chế cố hữu của LLM (60%):** Các mô hình ngôn ngữ lớn (LLM) không thể thay thế việc thực thi phần mềm động (Dynamic Execution). AI chỉ thấy "văn bản yêu cầu" chứ không thấy "hành vi thực thi lúc runtime".
2. **Do thiên kiến suy luận logic chuẩn (25%):** AI thường tư duy theo hướng "phần mềm được lập trình chuẩn mực", do đó thường đánh giá thấp các lỗi ngớ ngẩn (như quên đặt `required`, viết ngược regex, hoặc mở toang tham số `role` trong câu lệnh SQL).
3. **Do chất lượng ngữ cảnh trong Prompt (15%):** Nếu tester không cung cấp mã nguồn backend và chi tiết triển khai cụ thể, AI buộc phải phỏng đoán các endpoint API.

---

## PHẦN 2: PHÊ BÌNH AI (AI CRITIQUE)
*(Độ dài yêu cầu: 200–300 từ — Bắt buộc theo Mục 10 đề bài)*

Trong suốt quá trình thực hiện kiểm thử tính năng FR-04, sự cộng tác với AI đã bộc lộ rõ rệt cả ưu điểm vượt trội lẫn những lỗ hổng tư duy cố hữu của mô hình ngôn ngữ. Về mặt thiếu sót, AI thể hiện một thiên kiến suy đoán hoàn hảo (optimistic bias): nó luôn giả định hệ thống backend đã được thiết lập các lớp bảo vệ cơ bản. Điển hình là việc AI hoàn toàn không ngờ tới việc EShop cho phép lưu địa chỉ rỗng hay bỏ qua giới hạn 255 ký tự. Đặc biệt, AI không thể bắt được lỗi mâu thuẫn logic khi hệ thống từ chối số điện thoại 10–11 số và bắt nhập 9–10 số, bởi AI chỉ xử lý văn bản tĩnh chứ không có khả năng thực thi mã nguồn để quan sát phản hồi thực tế từ giao diện và cơ sở dữ liệu. Ngoài ra, AI còn suy đoán sai đường dẫn API thành `/api/profile` thay vì `/api/users/me` do thói quen áp đặt quy ước đặt tên thông thường. Bài học cốt lõi tôi rút ra về nguyên tắc cộng tác với AI trong kiểm thử phần mềm là: "AI là một trợ lý tổng hợp thông minh nhưng là một người kiểm thử mù quáng". Tester không bao giờ được chấp nhận kết quả của AI như một chiếc hộp đen mà bắt buộc phải giữ vai trò người phản biện (human-in-the-loop). Việc kiểm thử thực tế trên hệ thống (dynamic testing) và đối chiếu trực tiếp với cơ sở dữ liệu là ranh giới bất biến mà AI không thể thay thế con người.

*(Số từ: 254 từ — Đáp ứng chuẩn xác khoảng quy định 200–300 từ)*
