# BÃO CÃO CHI TIáº¾T DANH SÃCH Lá»–I (BUG REPORT)
## TÃNH NÄ‚NG FR-17: QUáº¢N LÃ MÃƒ GIáº¢M GIÃ (COUPON CRUD) (POOL C)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Tá»•ng sá»‘ lá»—i phÃ¡t hiá»‡n:** 5 nhÃ³m lá»—i (1 Critical, 3 High, 1 Low/Medium)
* **NgÃ y láº­p bÃ¡o cÃ¡o:** 2026-10-18

---

## DANH SÃCH CHI TIáº¾T CÃC Lá»–I ÄÃƒ PHÃT HIá»†N

---

### ðŸš¨ BUG #1: BUG-FR17-01 (CRITICAL - SECURITY / BROKEN ACCESS CONTROL)

* **TiÃªu Ä‘á» Issue:** `[FR-17][SEC-03][CRITICAL] Lá»— há»•ng Broken Access Control cho phÃ©p ngÆ°á»i dÃ¹ng thÆ°á»ng tá»± táº¡o vÃ  xÃ³a mÃ£ giáº£m giÃ¡`
* **NhÃ£n (Labels):** `bug`, `critical`, `security`, `FR-17`, `access-control`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Critical (Kiá»ƒm soÃ¡t truy cáº­p bá»‹ phÃ¡ vá»¡)
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Urgent / Immediate
* **MÃ´i trÆ°á»ng:** Chrome / Windows / EShop Local (Backend API `http://localhost:3000`)

#### 1. MÃ´ táº£ lá»—i
Theo Ä‘áº·c táº£ FR-12 vÃ  SEC-03: *"Táº¥t cáº£ cÃ¡c chá»©c nÄƒng quáº£n trá»‹ trong Web Admin (bao gá»“m thÃªm má»›i vÃ  xÃ³a mÃ£ giáº£m giÃ¡) chá»‰ dÃ nh riÃªng cho tÃ i khoáº£n cÃ³ vai trÃ² Quáº£n trá»‹ viÃªn (role = 'admin'). NgÆ°á»i dÃ¹ng thÃ´ng thÆ°á»ng (role = 'user') hoÃ n toÃ n bá»‹ cáº¥m truy cáº­p"*.
Tuy nhiÃªn, táº¡i `backend/server.js`:
- Endpoint `POST /api/admin/coupons` (dÃ²ng 457) vÃ  `DELETE /api/admin/coupons/:id` (dÃ²ng 483) chá»‰ sá»­ dá»¥ng middleware `authenticateToken` Ä‘á»ƒ kiá»ƒm tra Ä‘Äƒng nháº­p mÃ  **hoÃ n toÃ n khÃ´ng kiá»ƒm tra `req.user.role === 'admin'`**.

Háº­u quáº£: TÃ i khoáº£n ngÆ°á»i dÃ¹ng bÃ¬nh thÆ°á»ng (`test@eshop.com`, `role = 'user'`) cÃ³ thá»ƒ gá»­i HTTP request táº¡o mÃ£ giáº£m giÃ¡ 1.000.000 â‚« hoáº·c xÃ³a sáº¡ch toÃ n bá»™ mÃ£ giáº£m giÃ¡ cá»§a há»‡ thá»‘ng, gÃ¢y thiá»‡t háº¡i tÃ i chÃ­nh nghiÃªm trá»ng!

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. ÄÄƒng nháº­p vá»›i tÃ i khoáº£n ngÆ°á»i dÃ¹ng thÆ°á»ng: `test@eshop.com` / `Test1234!` Ä‘á»ƒ láº¥y JWT Token (`role = 'user'`).
2. Gá»­i request `POST /api/admin/coupons` vá»›i Header `Authorization: Bearer <user_token>` vÃ  payload:
```json
{
  "code": "HACKER1000K",
  "discount_type": "fixed",
  "discount_value": 1000000,
  "min_order_amount": 0,
  "max_uses_per_user": 10
}
```
3. Quan sÃ¡t káº¿t quáº£ API vÃ  danh sÃ¡ch mÃ£ trong há»‡ thá»‘ng.
4. Gá»­i request `DELETE /api/admin/coupons/:id` vá»›i token user thÆ°á»ng Ä‘á»ƒ xÃ³a coupon cá»§a há»‡ thá»‘ng.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u vá»›i mÃ£ lá»—i **HTTP 403 Forbidden** theo chuáº©n an ninh phÃ¢n quyá»n FR-12 vÃ  SEC-03.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
API tráº£ vá» **HTTP 200 OK**, táº¡o thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ vÃ  cho phÃ©p user thÆ°á»ng xÃ³a mÃ£ giáº£m giÃ¡ cá»§a Admin.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng táº¡o mÃ£:** [screenshots/pool C/EXE-04_Bug_Critical_NonAdmin_Create_Coupon.png](../screenshots/pool%20C/EXE-04_Bug_Critical_NonAdmin_Create_Coupon.png)
* Minh chá»©ng xÃ³a mÃ£ trÃ¡i phÃ©p: [screenshots/pool C/EXE-05_Bug_Critical_NonAdmin_Delete_Coupon.png](../screenshots/pool%20C/EXE-05_Bug_Critical_NonAdmin_Delete_Coupon.png)

