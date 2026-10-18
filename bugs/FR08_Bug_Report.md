# BÃO CÃO CHI TIáº¾T DANH SÃCH Lá»–I (BUG REPORT)
## TÃNH NÄ‚NG FR-08: THANH TOÃN (CHECKOUT) (POOL B)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Tá»•ng sá»‘ lá»—i phÃ¡t hiá»‡n:** 7 lá»—i (1 Critical, 4 High, 2 Medium)
* **NgÃ y láº­p bÃ¡o cÃ¡o:** 2026-10-18

---

## DANH SÃCH CHI TIáº¾T CÃC Lá»–I ÄÃƒ PHÃT HIá»†N

---

### ðŸš¨ BUG #1: BUG-FR08-01 (CRITICAL - SECURITY / PRICE TAMPERING)

* **TiÃªu Ä‘á» Issue:** `[FR-08][SEC-02][CRITICAL] Lá»— há»•ng Gian láº­n giÃ¡ (Price Tampering) cho phÃ©p sá»­a tá»•ng tiá»n thanh toÃ¡n qua UI vÃ  API`
* **NhÃ£n (Labels):** `bug`, `critical`, `security`, `FR-08`, `business-logic`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Critical (NghiÃªm trá»ng nháº¥t - Tháº¥t thoÃ¡t tÃ i chÃ­nh)
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Urgent / Immediate
* **MÃ´i trÆ°á»ng:** Chrome / Windows / EShop Local (Frontend: React, Backend: Node.js, CSDL: SQLite)

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ FR-08 quy Ä‘á»‹nh rÃµ rÃ ng:
> *"Tá»•ng tiá»n thanh toÃ¡n Ä‘Æ°á»£c tÃ­nh tá»± Ä‘á»™ng tá»« giá» hÃ ng vÃ  khÃ´ng cho phÃ©p ngÆ°á»i dÃ¹ng chá»‰nh sá»­a trá»±c tiáº¿p."*
> *"Backend pháº£i tá»± tÃ­nh láº¡i tá»•ng tiá»n; khÃ´ng cháº¥p nháº­n giÃ¡ trá»‹ total_amount do client gá»­i lÃªn."*

Tuy nhiÃªn trÃªn thá»±c táº¿:
1. **Táº¡i Frontend (`frontend-web/src/pages/Checkout.jsx` dÃ²ng 93-102):** Ã” Tá»•ng tiá»n thanh toÃ¡n lÃ  má»™t tháº» `<input type="number">` cho phÃ©p ngÆ°á»i dÃ¹ng tá»± do click vÃ o chá»‰nh sá»­a sá»‘ tiá»n tÃ¹y Ã½.
2. **Táº¡i Backend (`backend/server.js` dÃ²ng 299-304):** API `POST /api/checkout` nháº­n trá»±c tiáº¿p tham sá»‘ `total_amount` tá»« request body cá»§a client vÃ  lÆ°u tháº³ng vÃ o báº£ng `orders` trong CSDL SQLite mÃ  khÃ´ng há» tá»± tÃ­nh láº¡i tá»•ng tiá»n tá»« giá» hÃ ng.

