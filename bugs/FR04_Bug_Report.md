# BÃO CÃO CHI TIáº¾T DANH SÃCH Lá»–I (BUG REPORT)
## TÃNH NÄ‚NG FR-04: QUáº¢N LÃ Há»’ SÆ  CÃ NHÃ‚N (POOL A)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Tá»•ng sá»‘ lá»—i phÃ¡t hiá»‡n:** 5 lá»—i (1 Critical, 2 High, 2 Medium)
* **NgÃ y láº­p bÃ¡o cÃ¡o:** 2026-10-18

---

## DANH SÃCH CHI TIáº¾T CÃC Lá»–I ÄÃƒ PHÃT HIá»†N

---

### ðŸ› BUG #1: BUG-FR04-01 (High - Validation Logic Sai Lá»‡ch)

* **TiÃªu Ä‘á» Issue:** `[FR-04][BUG] Há»‡ thá»‘ng tá»« chá»‘i sá»‘ Ä‘iá»‡n thoáº¡i 10 vÃ  11 chá»¯ sá»‘ há»£p lá»‡ vÃ  thÃ´ng bÃ¡o sai lá»‡ch 9-10 chá»¯ sá»‘`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-04`, `validation`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High
* **MÃ´i trÆ°á»ng:** Chrome / Windows 11 / EShop Local (Frontend: React, Backend: Node.js)

#### 1. MÃ´ táº£ lá»—i
Theo Ä‘áº·c táº£ nghiá»‡p vá»¥ FR-04: *"Sá»‘ Ä‘iá»‡n thoáº¡i há»£p lá»‡: báº¯t Ä‘áº§u báº±ng sá»‘ 0, tá»« 10â€“11 chá»¯ sá»‘"*.
Tuy nhiÃªn, khi ngÆ°á»i dÃ¹ng nháº­p sá»‘ Ä‘iá»‡n thoáº¡i 10 chá»¯ sá»‘ (`0912345678`) hoáº·c 11 chá»¯ sá»‘ (`01234567890`), há»‡ thá»‘ng tá»« chá»‘i lÆ°u vÃ  Ä‘Æ°a ra thÃ´ng bÃ¡o lá»—i mÃ¢u thuáº«n: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p 9-10 chá»¯ sá»‘"*. Äiá»u nÃ y khiáº¿n ngÆ°á»i dÃ¹ng há»£p lá»‡ khÃ´ng thá»ƒ cáº­p nháº­t Ä‘Æ°á»£c sá»‘ Ä‘iá»‡n thoáº¡i chuáº©n cá»§a Viá»‡t Nam.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. ÄÄƒng nháº­p vÃ o há»‡ thá»‘ng vá»›i tÃ i khoáº£n `test@eshop.com` / `Test1234!`.
2. Äiá»u hÆ°á»›ng tá»›i trang **Há»“ sÆ¡ cÃ¡ nhÃ¢n** (`http://localhost:5173/profile`).
3. Nháº­p Há» TÃªn: `"Nguyá»…n VÄƒn A"`, Äá»‹a chá»‰: `"123 LÃª Lá»£i, Q1"`.
4. Nháº­p Sá»‘ Ä‘iá»‡n thoáº¡i 10 sá»‘ há»£p lá»‡: `"0912345678"`.
5. Nháº¥n nÃºt **Cáº­p nháº­t**.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng cháº¥p nháº­n SÄT 10 sá»‘ (vÃ  11 sá»‘), cáº­p nháº­t thÃ nh cÃ´ng vÃ o CSDL SQLite vÃ  hiá»ƒn thá»‹ thÃ´ng bÃ¡o thÃ nh cÃ´ng.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Há»‡ thá»‘ng bÃ¡o lá»—i: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p 9-10 chá»¯ sá»‘"*. KhÃ´ng cho phÃ©p lÆ°u.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool A/EXE-02_Valid_10Digits_Phone.png](../screenshots/pool%20A/EXE-02_Valid_10Digits_Phone.png)
* Minh chá»©ng lá»—i tá»« chá»‘i SÄT 11 sá»‘: [screenshots/pool A/EXE-03_Valid_11Digits_Phone.png](../screenshots/pool%20A/EXE-03_Valid_11Digits_Phone.png)

---

### ðŸ› BUG #2: BUG-FR04-02 (Medium - Data Integrity)

* **TiÃªu Ä‘á» Issue:** `[FR-04][BUG] Há»‡ thá»‘ng cho phÃ©p cáº­p nháº­t vÃ  lÆ°u Äá»‹a chá»‰ giao hÃ ng rá»—ng vÃ o CSDL`
* **NhÃ£n (Labels):** `bug`, `medium`, `FR-04`, `data-integrity`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Medium