---

### ðŸ› BUG #2: BUG-FR17-02 (High - Crash / Error Handling)

* **TiÃªu Ä‘á» Issue:** `[FR-17][HIGH] Backend gáº·p sá»± cá»‘ sáº­p HTTP 500 do Unhandled SQLite Constraint khi táº¡o mÃ£ giáº£m giÃ¡ bá»‹ trÃ¹ng láº·p`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-17`, `crash`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
TrÆ°á»ng `code` trong báº£ng `coupons` cÃ³ rÃ ng buá»™c duy nháº¥t (`UNIQUE`). Khi ngÆ°á»i dÃ¹ng nháº­p má»™t mÃ£ code Ä‘Ã£ tá»“n táº¡i trong CSDL (vÃ­ dá»¥: `"WELCOME10"`), Backend khÃ´ng báº¯t lá»—i xÃ¡c thá»±c mÃ  Ä‘á»ƒ CSDL SQLite vÄƒng ngoáº¡i lá»‡ thÃ´ `SQLITE_CONSTRAINT: UNIQUE constraint failed: coupons.code`, lÃ m sáº­p luá»“ng xá»­ lÃ½ vÃ  tráº£ vá» HTTP 500 Internal Server Error.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. ÄÄƒng nháº­p tÃ i khoáº£n Admin, Ä‘iá»u hÆ°á»›ng vÃ o trang Quáº£n lÃ½ mÃ£ giáº£m giÃ¡ (`http://localhost:5174/coupons`).
2. Nháº­p mÃ£ code Ä‘Ã£ cÃ³ sáºµn: `code: "WELCOME10"`.
3. Nháº­p cÃ¡c trÆ°á»ng dá»¯ liá»‡u há»£p lá»‡ khÃ¡c vÃ  báº¥m **ThÃªm MÃ£ Giáº£m GiÃ¡**.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng xá»­ lÃ½ lá»—i thÃ¢n thiá»‡n, tráº£ vá» mÃ£ lá»—i **HTTP 400 Bad Request** hoáº·c **409 Conflict** kÃ¨m thÃ´ng Ä‘iá»‡p *"MÃ£ giáº£m giÃ¡ Ä‘Ã£ tá»“n táº¡i"*.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Backend tráº£ vá» **HTTP 500 Internal Server Error** vá»›i mÃ£ lá»—i SQLite unhandled.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool C/EXE-07_Bug_Duplicate_Code_500_Error.png](../screenshots/pool%20C/EXE-07_Bug_Duplicate_Code_500_Error.png)

---

### ðŸ› BUG #3: BUG-FR17-03 (High - Data Validation & Boundary Violations)