Háº­u quáº£: Káº» gian cÃ³ thá»ƒ mua Ä‘Æ¡n hÃ ng 2 sáº£n pháº©m cao cáº¥p trá»‹ giÃ¡ **36.000.000 â‚«** nhÆ°ng chá»‰ cáº§n thanh toÃ¡n vá»›i sá»‘ tiá»n **1.000 â‚«** (hoáº·c 0 â‚«) vÃ  há»‡ thá»‘ng váº«n cháº¥p nháº­n táº¡o Ä‘Æ¡n hÃ ng thÃ nh cÃ´ng!

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. ÄÄƒng nháº­p vÃ o há»‡ thá»‘ng vá»›i tÃ i khoáº£n `test@eshop.com` / `Test1234!`.
2. ThÃªm vÃ o giá» hÃ ng: **iPhone 15 Pro Max** (30.000.000 â‚«) vÃ  **Tai nghe AirPods Pro 2** (6.000.000 â‚«). Tá»•ng tiá»n thá»±c táº¿: 36.000.000 â‚«.
3. Äiá»u hÆ°á»›ng tá»›i trang Checkout (`http://localhost:5173/checkout`).
4. Táº¡i Ã´ "Tá»•ng tiá»n thanh toÃ¡n (VND)", xÃ³a sá»‘ tiá»n 36.000.000 â‚« Ä‘i vÃ  nháº­p `1000`.
5. Nháº¥n nÃºt **"XÃ¡c Nháº­n Thanh ToÃ¡n"**.
6. Kiá»ƒm tra CSDL SQLite báº£ng `orders`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
- Giao diá»‡n pháº£i khÃ³a trÆ°á»ng Tá»•ng tiá»n (Read-only / Text).
- Backend pháº£i tá»± tÃ­nh láº¡i tá»•ng tiá»n tá»« báº£ng `products` trong CSDL ($36,000,000\text{ â‚«}$) vÃ  tá»« chá»‘i giÃ¡ trá»‹ 1.000 â‚« do client gá»­i lÃªn.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
ÄÆ¡n hÃ ng Ä‘Æ°á»£c lÆ°u thÃ nh cÃ´ng vÃ o CSDL SQLite vá»›i `total_amount = 1000 â‚«`.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-12_Bug_Critical_Price_Tampering.png](../screenshots/pool%20B/EXE-12_Bug_Critical_Price_Tampering.png)
* Minh chá»©ng Ã´ input cho sá»­a giÃ¡: [screenshots/pool B/EXE-01_GUI_TotalAmount_And_H1_Check.png](../screenshots/pool%20B/EXE-01_GUI_TotalAmount_And_H1_Check.png)

---

### ðŸ› BUG #2: BUG-FR08-02 (High - State Transition / Data Integrity)

* **TiÃªu Ä‘á» Issue:** `[FR-08][HIGH] Giá» hÃ ng khÃ´ng Ä‘Æ°á»£c lÃ m trá»‘ng sau khi ngÆ°á»i dÃ¹ng hoÃ n táº¥t thanh toÃ¡n thÃ nh cÃ´ng`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-08`, `state-transition`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Theo Ä‘áº·c táº£ nghiá»‡p vá»¥ FR-08: *"Sau thanh toÃ¡n thÃ nh cÃ´ng, giá» hÃ ng Ä‘Æ°á»£c xÃ³a"*.
Tuy nhiÃªn, sau khi ngÆ°á»i dÃ¹ng hoÃ n táº¥t Ä‘áº·t hÃ ng thÃ nh cÃ´ng, giá» hÃ ng cá»§a ngÆ°á»i dÃ¹ng váº«n giá»¯ nguyÃªn toÃ n bá»™ cÃ¡c sáº£n pháº©m vá»«a mua. Khi ngÆ°á»i dÃ¹ng quay láº¡i trang giá» hÃ ng hoáº·c tiáº¿p tá»¥c mua sáº¯m, cÃ¡c sáº£n pháº©m nÃ y váº«n cÃ²n trong giá», dáº«n tá»›i nguy cÆ¡ Ä‘áº·t hÃ ng trÃ¹ng láº·p ngoÃ i Ã½ muá»‘n.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. ThÃªm 2 sáº£n pháº©m vÃ o giá» hÃ ng (`/cart`).
2. Tiáº¿n hÃ nh Checkout táº¡i `/checkout` vÃ  nháº¥n **XÃ¡c Nháº­n Thanh ToÃ¡n**.
3. Há»‡ thá»‘ng hiá»ƒn thá»‹ thÃ´ng bÃ¡o "Äáº·t hÃ ng thÃ nh cÃ´ng!".
4. Quay láº¡i trang Giá» hÃ ng (`http://localhost:5173/cart`).

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Giá» hÃ ng pháº£i Ä‘Æ°á»£c lÃ m trá»‘ng (`items = []`), hiá»ƒn thá»‹ tráº¡ng thÃ¡i "Giá» hÃ ng cá»§a báº¡n Ä‘ang trá»‘ng".

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Giá» hÃ ng váº«n giá»¯ nguyÃªn 2 sáº£n pháº©m ban Ä‘áº§u kÃ¨m tá»•ng tiá»n 36.000.000 â‚«.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-05_Bug_Cart_Not_Cleared_After_Checkout.png](../screenshots/pool%20B/EXE-05_Bug_Cart_Not_Cleared_After_Checkout.png)

