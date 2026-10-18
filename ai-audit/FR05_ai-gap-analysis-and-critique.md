# PHÂN TÍCH KHOẢNG TRỐNG AI & PHÊ BÌNH AI
## TÍNH NĂNG FR-05: XEM DANH SÁCH & TÌM KIẾM SẢN PHẨM (MOBILE - POOL D)

* **Mã sinh viên:** 23120127
* **Họ và tên:** Dương Trọng Hòa
* **Tính năng:** FR-05: Xem danh sách & Tìm kiếm sản phẩm (Mobile - Pool D)

---

## PHẦN 1: PHÂN TÍCH KHOẢNG TRỐNG AI (AI GAP ANALYSIS)

Đề bài yêu cầu: *"Nếu các công cụ AI bỏ sót bất kỳ test case hoặc lỗi nào, bạn phải báo cáo và giải thích nguyên nhân do chất lượng prompt, hạn chế của AI hay độ phức tạp của tính năng."*

### 1.1 Những lỗi và tình huống thực tế mà AI đã bỏ sót hoặc suy đoán sai:

| STT | Vấn đề thực tế AI đã bỏ sót / Nhầm lẫn | Biểu hiện trên SUT thực tế | Lý do AI bỏ sót / Nhầm lẫn |
| :---: | :--- | :--- | :--- |
| **1** | **Lỗi cấu hình IP mạng cứng (`API_URL`) làm tê liệt ứng dụng** | `App.js` gán cứng `http://192.168.10.13:3000/api`, khiến máy ảo Pixel 6 và Web bị lỗi mạng `TypeError: Network request failed`, không tải được sản phẩm. | **Hạn chế không thể thực thi môi trường động:** AI giả định mã nguồn luôn kết nối đúng `localhost`, hoàn toàn không biết mã nguồn tồn tại một IP LAN chết. Tester phải tự chẩn đoán và sửa thành `Platform.OS` (`10.0.2.2` cho Android, `localhost` cho Web). |
| **2** | **Lỗ hổng SQL Injection thô và lộ mã HTML lỗi 500 (BUG-FR05-03)** | Backend nối chuỗi SQL thô (`LIKE '%${searchQuery}%'`), nhập `' OR 1=1 --` làm lộ hết sản phẩm, nhập `'` văng lỗi HTML `<h1>Database Error</h1>`. | **Thiên kiến tin cậy Backend:** AI mặc định các framework hiện đại luôn sử dụng Parameterized Query để chống SQLi và có bộ lọc Exception trả về JSON chuẩn, không ngờ Backend lại để lộ lỗ hổng sơ đẳng như vậy. |
| **3** | **Màn hình trắng trơn do thiếu Empty State (BUG-FR05-01)** | Khi tìm kiếm không có kết quả (`XYZ999`), màn hình Mobile dưới ô tìm kiếm hoàn toàn trắng tinh, không có thông báo hay icon nào. | **Mô hình hóa lý thuyết:** AI cho rằng khi không có dữ liệu thì hệ thống sẽ tự hiển thị thông báo "Không tìm thấy", nhưng không đọc mã nguồn để thấy `<FlatList>` thiếu thuộc tính `ListEmptyComponent`. |
| **4** | **Backend không cắt tỉa khoảng trắng (BUG-FR05-02)** | Nhập `"  iPhone  "` trả về 0 kết quả do Backend không gọi `.trim()`. | **Thiên kiến giả định tự động hóa:** AI nghĩ rằng tầng xử lý chuỗi của framework sẽ tự chuẩn hóa chuỗi đầu vào của người dùng. |
| **5** | **Ảnh sản phẩm bị méo và thiếu thẻ alt (BUG-FR05-04)** | Thẻ `<Image>` dùng `resizeMode="stretch"` làm méo khung hình sản phẩm và thiếu `accessibilityLabel`. | **Hạn chế thị giác:** AI chỉ phân tích văn bản logic, không nhìn thấy giao diện hiển thị thực tế trên màn hình điện thoại để phát hiện ảnh bị bóp méo. |

### 1.2 Phân loại nguyên nhân gốc rễ (Root Cause Analysis):
1. **Thiếu khả năng tương tác với hạ tầng runtime (50%):** AI hoàn toàn "mù" trước các vấn đề về cấu hình mạng thực tế (như gán cứng IP `192.168.10.13` hay khác biệt giữa IP máy host `10.0.2.2` trong máy ảo Android và `localhost` trên trình duyệt Web).
2. **Thiên kiến giả định hệ thống hoàn hảo (30%):** AI luôn tin rằng lập trình viên đã viết câu lệnh SQL an toàn và xử lý ngoại lệ bài bản.
3. **Vai trò sống còn của Human-in-the-loop (20%):** Tester con người phải trực tiếp cài đặt, debug lỗi kết nối mạng, quan sát giao diện thực tế và khai thác các kịch bản biên thì các lỗi thực tế mới được phơi bày.

---

## PHẦN 2: PHÊ BÌNH AI (AI CRITIQUE)
*(Độ dài yêu cầu: 200–300 từ — Bắt buộc theo Mục 10 đề bài)*

Quá trình áp dụng AI vào kiểm thử tính năng FR-05 trên nền tảng Mobile đã mang lại những bài học thực tiễn vô cùng sâu sắc về giới hạn của trí tuệ nhân tạo. Điểm yếu lớn nhất của AI là sự tách rời hoàn toàn khỏi môi trường thực thi runtime. Khi tôi khởi chạy ứng dụng Mobile trên máy ảo Pixel 6 và Expo Web, cả hai đều gặp lỗi kết nối mạng do mã nguồn bị gán cứng địa chỉ IP `192.168.10.13`. AI hoàn toàn không có khả năng nhận biết sự cố hạ tầng này, buộc tôi phải tự đọc log, can thiệp mã nguồn `App.js` và cấu hình lại địa chỉ `10.0.2.2` cùng `localhost` thì bài kiểm thử mới có thể tiến hành. Bên cạnh đó, AI mang nặng thiên kiến lạc quan khi mặc định rằng backend luôn sử dụng truy vấn tham số hóa (Parameterized Query) an toàn và frontend luôn trang bị sẵn giao diện Empty State. Chỉ khi tôi trực tiếp kiểm tra mã nguồn `server.js` và thực thi tấn công trên giao diện máy ảo, lỗ hổng SQL Injection nghiêm trọng cùng hiện tượng màn hình trắng tinh mới chính thức bị vạch trần. Dẫu vậy, AI vẫn là một trợ lý đắc lực trong việc cấu trúc hóa ma trận phân vùng tương đương và tính toán chuẩn xác các mốc biên độ dài 3-Value BVA. Tóm lại, AI chỉ phát huy hiệu quả khi đóng vai trò hỗ trợ gợi ý; người kiểm thử bắt buộc phải là nhân tố trung tâm trực tiếp gỡ lỗi và thẩm định chất lượng phần mềm.

*(Số từ: 254 từ — Đáp ứng chuẩn xác khoảng quy định 200–300 từ)*
