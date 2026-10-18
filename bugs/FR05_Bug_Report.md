# BÃO CÃO CHI TIáº¾T DANH SÃCH Lá»–I (BUG REPORT)
## TÃNH NÄ‚NG FR-05: XEM DANH SÃCH & TÃŒM KIáº¾M Sáº¢N PHáº¨M (MOBILE - POOL D)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform â€” PhÃ¢n há»‡ Mobile Expo React Native (`frontend-mobile` & Backend API `http://localhost:3000`)
* **Tá»•ng sá»‘ lá»—i phÃ¡t hiá»‡n:** 4 nhÃ³m lá»—i (1 Critical, 1 High, 1 Medium, 1 Low/Medium)
* **NgÃ y láº­p bÃ¡o cÃ¡o:** 2026-10-18

---

## DANH SÃCH CHI TIáº¾T CÃC Lá»–I ÄÃƒ PHÃT HIá»†N

---

### ðŸš¨ BUG #1: BUG-FR05-03 (CRITICAL - SECURITY / SQL INJECTION & HTML LEAK)

* **TiÃªu Ä‘á» Issue:** `[FR-05][SEC-01][CRITICAL] Lá»— há»•ng SQL Injection táº¡i Ã´ tÃ¬m kiáº¿m sáº£n pháº©m vÃ  rÃ² rá»‰ mÃ£ HTML lá»—i 500 trÃªn Mobile`
* **NhÃ£n (Labels):** `bug`, `critical`, `security`, `FR-05`, `sql-injection`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Critical (Lá»— há»•ng báº£o máº­t tá»‘i nghiÃªm trá»ng - Tiáº¿t lá»™ dá»¯ liá»‡u & Sáº­p CSDL)
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Urgent / Immediate
* **MÃ´i trÆ°á»ng:** Expo React Native / Android Emulator (Pixel 6) & Web Mobile / Backend Node.js Express / CSDL SQLite

#### 1. MÃ´ táº£ lá»—i
Theo Ä‘áº·c táº£ SEC-01: *"Há»‡ thá»‘ng pháº£i Ã¡p dá»¥ng Parameterized Query cho táº¥t cáº£ cÃ¡c cÃ¢u truy váº¥n cÆ¡ sá»Ÿ dá»¯ liá»‡u Ä‘á»ƒ ngÄƒn cháº·n triá»‡t Ä‘á»ƒ táº¥n cÃ´ng SQL Injection"*.
Tuy nhiÃªn, táº¡i `backend/server.js` (dÃ²ng 141-151), API `GET /api/products` ná»‘i chuá»—i trá»±c tiáº¿p giÃ¡ trá»‹ `req.query.search` vÃ o cÃ¢u truy váº¥n SQL:
```javascript
const query = `SELECT * FROM products WHERE name LIKE '%${searchQuery}%'`;
db.all(query, [], (err, rows) => { ... });
```

Háº­u quáº£:
1. **Bypass bá»™ lá»c dá»¯ liá»‡u (Data Leak):** Káº» táº¥n cÃ´ng nháº­p `' OR 1=1 --` vÃ o thanh tÃ¬m kiáº¿m, má»‡nh Ä‘á» `1=1` luÃ´n Ä‘Ãºng khiáº¿n CSDL tráº£ vá» toÃ n bá»™ 5 sáº£n pháº©m cá»§a há»‡ thá»‘ng thay vÃ¬ 0 sáº£n pháº©m.
2. **Sáº­p cÃ¢u truy váº¥n & Lá»™ lá»—i HTML thÃ´:** Khi ngÆ°á»i dÃ¹ng nháº­p dáº¥u nhÃ¡y Ä‘Æ¡n `'`, Backend vÄƒng lá»—i `SQLITE_ERROR: unrecognized token: "'"` vÃ  tráº£ vá» mÃ£ HTTP 500 kÃ¨m chuá»—i HTML:
   `<h1>Database Error</h1><p>SQLITE_ERROR: unrecognized token: "'"</p>`
   á»¨ng dá»¥ng Mobile táº¡i `App.js` dÃ²ng 97-104 khÃ´ng lá»c mÃ  render nguyÃªn khá»‘i mÃ£ HTML lá»—i thÃ´ nÃ y lÃªn mÃ n hÃ¬nh giao diá»‡n ngÆ°á»i dÃ¹ng.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Má»Ÿ á»©ng dá»¥ng EShop Mobile trÃªn mÃ¡y áº£o Pixel 6 hoáº·c Expo Web.