---

### ðŸ› BUG #3: BUG-FR08-03 (High - Missing Mandatory Field & Validation)

* **TiÃªu Ä‘á» Issue:** `[FR-08][FR-22][HIGH] Giao diá»‡n Checkout thiáº¿u hoÃ n toÃ n Ã´ nháº­p Äá»‹a chá»‰ giao hÃ ng vÃ  Backend cho phÃ©p lÆ°u Ä‘Æ¡n hÃ ng khÃ´ng Ä‘á»‹a chá»‰`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-08`, `FR-22`, `validation`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Äá»‹a chá»‰ giao hÃ ng lÃ  trÆ°á»ng thÃ´ng tin báº¯t buá»™c Ä‘á»‘i vá»›i Ä‘Æ¡n hÃ ng thÆ°Æ¡ng máº¡i Ä‘iá»‡n tá»­ (cÃ³ dáº¥u `*` Ä‘á» theo FR-22).
Tuy nhiÃªn, trang Checkout khÃ´ng há» cÃ³ Ã´ nháº­p trÆ°á»ng "Äá»‹a chá»‰ giao hÃ ng", cÅ©ng khÃ´ng káº¿ thá»«a Ä‘á»‹a chá»‰ máº·c Ä‘á»‹nh tá»« há»“ sÆ¡ cÃ¡ nhÃ¢n. Äá»“ng thá»i API `POST /api/checkout` cho phÃ©p lÆ°u Ä‘Æ¡n hÃ ng cÃ³ Ä‘á»‹a chá»‰ rá»—ng vÃ o báº£ng `orders`.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Truy cáº­p trang Checkout `http://localhost:5173/checkout`.
2. Quan sÃ¡t toÃ n bá»™ form Ä‘áº·t hÃ ng: KhÃ´ng cÃ³ trÆ°á»ng Äá»‹a chá»‰ nháº­n hÃ ng.
3. Gá»­i request `POST /api/checkout` vá»›i `shipping_address: ""`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
- Form Checkout pháº£i cÃ³ Ã´ nháº­p Äá»‹a chá»‰ giao hÃ ng vá»›i nhÃ£n cÃ³ dáº¥u `*` Ä‘á».
- Backend pháº£i tá»« chá»‘i (HTTP 400 Bad Request) náº¿u Ä‘á»‹a chá»‰ giao hÃ ng bá»‹ Ä‘á»ƒ trá»‘ng.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
KhÃ´ng cÃ³ trÆ°á»ng Ä‘á»‹a chá»‰ trÃªn UI; Backend cháº¥p nháº­n táº¡o Ä‘Æ¡n hÃ ng vá»›i Ä‘á»‹a chá»‰ rá»—ng.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-03_Error_Missing_Shipping_Address_Field.png](../screenshots/pool%20B/EXE-03_Error_Missing_Shipping_Address_Field.png)
* Minh chá»©ng lÆ°u Ä‘Æ¡n hÃ ng rá»—ng Ä‘á»‹a chá»‰: [screenshots/pool B/EXE-09_Bug_BVA_Empty_Address_Accepted.png](../screenshots/pool%20B/EXE-09_Bug_BVA_Empty_Address_Accepted.png)

---

### ðŸ› BUG #4: BUG-FR08-04 (Medium - GUI Standard FR-21)

* **TiÃªu Ä‘á» Issue:** `[FR-08][FR-21][MEDIUM] Trang Checkout khÃ´ng cÃ³ tháº» <h1> nÃ o, vi pháº¡m quy chuáº©n giao diá»‡n chung`
* **NhÃ£n (Labels):** `bug`, `medium`, `FR-08`, `FR-21`, `gui-standards`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Medium