* **TiÃªu Ä‘á» Issue:** `[FR-17][HIGH] Backend khÃ´ng kiá»ƒm tra rÃ ng buá»™c sá»‘ dÆ°Æ¡ng cho discount_value, min_order_amount vÃ  max_uses_per_user`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-17`, `validation`, `boundary`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Theo Ä‘áº·c táº£ nghiá»‡p vá»¥ FR-17:
- `discount_value`: Pháº£i lÃ  sá»‘ dÆ°Æ¡ng $> 0$.
- `min_order_amount`: Pháº£i lÃ  sá»‘ nguyÃªn khÃ´ng Ã¢m $\ge 0$.
- `max_uses_per_user`: Pháº£i lÃ  sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$.

Tuy nhiÃªn, Backend `server.js` hoÃ n toÃ n khÃ´ng cÃ³ táº§ng kiá»ƒm tra dá»¯ liá»‡u Ä‘áº§u vÃ o (Input Validation), cháº¥p nháº­n cÃ¡c giÃ¡ trá»‹ vi pháº¡m biÃªn nghiÃªm trá»ng:
1. Cho phÃ©p `discount_value = 0` (mÃ£ giáº£m 0 Ä‘á»“ng).
2. Cho phÃ©p `discount_value = -50000` (giÃ¡ trá»‹ Ã¢m).
3. Cho phÃ©p `min_order_amount = -10000` (Ä‘Æ¡n tá»‘i thiá»ƒu Ã¢m).
4. Cho phÃ©p `max_uses_per_user = 0` (lÆ°á»£t dÃ¹ng báº±ng 0). Äáº·c biá»‡t, khi F5 Web Admin thÃ¬ giao diá»‡n láº¡i tá»± Ä‘á»™ng hiá»ƒn thá»‹ sá»‘ 1 (do fallback `max_uses || 1`), gÃ¢y báº¥t nháº¥t dá»¯ liá»‡u nghiÃªm trá»ng giá»¯a CSDL vÃ  UI.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Gá»­i request `POST /api/admin/coupons` vá»›i cÃ¡c giÃ¡ trá»‹ vi pháº¡m biÃªn:
   - Request 1: `discount_value = 0`
   - Request 2: `discount_value = -50000`
   - Request 3: `min_order_amount = -10000`
   - Request 4: `max_uses_per_user = 0`
2. Kiá»ƒm tra pháº£n há»“i API vÃ  CSDL SQLite.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Backend pháº£i tá»« chá»‘i cÃ¡c giÃ¡ trá»‹ khÃ´ng há»£p lá»‡ vá»›i mÃ£ lá»—i **HTTP 400 Bad Request**.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Táº¥t cáº£ cÃ¡c request trÃªn Ä‘á»u Ä‘Æ°á»£c Backend cháº¥p nháº­n vá»›i **HTTP 200 OK** vÃ  lÆ°u vÃ o SQLite.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng má»©c giáº£m báº±ng 0:** [screenshots/pool C/EXE-08_Bug_BVA_DiscountValue_Zero_Accepted.png](../screenshots/pool%20C/EXE-08_Bug_BVA_DiscountValue_Zero_Accepted.png)
* Minh chá»©ng má»©c giáº£m Ã¢m: [screenshots/pool C/EXE-09_Bug_Negative_DiscountValue_Accepted.png](../screenshots/pool%20C/EXE-09_Bug_Negative_DiscountValue_Accepted.png)
* Minh chá»©ng Ä‘Æ¡n tá»‘i thiá»ƒu Ã¢m: [screenshots/pool C/EXE-11_Bug_BVA_MinOrderAmount_Negative_Accepted.png](../screenshots/pool%20C/EXE-11_Bug_BVA_MinOrderAmount_Negative_Accepted.png)
* Minh chá»©ng max uses báº±ng 0: [screenshots/pool C/EXE-12_Bug_BVA_MaxUses_Zero_Accepted.png](../screenshots/pool%20C/EXE-12_Bug_BVA_MaxUses_Zero_Accepted.png)

---

### ðŸ› BUG #4: BUG-FR17-04 (High - Business Logic / Boundary Violation)

* **TiÃªu Ä‘á» Issue:** `[FR-17][HIGH] Há»‡ thá»‘ng cho phÃ©p táº¡o mÃ£ giáº£m giÃ¡ loáº¡i pháº§n trÄƒm vÆ°á»£t quÃ¡ 100% (150%)`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-17`, `business-logic`, `boundary`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Äá»‘i vá»›i mÃ£ giáº£m giÃ¡ loáº¡i pháº§n trÄƒm (`discount_type = 'percent'`), ranh giá»›i nghiá»‡p vá»¥ chuáº©n theo Ä‘áº·c táº£ lÃ  tá»« $1\%$ Ä‘áº¿n $100\%$.
Táº¡i ranh giá»›i kiá»ƒm thá»­ Max + 1 ($101\%$, $150\%$), Backend khÃ´ng kiá»ƒm tra cháº·n biÃªn trÃªn, cho phÃ©p táº¡o mÃ£ giáº£m giÃ¡ **150%** thÃ nh cÃ´ng. Äiá»u nÃ y lÃ m sai lá»‡ch toÃ n bá»™ cÃ´ng thá»©c tÃ­nh toÃ¡n tÃ i chÃ­nh khi Ã¡p dá»¥ng mÃ£ vÃ o Ä‘Æ¡n hÃ ng.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Gá»­i request `POST /api/admin/coupons` vá»›i payload:
```json
{
  "code": "OVER100",
  "discount_type": "percent",
  "discount_value": 150,
  "min_order_amount": 100000
}
```
2. Kiá»ƒm tra CSDL SQLite.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u vá»›i mÃ£ lá»—i **HTTP 400 Bad Request** kÃ¨m thÃ´ng bÃ¡o pháº§n trÄƒm giáº£m giÃ¡ chá»‰ Ä‘Æ°á»£c náº±m trong khoáº£ng tá»« 1% Ä‘áº¿n 100%.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Backend cháº¥p nháº­n vÃ  lÆ°u mÃ£ giáº£m giÃ¡ 150% vÃ o CSDL SQLite vá»›i mÃ£ pháº£n há»“i HTTP 200 OK.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool C/EXE-10_Bug_BVA_Percent_Over100_Accepted.png](../screenshots/pool%20C/EXE-10_Bug_BVA_Percent_Over100_Accepted.png)

