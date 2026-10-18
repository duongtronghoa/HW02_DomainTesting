# HƯỚNG DẪN TẠO GITHUB ISSUES & MẪU BÁO CÁO BUG
## TÍNH NĂNG FR-05: XEM DANH SÁCH & TÌM KIẾM SẢN PHẨM (MOBILE - POOL D)

> **Quy định đề bài (Mục 6 & 14):**
> * Báo cáo tất cả các lỗi được phát hiện trong cả báo cáo Markdown và trên trang GitHub Issues của nhóm bạn.
> * Hãy nhớ đính kèm ảnh chụp màn hình lỗi vào mỗi GitHub issue.
> * Chụp lại ảnh màn hình trang GitHub Issue sau khi tạo để lưu vào thư mục nộp bài: `screenshots/github-issues/`.

---

## 🛠️ HƯỚNG DẪN THAO TÁC TRÊN GITHUB

1. Mở trình duyệt, truy cập vào kho lưu trữ GitHub bài tập của bạn.
2. Click vào tab **`Issues`** $\rightarrow$ Click nút xanh **`New issue`**.
3. Copy **Tiêu đề (Title)** và **Nội dung (Body Markdown)** của từng Bug dưới đây dán vào.
4. **Đính kèm ảnh:** Kéo thả file ảnh chụp màn hình tương ứng từ thư mục `screenshots/fr05/` vào khung soạn thảo của Issue.
5. Gắn nhãn (Labels) ở cột bên phải: `bug`, `FR-05`, `feature-D`, `mobile`.
6. Click **`Submit new issue`**.
7. Chụp lại ảnh màn hình trang GitHub Issue sau khi tạo lưu vào `screenshots/github-issues/` với tên dạng `Issue_BUG_FR05_01.png`.

---

## 📋 NỘI DUNG MẪU CHI TIẾT 4 GITHUB ISSUES (COPY & PASTE)

---

### 🚨 ISSUE #1: BUG-FR05-03 (CRITICAL - SECURITY / SQL INJECTION & HTML LEAK)

* **Title:** `[FR-05][SEC-01][CRITICAL] Lỗ hổng SQL Injection tại ô tìm kiếm sản phẩm và rò rỉ mã HTML lỗi 500 trên Mobile`
* **Labels:** `bug`, `critical`, `security`, `FR-05`, `sql-injection`
* **File ảnh cần kéo thả vào:** `screenshots/fr05/EXE-08_Bug_Critical_SQL_Injection.png` (kèm `EXE-09_Bug_SQL_Error_HTML_Leak.png`)

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR05-03
- **Tính năng:** FR-05: Xem danh sách & Tìm kiếm sản phẩm (Mobile) & SEC-01: Phòng chống SQL Injection
- **Mức độ nghiêm trọng (Severity):** Critical (Lỗ hổng bảo mật tối nghiêm trọng - Tiết lộ dữ liệu & Sập CSDL)
- **Mức độ ưu tiên (Priority):** Urgent / Immediate
- **Môi trường:** Expo React Native / Android Emulator (Pixel 6) & Web Mobile / Backend Node.js Express / CSDL SQLite

### 2. Mô tả lỗi
Theo đặc tả SEC-01:
> *"Hệ thống phải áp dụng Parameterized Query cho tất cả các câu truy vấn cơ sở dữ liệu để ngăn chặn triệt để tấn công SQL Injection."*

Tuy nhiên, tại `backend/server.js` (dòng 141-151), API `GET /api/products` nối chuỗi trực tiếp giá trị `req.query.search` vào câu truy vấn SQL:
```javascript
const query = `SELECT * FROM products WHERE name LIKE '%${searchQuery}%'`;
db.all(query, [], (err, rows) => { ... });
```

Hậu quả:
1. **Bypass bộ lọc dữ liệu (Data Leak):** Kẻ tấn công nhập `' OR 1=1 --` vào thanh tìm kiếm, mệnh đề `1=1` luôn đúng khiến CSDL trả về toàn bộ 5 sản phẩm của hệ thống thay vì 0 sản phẩm.
2. **Sập câu truy vấn & Lộ lỗi HTML thô:** Khi người dùng nhập dấu nháy đơn `'`, Backend văng lỗi `SQLITE_ERROR: unrecognized token: "'"` và trả về mã HTTP 500 kèm chuỗi HTML:
   `<h1>Database Error</h1><p>SQLITE_ERROR: unrecognized token: "'"</p>`
   Ứng dụng Mobile tại `App.js` dòng 97-104 không lọc mà render nguyên khối mã HTML lỗi thô này lên màn hình giao diện người dùng.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Mở ứng dụng EShop Mobile trên máy ảo Pixel 6 hoặc Expo Web.