#### 1. MÃ´ táº£ lá»—i
Quy chuáº©n giao diá»‡n chung FR-21 quy Ä‘á»‹nh rÃµ: *"Má»—i trang cÃ³ Ä‘Ãºng 1 tháº» <h1> duy nháº¥t"*.
TrÃªn trang Checkout, tiÃªu Ä‘á» trang Ä‘ang Ä‘Æ°á»£c Ä‘á»‹nh dáº¡ng báº±ng tháº» `<h2>Thanh ToÃ¡n</h2>` thay vÃ¬ tháº» `<h1>`. Kiá»ƒm tra báº±ng lá»‡nh JavaScript `document.querySelectorAll('h1').length` tráº£ vá» káº¿t quáº£ báº±ng **`0`**.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Má»Ÿ trang `http://localhost:5173/checkout`.
2. Nháº¥n `F12` má»Ÿ Console trÃ¬nh duyá»‡t.
3. Cháº¡y lá»‡nh: `document.querySelectorAll('h1').length`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Káº¿t quáº£ tráº£ vá» Ä‘Ãºng `1` tháº» `<h1>`.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Káº¿t quáº£ tráº£ vá» `0` (khÃ´ng cÃ³ tháº» `<h1>` nÃ o).

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-01_GUI_TotalAmount_And_H1_Check.png](../screenshots/pool%20B/EXE-01_GUI_TotalAmount_And_H1_Check.png)

---

### ðŸ› BUG #5: BUG-FR08-05 (High - Business Logic / Cart Integrity)