#### 1. MÃ´ táº£ lá»—i
TrÆ°á»ng "Äá»‹a chá»‰ giao hÃ ng máº·c Ä‘á»‹nh" lÃ  thÃ´ng tin thiáº¿t yáº¿u cÃ³ dáº¥u `*` báº¯t buá»™c theo FR-22 Ä‘á»ƒ phá»¥c vá»¥ chá»©c nÄƒng Thanh toÃ¡n & Giao hÃ ng (FR-08, FR-18). Tuy nhiÃªn, form cáº­p nháº­t há»“ sÆ¡ vÃ  Backend API cho phÃ©p ngÆ°á»i dÃ¹ng xÃ³a tráº¯ng Ä‘á»‹a chá»‰ vÃ  báº¥m lÆ°u thÃ nh cÃ´ng, ghi Ä‘Ã¨ chuá»—i rá»—ng `""` vÃ o CSDL SQLite.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Truy cáº­p vÃ o trang Há»“ sÆ¡ cÃ¡ nhÃ¢n (`http://localhost:5173/profile`).
2. Nháº­p Há» TÃªn: `"Nguyá»…n VÄƒn A"`, SÄT há»£p lá»‡.
3. XÃ³a sáº¡ch Ã´ **Äá»‹a chá»‰ giao hÃ ng máº·c Ä‘á»‹nh** (Ä‘á»ƒ rá»—ng 0 kÃ½ tá»±).
4. Nháº¥n nÃºt **Cáº­p nháº­t**.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng pháº£i cháº·n láº¡i, hiá»ƒn thá»‹ thÃ´ng bÃ¡o lá»—i yÃªu cáº§u nháº­p Ä‘á»‹a chá»‰ á»Ÿ phÃ­a trÃªn nÃºt báº¥m (theo FR-22).

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Há»‡ thá»‘ng hiá»ƒn thá»‹ thÃ´ng bÃ¡o "Cáº­p nháº­t thÃ nh cÃ´ng" vÃ  CSDL lÆ°u Ä‘á»‹a chá»‰ rá»—ng.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool A/EXE-07_Error_Empty_Address.png](../screenshots/pool%20A/EXE-07_Error_Empty_Address.png)

---

### ðŸ› BUG #3: BUG-FR04-03 (Medium - Boundary Validation)

* **TiÃªu Ä‘á» Issue:** `[FR-04][BUG] KhÃ´ng giá»›i háº¡n Ä‘á»™ dÃ i tá»‘i Ä‘a 255 kÃ½ tá»± cho trÆ°á»ng Há» TÃªn vÃ  Äá»‹a chá»‰`
* **NhÃ£n (Labels):** `bug`, `medium`, `FR-04`, `boundary`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Low/Medium

#### 1. MÃ´ táº£ lá»—i
Táº¡i ranh giá»›i kiá»ƒm thá»­ Max + 1 (256 kÃ½ tá»±), cáº£ giao diá»‡n Frontend vÃ  API Backend Ä‘á»u khÃ´ng cÃ³ cÆ¡ cháº¿ cháº·n giá»›i háº¡n Ä‘á»™ dÃ i (thiáº¿u thuá»™c tÃ­nh `maxlength="255"` trÃªn tháº» `<input>`), cho phÃ©p lÆ°u chuá»—i dÃ i tÃ¹y Ã½ vÃ o CSDL SQLite.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Má»Ÿ Console trong F12 gÃµ: `copy('A'.repeat(256))` Ä‘á»ƒ láº¥y chuá»—i 256 kÃ½ tá»±.
2. DÃ¡n chuá»—i nÃ y vÃ o Ã´ **Há» TÃªn** vÃ  **Äá»‹a chá»‰**.
3. Nháº¥n nÃºt **Cáº­p nháº­t**.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng cháº·n khÃ´ng cho nháº­p kÃ½ tá»± thá»© 256 hoáº·c Backend tráº£ vá» mÃ£ lá»—i HTTP 400 Bad Request tá»« chá»‘i chuá»—i quÃ¡ dÃ i.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Há»‡ thá»‘ng cháº¥p nháº­n vÃ  lÆ°u chuá»—i 256 kÃ½ tá»± thÃ nh cÃ´ng vÃ o SQLite.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool A/EXE-08_BVA_MaxPlus1_256Chars.png](../screenshots/pool%20A/EXE-08_BVA_MaxPlus1_256Chars.png)

---

### ðŸ› BUG #4: BUG-FR04-04 (High - Boundary Validation)