2. Tại ô tìm kiếm, nhập chuỗi payload SQL Injection:
   `' OR 1=1 --`
3. Bấm nút **"Tìm"**: Quan sát thấy toàn bộ 5 sản phẩm hiển thị trên màn hình.
4. Xóa từ khóa, nhập đúng 1 dấu nháy đơn:
   `'`
5. Bấm nút **"Tìm"**: Màn hình xuất hiện khung đỏ hiển thị nguyên văn bản HTML thô `<h1>Database Error</h1>`.

### 4. Kết quả mong đợi (Expected Result)
- Backend phải sử dụng tham số an toàn (`Parameterized Query` với dấu `?`), xử lý từ khóa như chuỗi ký tự thuần túy.
- Chuỗi `' OR 1=1 --` phải trả về 0 kết quả hợp lệ.
- Ký tự nháy đơn không được làm sập câu truy vấn, không trả về mã HTML hay làm lộ cấu trúc CSDL nội bộ.

### 5. Kết quả thực tế (Actual Result)
- Payload `' OR 1=1 --` bypass bộ lọc và làm lộ toàn bộ sản phẩm.
- Ký tự nháy đơn làm Backend văng HTTP 500, Mobile in nguyên chuỗi HTML lỗi thô lên màn hình.

### 6. Bằng chứng kiểm thử (Screenshots)
![SQL Injection Bypass](screenshots/fr05/EXE-08_Bug_Critical_SQL_Injection.png)
![SQL Error HTML Leak](screenshots/fr05/EXE-09_Bug_SQL_Error_HTML_Leak.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Sửa câu lệnh truy vấn trong `backend/server.js` sử dụng Parameterized Query:
```javascript
app.get("/api/products", (req, res) => {
  const searchQuery = req.query.search ? req.query.search.trim() : "";
  if (searchQuery) {
    const query = "SELECT * FROM products WHERE name LIKE ?";
    db.all(query, [`%${searchQuery}%`], (err, rows) => {
      if (err) return res.status(500).json({ error: "Lỗi truy vấn cơ sở dữ liệu" });
      res.json(rows);
    });
  } else {
    db.all("SELECT * FROM products", [], (err, rows) => {
      if (err) return res.status(500).json({ error: "Lỗi máy chủ" });
      res.json(rows);
    });
  }
});
```
```

---

### ⚠️ ISSUE #2: BUG-FR05-01 (HIGH - MISSING EMPTY STATE UI COMPONENT)

* **Title:** `[FR-05][HIGH] Ứng dụng Mobile hoàn toàn thiếu màn hình Empty State khi tìm kiếm không có kết quả`
* **Labels:** `bug`, `high`, `ui/ux`, `FR-05`, `mobile`
* **File ảnh cần kéo thả vào:** `screenshots/fr05/EXE-06_Bug_Missing_Empty_State.png` (kèm `EXE-13_BVA_Max_255_Chars.png`)

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR05-01
- **Tính năng:** FR-05: Xem danh sách & Tìm kiếm sản phẩm (Mobile Empty State)
- **Mức độ nghiêm trọng (Severity):** High (Vi phạm trực tiếp yêu cầu giao diện cốt lõi của FR-05)
- **Mức độ ưu tiên (Priority):** High
- **Môi trường:** Expo React Native / Android Emulator (Pixel 6) & Web Mobile

### 2. Mô tả lỗi
Theo quy định rõ ràng trong đặc tả FR-05:
> *"Khi không có kết quả tìm kiếm phải hiển thị thông báo empty state phù hợp (có hình minh họa và thông báo rõ ràng/thân thiện)."*

Tuy nhiên, kiểm tra mã nguồn `frontend-mobile/App.js` (dòng 518-531):
```jsx
<FlatList
  data={products}
  renderItem={renderProduct}
  keyExtractor={(item, index) => item?.id ? item.id.toString() : index.toString()}
  contentContainerStyle={styles.productList}
/>
{products.length > 0 && (
  <Text style={styles.resultCount}>Hiển thị {products.length} sản phẩm</Text>
)}
```
Component `<FlatList>` hoàn toàn không khai báo thuộc tính `ListEmptyComponent`. Khi `products = []`, danh sách không hiển thị bất kỳ phần tử nào, khiến toàn bộ vùng màn hình bên dưới thanh tìm kiếm bị trắng tinh (Blank Screen), không có hình minh họa và không có bất kỳ dòng chữ thông báo nào.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Mở ứng dụng EShop Mobile.
2. Nhập từ khóa không tồn tại trong hệ thống: `XYZ999` (hoặc chuỗi dài 255 ký tự).
3. Bấm nút **"Tìm"**.
4. Quan sát vùng hiển thị danh sách sản phẩm.

### 4. Kết quả mong đợi (Expected Result)
Giao diện phải hiển thị một khối Empty State đẹp mắt, gồm icon/hình minh họa giỏ hàng rỗng/kính lúp và thông điệp:
`"Không tìm thấy sản phẩm nào phù hợp với từ khóa của bạn."`

### 5. Kết quả thực tế (Actual Result)
Màn hình hoàn toàn trắng trơn dưới thanh tìm kiếm, gây cảm giác ứng dụng bị đơ/treo cho người dùng.

### 6. Bằng chứng kiểm thử (Screenshots)
![Missing Empty State](screenshots/fr05/EXE-06_Bug_Missing_Empty_State.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Bổ sung `ListEmptyComponent` cho `<FlatList>` trong `frontend-mobile/App.js`:
```jsx
<FlatList
  data={products}
  renderItem={renderProduct}
  keyExtractor={(item) => item.id.toString()}
  ListEmptyComponent={
    !loadingProducts && (
      <View style={styles.emptyContainer}>
        <Text style={styles.emptyIcon}>🔍</Text>
        <Text style={styles.emptyTitle}>Không tìm thấy sản phẩm</Text>
        <Text style={styles.emptySubtitle}>Vui lòng thử lại với từ khóa khác</Text>
      </View>
    )
  }
/>
```
```

---

### ⚠️ ISSUE #3: BUG-FR05-02 (MEDIUM - UNTRIMMED WHITESPACE IN SEARCH QUERY)

* **Title:** `[FR-05][MEDIUM] Backend không cắt bỏ khoảng trắng thừa (untrimmed whitespace), khiến tìm kiếm thất bại`
* **Labels:** `bug`, `medium`, `backend`, `FR-05`, `data-handling`
* **File ảnh cần kéo thả vào:** `screenshots/fr05/EXE-05_Bug_Untrimmed_Whitespace_Fail.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR05-02
- **Tính năng:** FR-05: Tìm kiếm sản phẩm (Input Data Handling)
- **Mức độ nghiêm trọng (Severity):** Medium (Lỗi xử lý dữ liệu đầu vào người dùng)
- **Mức độ ưu tiên (Priority):** Medium
- **Môi trường:** Backend Node.js Express / Mobile App

### 2. Mô tả lỗi
Người dùng trên thiết bị di động thường vô tình gõ thêm dấu cách (khoảng trắng) ở đầu hoặc cuối từ khóa tìm kiếm (ví dụ do bàn phím ảo tự động thêm space sau từ).
Theo chuẩn xử lý dữ liệu biểu mẫu (Form Data Handling), hệ thống phải tự động cắt tỉa khoảng trắng ở hai đầu (`trim()`) trước khi thực hiện truy vấn.

Tuy nhiên, tại `backend/server.js` (dòng 142), giá trị `req.query.search` được gán trực tiếp vào câu lệnh SQL mà không gọi `.trim()`. 
Do đó, khi tìm kiếm chuỗi `"  iPhone  "`, câu truy vấn trở thành `WHERE name LIKE '%  iPhone  %'`. Vì tên sản phẩm trong CSDL không có khoảng trắng thừa, hệ thống trả về 0 kết quả dù sản phẩm "iPhone 15 Pro Max" đang tồn tại.

### 3. Các bước tái hiện (Steps to Reproduce)
1. Mở ứng dụng Mobile, nhập từ khóa có 2 dấu cách đầu và 2 dấu cách cuối:
   `"  iPhone  "`
2. Bấm nút **"Tìm"**.
3. Quan sát kết quả tìm kiếm.

### 4. Kết quả mong đợi (Expected Result)
Hệ thống tự động cắt bỏ khoảng trắng thừa, tìm kiếm từ khóa `"iPhone"` và hiển thị sản phẩm `iPhone 15 Pro Max`.

### 5. Kết quả thực tế (Actual Result)
Hệ thống không tìm thấy sản phẩm nào (trả về danh sách rỗng 0 sản phẩm).

### 6. Bằng chứng kiểm thử (Screenshots)
![Untrimmed Whitespace Fail](screenshots/fr05/EXE-05_Bug_Untrimmed_Whitespace_Fail.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Gọi `.trim()` trên biến `search` ở cả Frontend lẫn Backend:
```javascript
const searchQuery = req.query.search ? req.query.search.trim() : "";
```
```

---

### ℹ️ ISSUE #4: BUG-FR05-04 (LOW/MEDIUM - IMAGE ASPECT RATIO DISTORTION & MISSING ALT)

* **Title:** `[FR-05][GUI] Ảnh sản phẩm bị co giãn méo tỷ lệ (resizeMode stretch) và thiếu thuộc tính alt text mô tả`
* **Labels:** `bug`, `low`, `ui/ux`, `accessibility`, `FR-05`
* **File ảnh cần kéo thả vào:** `screenshots/fr05/EXE-01_GUI_Default_Product_List.png`

**Nội dung Markdown dán vào Issue:**
```markdown
### 1. Thông tin chung
- **Mã lỗi:** BUG-FR05-04
- **Tính năng:** FR-05: Xem danh sách sản phẩm (Product Card Rendering)
- **Mức độ nghiêm trọng (Severity):** Low / Medium (Vi phạm chuẩn giao diện và khả năng tiếp cận Accessibility)
- **Mức độ ưu tiên (Priority):** Medium
- **Môi trường:** Expo React Native / Mobile App

### 2. Mô tả lỗi
Đặc tả FR-05 nêu rõ:
> *"Mỗi sản phẩm hiển thị: Ảnh (tỷ lệ chuẩn, có alt text mô tả), Tên sản phẩm, Giá (đơn vị: ₫, định dạng phân cách hàng nghìn)."*

Tuy nhiên, kiểm tra component `<Image>` tại `frontend-mobile/App.js` dòng 457-461:
```jsx
<Image
  source={{ uri: item.imageUrl }}
  style={styles.productImage}
  resizeMode="stretch"
/>
```
1. **Méo tỷ lệ ảnh:** Thuộc tính `resizeMode="stretch"` ép ảnh phải giãn hết chiều ngang và chiều cao cố định (160px) bất kể kích thước gốc của ảnh, khiến hình ảnh sản phẩm (như laptop, điện thoại) bị bóp méo, mất cân đối thị giác nghiêm trọng.
2. **Thiếu thông tin mô tả:** Thẻ `<Image>` không khai báo thuộc tính `accessibilityLabel` (tương đương `alt text` trên web), vi phạm tiêu chuẩn tiếp cận người dùng khiếm thị khi dùng trình đọc màn hình (Screen Reader).

### 3. Các bước tái hiện (Steps to Reproduce)
1. Mở ứng dụng Mobile màn hình chính.
2. Quan sát hình ảnh của các sản phẩm như iPhone 15 Pro Max, MacBook Pro M3.
3. Nhận thấy các cạnh và khung máy bị kéo giãn bất thường do chế độ stretch.

### 4. Kết quả mong đợi (Expected Result)
- Ảnh sản phẩm phải giữ đúng tỷ lệ khung hình gốc (Aspect Ratio), sử dụng `resizeMode="contain"` hoặc `resizeMode="cover"`.
- Bổ sung `accessibilityLabel={item.name}` để cung cấp mô tả cho ảnh.

### 5. Kết quả thực tế (Actual Result)
Ảnh bị kéo giãn méo tỷ lệ và thiếu mô tả accessibility.

### 6. Bằng chứng kiểm thử (Screenshots)
![GUI Default Product List](screenshots/fr05/EXE-01_GUI_Default_Product_List.png)

### 7. Đề xuất khắc phục (Suggested Fix)
Cập nhật thuộc tính của `<Image>` trong `frontend-mobile/App.js`:
```jsx
<Image
  source={{ uri: item.imageUrl }}
  style={styles.productImage}
  resizeMode="contain"
  accessibilityLabel={item.name}
/>
```
```