* **TiÃªu Ä‘á» Issue:** `[FR-08][HIGH] Há»‡ thá»‘ng cho phÃ©p thanh toÃ¡n vÃ  táº¡o Ä‘Æ¡n hÃ ng khi giá» hÃ ng hoÃ n toÃ n trá»‘ng`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-08`, `business-logic`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ nghiá»‡p vá»¥ FR-08 quy Ä‘á»‹nh thanh toÃ¡n chá»‰ Ã¡p dá»¥ng cho giá» hÃ ng cÃ³ tá»‘i thiá»ƒu 1 sáº£n pháº©m ($\text{length} \ge 1$).
Khi gá»­i request API `POST /api/checkout` vá»›i danh sÃ¡ch sáº£n pháº©m rá»—ng (`items: []`) vÃ  `total_amount: 0`, Backend khÃ´ng kiá»ƒm tra dá»¯ liá»‡u giá» hÃ ng mÃ  tráº£ vá» HTTP 200 OK vÃ  táº¡o thÃ nh cÃ´ng má»™t Ä‘Æ¡n hÃ ng "ma" 0 Ä‘á»“ng trong CSDL SQLite.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Gá»­i request `POST /api/checkout` vá»›i Header JWT há»£p lá»‡ vÃ  body JSON:
```json
{
  "items": [],
  "shipping_address": "123 LÃª Lá»£i, Q1",
  "total_amount": 0
}
```
2. Kiá»ƒm tra pháº£n há»“i API vÃ  CSDL báº£ng `orders`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u vá»›i HTTP 400 Bad Request kÃ¨m thÃ´ng bÃ¡o lá»—i giá» hÃ ng rá»—ng.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
API tráº£ vá» HTTP 200 OK vÃ  lÆ°u Ä‘Æ¡n hÃ ng rá»—ng vÃ o CSDL.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-07_Bug_BVA_Empty_Cart_Accepted.png](../screenshots/pool%20B/EXE-07_Bug_BVA_Empty_Cart_Accepted.png)

---

### ðŸ› BUG #6: BUG-FR08-06 (High - Data Integrity)

* **TiÃªu Ä‘á» Issue:** `[FR-08][HIGH] Backend cháº¥p nháº­n táº¡o Ä‘Æ¡n hÃ ng vá»›i sá»‘ lÆ°á»£ng sáº£n pháº©m lÃ  sá»‘ nguyÃªn Ã¢m`
* **NhÃ£n (Labels):** `bug`, `high`, `FR-08`, `data-integrity`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** High
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** High

#### 1. MÃ´ táº£ lá»—i
Äáº·c táº£ nghiá»‡p vá»¥ quy Ä‘á»‹nh sá»‘ lÆ°á»£ng sáº£n pháº©m Ä‘áº·t mua báº¯t buá»™c pháº£i lÃ  sá»‘ nguyÃªn dÆ°Æ¡ng ($\ge 1$).
Khi gá»­i request táº¡o Ä‘Æ¡n hÃ ng vá»›i sá»‘ lÆ°á»£ng Ã¢m (`quantity: -2`), Backend `server.js` khÃ´ng kiá»ƒm tra giÃ¡ trá»‹ sá»‘ lÆ°á»£ng mÃ  váº«n tiáº¿p nháº­n vÃ  táº¡o Ä‘Æ¡n hÃ ng thÃ nh cÃ´ng vÃ o CSDL.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Gá»­i request `POST /api/checkout` vá»›i body:
```json
{
  "items": [{ "id": 1, "quantity": -2, "price": 30000000 }],
  "shipping_address": "123 LÃª Lá»£i",
  "total_amount": -60000000
}
```
2. Quan sÃ¡t pháº£n há»“i API.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request) yÃªu cáº§u sá»‘ lÆ°á»£ng pháº£i $\ge 1$.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
API tráº£ vá» HTTP 200 OK vÃ  lÆ°u Ä‘Æ¡n hÃ ng cÃ³ sá»‘ lÆ°á»£ng Ã¢m vÃ o CSDL.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-08_Bug_Negative_Quantity_Accepted.png](../screenshots/pool%20B/EXE-08_Bug_Negative_Quantity_Accepted.png)

---

### ðŸ› BUG #7: BUG-FR08-07 (Medium - Boundary Validation)

* **TiÃªu Ä‘á» Issue:** `[FR-08][MEDIUM] KhÃ´ng kiá»ƒm soÃ¡t giá»›i háº¡n Ä‘á»™ dÃ i tá»‘i Ä‘a 255 kÃ½ tá»± cho Äá»‹a chá»‰ giao hÃ ng khi checkout`
* **NhÃ£n (Labels):** `bug`, `medium`, `FR-08`, `boundary`
* **Má»©c Ä‘á»™ nghiÃªm trá»ng (Severity):** Medium
* **Má»©c Ä‘á»™ Æ°u tiÃªn (Priority):** Low/Medium

#### 1. MÃ´ táº£ lá»—i
Táº¡i ranh giá»›i kiá»ƒm thá»­ Max + 1 (256 kÃ½ tá»±), Backend API cho phÃ©p lÆ°u chuá»—i Ä‘á»‹a chá»‰ vÆ°á»£t quÃ¡ 255 kÃ½ tá»± vÃ o CSDL SQLite mÃ  khÃ´ng há» cÃ³ cÆ¡ cháº¿ validate hay cáº£nh bÃ¡o lá»—i.

#### 2. CÃ¡c bÆ°á»›c tÃ¡i hiá»‡n (Steps to Reproduce)
1. Gá»­i request `POST /api/checkout` vá»›i trÆ°á»ng `shipping_address` lÃ  chuá»—i 256 kÃ½ tá»±: `"A".repeat(256)`.
2. Kiá»ƒm tra CSDL SQLite báº£ng `orders`.

#### 3. Káº¿t quáº£ mong Ä‘á»£i (Expected Result)
Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request) hoáº·c giá»›i háº¡n Ä‘á»™ dÃ i khÃ´ng vÆ°á»£t quÃ¡ 255 kÃ½ tá»±.

#### 4. Káº¿t quáº£ thá»±c táº¿ (Actual Result)
Request Ä‘Æ°á»£c cháº¥p nháº­n vÃ  lÆ°u chuá»—i 256 kÃ½ tá»± thÃ nh cÃ´ng vÃ o SQLite.

#### 5. Báº±ng chá»©ng hÃ¬nh áº£nh (Screenshot)
* **HÃ¬nh áº£nh báº±ng chá»©ng:** [screenshots/pool B/EXE-11_Bug_BVA_Address_256Chars_Accepted.png](../screenshots/pool%20B/EXE-11_Bug_BVA_Address_256Chars_Accepted.png)