2. Táº¡i Ã´ tÃ¬m kiáº¿m, nháº­p chuá»—i payload SQL Injection: `' OR 1=1 --`. Quan sÃ¡t tháº¥y há»‡ thá»‘ng tráº£ vá» toÃ n bá»™ 5 sáº£n pháº©m.
3. Tiáº¿p tá»¥c nháº­p dáº¥u nhÃ¡y Ä‘Æ¡n `'` vÃ o Ã´ tÃ¬m kiáº¿m. Quan sÃ¡t tháº¥y mÃ n hÃ¬nh hiá»ƒn thá»‹ chuá»—i HTML thÃ´ `<h1>Database Error</h1>`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
- Backend pháº£i sá»­ dá»¥ng Parameterized Query: `WHERE name LIKE ?` vá»›i tham sá»‘ `[%${searchQuery}%]`.
- Khi nháº­p dáº¥u nhÃ¡y Ä‘Æ¡n hoáº·c kÃ½ tá»± Ä‘áº·c biá»‡t, há»‡ thá»‘ng pháº£i xá»­ lÃ½ nhÆ° chuá»—i tÃ¬m kiáº¿m thÃ´ng thÆ°á»ng (tráº£ vá» 0 káº¿t quáº£), khÃ´ng Ä‘Æ°á»£c lÃ m sáº­p cÃ¢u lá»‡nh SQL vÃ  khÃ´ng render mÃ£ HTML lá»—i ra ngoÃ i giao diá»‡n Mobile.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
ToÃ n bá»™ danh sÃ¡ch sáº£n pháº©m bá»‹ rÃ² rá»‰ khi nháº­p payload SQLi; giao diá»‡n Mobile vÄƒng nguyÃªn khá»‘i HTML lá»—i 500.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng SQL Injection:** [screenshots/pool D/EXE-08_Bug_Critical_SQL_Injection.png](../screenshots/pool%20D/EXE-08_Bug_Critical_SQL_Injection.png)
* Minh chá»©ng rÃ² rá»‰ mÃ£ HTML lá»—i 500: [screenshots/pool D/EXE-09_Bug_SQL_Error_HTML_Leak.png](../screenshots/pool%20D/EXE-09_Bug_SQL_Error_HTML_Leak.png)

---

### ðŸ› BUG #2: BUG-FR05-01 (High - Missing Empty State on Mobile)