* **TiÃªu Ä‘á» Issue:** `[FR-04][BUG] Há»‡ thá»‘ng cháº¥p nháº­n sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng báº¯t Ä‘áº§u báº±ng sá»‘ 0`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-04`, `boundary`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ FR-04 quy Ä‘á»‹nh rÃµ rÃ ng: *"Sá»‘ Ä‘iá»‡n thoáº¡i há»£p lá»‡: báº¯t Ä‘áº§u báº±ng sá»‘ 0"*. Tuy nhiÃªn, há»‡ thá»‘ng cho phÃ©p ngÆ°á»i dÃ¹ng nháº­p sá»‘ Ä‘iá»‡n thoáº¡i báº¯t Ä‘áº§u báº±ng chá»¯ sá»‘ khÃ¡c 0 (vÃ­ dá»¥: `1912345678`) vÃ  lÆ°u thÃ nh cÃ´ng vÃ o CSDL.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Táº¡i trang Há»“ sÆ¡ cÃ¡ nhÃ¢n, nháº­p Há» TÃªn vÃ  Äá»‹a chá»‰ há»£p lá»‡.
2. Nháº­p Sá»‘ Ä‘iá»‡n thoáº¡i: `"1912345678"` (báº¯t Ä‘áº§u báº±ng sá»‘ 1 thay vÃ¬ sá»‘ 0).
3. Nháº¥n nÃºt **Cáº­p nháº­t**.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»« chá»‘i cáº­p nháº­t, hiá»ƒn thá»‹ lá»—i yÃªu cáº§u sá»‘ Ä‘iá»‡n thoáº¡i pháº£i báº¯t Ä‘áº§u báº±ng sá»‘ 0.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Há»‡ thá»‘ng bÃ¡o cáº­p nháº­t thÃ nh cÃ´ng vÃ  lÆ°u sá»‘ Ä‘iá»‡n thoáº¡i sai Ä‘á»‹nh dáº¡ng vÃ o CSDL.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool A/EXE-12_BVA_Phone_NotStartWithZero.png](../screenshots/pool%20A/EXE-12_BVA_Phone_NotStartWithZero.png)

---

### ðŸš¨ BUG #5: BUG-FR04-05 (CRITICAL - SECURITY / PRIVILEGE ESCALATION)

* **TiÃªu Ä‘á» Issue:** `[FR-04][SEC-06][CRITICAL] Lá»— há»•ng Leo thang Ä‘áº·c quyá»n (Privilege Escalation) cho phÃ©p User tá»± Ä‘á»•i role="admin"`
* **NhÃ£n (Labels):** `bug`, `critical`, `security`, `FR-04`, `SEC-06`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Critical (NghiÃªm trá»ng nháº¥t)
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Urgent / Immediate

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ FR-04 vÃ  SEC-06 nghiÃªm cáº¥m: *"NgÆ°á»i dÃ¹ng chá»‰ cÃ³ thá»ƒ cáº­p nháº­t há»“ sÆ¡ cá»§a chÃ­nh mÃ¬nh; khÃ´ng thá»ƒ tá»± thay Ä‘á»•i thuá»™c tÃ­nh role"*.
Tuy nhiÃªn, trong mÃ£ nguá»“n Backend táº¡i `backend/server.js` (dÃ²ng 124-127):
```javascript
if (role) {
  query += ", role = ?";
  params.push(role);
}
```
API `PUT /api/users/me` Ä‘Ã£ nháº­n trá»±c tiáº¿p tham sá»‘ `role` tá»« body request vÃ  cáº­p nháº­t tháº³ng vÃ o CSDL SQLite mÃ  khÃ´ng há» kiá»ƒm tra quyá»n háº¡n. Äiá»u nÃ y táº¡o Ä‘iá»u kiá»‡n cho báº¥t ká»³ ngÆ°á»i dÃ¹ng thÃ´ng thÆ°á»ng nÃ o cÅ©ng cÃ³ thá»ƒ tá»± cáº¥p quyá»n Quáº£n trá»‹ viÃªn (`admin`) cho chÃ­nh mÃ¬nh vÃ  chiáº¿m quyá»n Ä‘iá»u khiá»ƒn toÃ n bá»™ há»‡ thá»‘ng Web Admin.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. ÄÄƒng nháº­p báº±ng tÃ i khoáº£n ngÆ°á»i dÃ¹ng thÆ°á»ng `test@eshop.com`.
2. Gá»­i request HTTP PUT tá»›i `http://localhost:3000/api/users/me` vá»›i Header `Authorization: Bearer <token_user>` vÃ  body JSON:
```json
{
  "name": "Nguyá»…n VÄƒn A",
  "phone": "0912345678",
  "shipping_address": "123 LÃª Lá»£i",
  "role": "admin"
}
```
3. Kiá»ƒm tra CSDL SQLite: Cá»™t `role` cá»§a user chuyá»ƒn thÃ nh `'admin'`.
4. Má»Ÿ trÃ¬nh duyá»‡t truy cáº­p Web Admin `http://localhost:5174/` báº±ng tÃ i khoáº£n `test@eshop.com` $\rightarrow$ ÄÄƒng nháº­p thÃ nh cÃ´ng vÃ  chiáº¿m toÃ n quyá»n quáº£n trá»‹!

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Backend pháº£i tá»« chá»‘i request cÃ³ chá»©a thuá»™c tÃ­nh `role` (HTTP 400 Bad Request hoáº·c 403 Forbidden) hoáº·c phá»›t lá» trÆ°á»ng `role`, giá»¯ nguyÃªn quyá»n háº¡n ban Ä‘áº§u cá»§a ngÆ°á»i dÃ¹ng.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Backend cáº­p nháº­t thÃ nh cÃ´ng vÃ  trao quyá»n Admin cho tÃ i khoáº£n ngÆ°á»i dÃ¹ng thÆ°á»ng.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool A/EXE-15_Bug_Privilege_Escalation.png](../screenshots/pool%20A/EXE-15_Bug_Privilege_Escalation.png)