---

### ðŸ› BUG #5: BUG-FR17-05 (Low/Medium - RESTful API Standards)

* **TiÃªu Ä‘á» Issue:** `[FR-17][MEDIUM] XÃ³a mÃ£ giáº£m giÃ¡ vá»›i ID khÃ´ng tá»“n táº¡i tráº£ vá» HTTP 200 OK thay vÃ¬ 404 Not Found`
* **NhÃ£n (Labels):** `bug`, `medium`, `FR-17`, `api-standards`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Low / Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Low

#### 1. MÃ´ táº£ lá»—i
Khi thá»±c hiá»‡n xÃ³a má»™t tÃ i nguyÃªn khÃ´ng tá»“n táº¡i thÃ´ng qua phÆ°Æ¡ng thá»©c HTTP DELETE, chuáº©n thiáº¿t káº¿ RESTful API yÃªu cáº§u há»‡ thá»‘ng pháº£i kiá»ƒm tra xem báº£n ghi cÃ³ bá»‹ áº£nh hÆ°á»Ÿng hay khÃ´ng (`this.changes > 0`). Náº¿u khÃ´ng tÃ¬m tháº¥y, há»‡ thá»‘ng pháº£i tráº£ vá» mÃ£ lá»—i `404 Not Found`.
Táº¡i `backend/server.js` (dÃ²ng 483-490):
```javascript
db.run("DELETE FROM coupons WHERE id = ?", [id], function (err) {
  if (err) return res.status(500).json({ error: err.message });
  res.json({ message: "Coupon deleted" });
});
```
HÃ m callback hoÃ n toÃ n khÃ´ng kiá»ƒm tra `this.changes`. Do Ä‘Ã³ dÃ¹ `id = 99999` khÃ´ng há» tá»“n táº¡i trong CSDL, API váº«n bÃ¡o xÃ³a thÃ nh cÃ´ng vá»›i HTTP 200 OK.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Gá»­i request `DELETE /api/admin/coupons/99999` vá»›i Header Admin JWT.
2. Quan sÃ¡t HTTP Status Code vÃ  Response Body.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng pháº£i kiá»ƒm tra sá»‘ dÃ²ng bá»‹ áº£nh hÆ°á»Ÿng vÃ  tráº£ vá» **HTTP 404 Not Found** kÃ¨m thÃ´ng bÃ¡o: `Coupon not found`.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
API tráº£ vá» **HTTP 200 OK** kÃ¨m thÃ´ng bÃ¡o: `{"message": "Coupon deleted"}`.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool C/EXE-13_Bug_Delete_NonExistent_ID_200OK.png](../screenshots/pool%20C/EXE-13_Bug_Delete_NonExistent_ID_200OK.png)