* **TiÃªu Ä‘á» Issue:** `[FR-05][HIGH] Giao diá»‡n Mobile tráº¯ng trÆ¡n, thiáº¿u háº³n giao diá»‡n Empty State khi khÃ´ng tÃ¬m tháº¥y sáº£n pháº©m`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-05`, `ui-ux`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ nghiá»‡p vá»¥ FR-05 quy Ä‘á»‹nh rÃµ rÃ ng:
> *"Khi khÃ´ng cÃ³ káº¿t quáº£ tÃ¬m kiáº¿m pháº£i hiá»ƒn thá»‹ thÃ´ng bÃ¡o empty state phÃ¹ há»£p (kÃ¨m hÃ¬nh minh há»a vÃ  thÃ´ng bÃ¡o rÃµ rÃ ng)."*

Tuy nhiÃªn, trong mÃ£ nguá»“n `frontend-mobile/App.js`, danh sÃ¡ch sáº£n pháº©m hiá»ƒn thá»‹ báº±ng tháº» `<FlatList>` hoÃ n toÃ n thiáº¿u thuá»™c tÃ­nh `ListEmptyComponent`. Khi tÃ¬m kiáº¿m vá»›i tá»« khÃ³a khÃ´ng tá»“n táº¡i trong CSDL (vÃ­ dá»¥: `"XYZ999"` hoáº·c chuá»—i vÆ°á»£t biÃªn $255$ kÃ½ tá»±), vÃ¹ng hiá»ƒn thá»‹ danh sÃ¡ch phÃ­a dÆ°á»›i thanh tÃ¬m kiáº¿m hoÃ n toÃ n tráº¯ng tinh, khÃ´ng há» cÃ³ icon minh há»a hay thÃ´ng bÃ¡o nÃ o cho ngÆ°á»i dÃ¹ng.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Khá»Ÿi cháº¡y á»©ng dá»¥ng Mobile EShop.
2. Táº¡i thanh tÃ¬m kiáº¿m, gÃµ tá»« khÃ³a: `"XYZ999"` (khÃ´ng cÃ³ sáº£n pháº©m nÃ o khá»›p).
3. Quan sÃ¡t mÃ n hÃ¬nh bÃªn dÆ°á»›i thanh tÃ¬m kiáº¿m.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Hiá»ƒn thá»‹ giao diá»‡n Empty State thÃ¢n thiá»‡n kÃ¨m hÃ¬nh áº£nh minh há»a vÃ  thÃ´ng Ä‘iá»‡p: *"KhÃ´ng tÃ¬m tháº¥y sáº£n pháº©m phÃ¹ há»£p vá»›i tá»« khÃ³a cá»§a báº¡n"*.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
MÃ n hÃ¬nh hoÃ n toÃ n tráº¯ng trÆ¡n, khÃ´ng cÃ³ báº¥t ká»³ pháº£n há»“i trá»±c quan nÃ o.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng thiáº¿u Empty State:** [screenshots/pool D/EXE-06_Bug_Missing_Empty_State.png](../screenshots/pool%20D/EXE-06_Bug_Missing_Empty_State.png)
* Minh chá»©ng táº¡i biÃªn Max 255 kÃ½ tá»±: [screenshots/pool D/EXE-13_BVA_Max_255_Chars.png](../screenshots/pool%20D/EXE-13_BVA_Max_255_Chars.png)
* Minh chá»©ng táº¡i biÃªn Max+1 256 kÃ½ tá»±: [screenshots/pool D/EXE-14_BVA_MaxPlus1_256_Chars.png](../screenshots/pool%20D/EXE-14_BVA_MaxPlus1_256_Chars.png)

---

### ðŸ› BUG #3: BUG-FR05-02 (Medium - Data Handling / Untrimmed Whitespace)

* **TiÃªu Ä‘á» Issue:** `[FR-05][MEDIUM] Backend khÃ´ng cáº¯t bá» khoáº£ng tráº¯ng thá»«a Ä‘áº§u cuá»‘i khiáº¿n tÃ¬m kiáº¿m tháº¥t báº¡i dÃ¹ sáº£n pháº©m cÃ³ tá»“n táº¡i`
* **NhÃ£n (Labels):** `bug`, `medium`, `FR-05`, `data-handling`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Medium

#### 1. MÃ´ táº£ lá»—i
NgÆ°á»i dÃ¹ng thiáº¿t bá»‹ di Ä‘á»™ng thÆ°á»ng vÃ´ tÃ¬nh gÃµ kÃ¨m khoáº£ng tráº¯ng á»Ÿ Ä‘áº§u hoáº·c cuá»‘i tá»« khÃ³a khi nháº­p liá»‡u bÃ n phÃ­m áº£o hoáº·c copy/paste.
Táº¡i `backend/server.js` (dÃ²ng 142), API tÃ¬m kiáº¿m láº¥y trá»±c tiáº¿p giÃ¡ trá»‹ `req.query.search` mÃ  khÃ´ng gá»i phÆ°Æ¡ng thá»©c `.trim()`. Khi ngÆ°á»i dÃ¹ng nháº­p `"  iPhone  "`, há»‡ thá»‘ng tÃ¬m kiáº¿m chuá»—i cÃ³ khoáº£ng tráº¯ng vÃ  khÃ´ng tÃ¬m tháº¥y báº¥t ká»³ sáº£n pháº©m nÃ o, máº·c dÃ¹ sáº£n pháº©m "iPhone 15 Pro Max" Ä‘ang cÃ³ sáºµn trong CSDL.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. TrÃªn giao diá»‡n Mobile, nháº­p vÃ o Ã´ tÃ¬m kiáº¿m tá»« khÃ³a cÃ³ khoáº£ng tráº¯ng Ä‘áº§u cuá»‘i: `"  iPhone  "`.
2. Quan sÃ¡t káº¿t quáº£ tÃ¬m kiáº¿m.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»± Ä‘á»™ng loáº¡i bá» khoáº£ng tráº¯ng thá»«a vÃ  tÃ¬m tháº¥y sáº£n pháº©m "iPhone 15 Pro Max".

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Há»‡ thá»‘ng khÃ´ng tÃ¬m tháº¥y báº¥t ká»³ sáº£n pháº©m nÃ o (0 káº¿t quáº£).

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool D/EXE-05_Bug_Untrimmed_Whitespace_Fail.png](../screenshots/pool%20D/EXE-05_Bug_Untrimmed_Whitespace_Fail.png)

---

### ðŸ› BUG #4: BUG-FR05-04 (Low/Medium - GUI Standards & Accessibility)

* **TiÃªu Ä‘á» Issue:** `[FR-05][LOW/MEDIUM] áº¢nh sáº£n pháº©m bá»‹ mÃ©o tá»· lá»‡ do resizeMode="stretch" vÃ  thiáº¿u thuá»™c tÃ­nh mÃ´ táº£ áº£nh (alt text)`
* **NhÃ£n (Labels):** `bug`, `low`, `FR-05`, `gui-standards`, `accessibility`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Low / Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Low

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ nghiá»‡p vá»¥ FR-05 quy Ä‘á»‹nh: *"Má»—i sáº£n pháº©m hiá»ƒn thá»‹: áº¢nh (tá»· lá»‡ chuáº©n, cÃ³ alt text mÃ´ táº£), TÃªn sáº£n pháº©m, GiÃ¡..."*.
Tuy nhiÃªn trong mÃ£ nguá»“n `frontend-mobile/App.js` dÃ²ng 460:
1. Tháº» `<Image>` Ä‘Æ°á»£c cáº¥u hÃ¬nh thuá»™c tÃ­nh `resizeMode="stretch"`, lÃ m áº£nh sáº£n pháº©m bá»‹ co giÃ£n mÃ©o mÃ³, biáº¿n dáº¡ng tá»· lá»‡ khung hÃ¬nh chuáº©n trÃªn thiáº¿t bá»‹ di Ä‘á»™ng.
2. Tháº» `<Image>` thiáº¿u hoÃ n toÃ n thuá»™c tÃ­nh mÃ´ táº£ há»— trá»£ tiáº¿p cáº­n `accessibilityLabel` (tÆ°Æ¡ng Ä‘Æ°Æ¡ng vá»›i thuá»™c tÃ­nh `alt` trong HTML).

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Má»Ÿ á»©ng dá»¥ng Mobile EShop á»Ÿ mÃ n hÃ¬nh chÃ­nh.
2. Quan sÃ¡t hÃ¬nh áº£nh hiá»ƒn thá»‹ cá»§a cÃ¡c sáº£n pháº©m dáº¡ng Card/Grid.
3. Kiá»ƒm tra mÃ£ nguá»“n tháº» Image táº¡i `frontend-mobile/App.js`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
áº¢nh sáº£n pháº©m pháº£i hiá»ƒn thá»‹ theo tá»· lá»‡ chuáº©n khÃ´ng bá»‹ mÃ©o (`resizeMode="cover"` hoáº·c `"contain"`), Ä‘á»“ng thá»i cÃ³ thuá»™c tÃ­nh mÃ´ táº£ `accessibilityLabel={item.name}`.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
áº¢nh bá»‹ kÃ©o giÃ£n mÃ©o tá»· lá»‡ vÃ  thiáº¿u thuá»™c tÃ­nh mÃ´ táº£ tiáº¿p cáº­n.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool D/EXE-01_GUI_Default_Product_List.png](../screenshots/pool%20D/EXE-01_GUI_Default_Product_List.png)

