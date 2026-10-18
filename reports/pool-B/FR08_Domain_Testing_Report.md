# BÃO CÃO KIá»‚M THá»¬ MIá»€N GIÃ TRá»Š VÃ€ PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN
## TÃNH NÄ‚NG FR-08: THANH TOÃN (CHECKOUT) (POOL B)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Ká»¹ thuáº­t Ã¡p dá»¥ng:** Domain Testing (Equivalence Partitioning - EP) & Boundary Value Analysis (3-Value BVA theo chuáº©n ISTQB)
* **Thá»i gian thá»±c hiá»‡n:** 2026-10-18

---

## 1. Äáº¶C Táº¢ YÃŠU Cáº¦U & PHÃ‚N TÃCH BIáº¾N Äáº¦U VÃ€O (SPECIFICATION ANALYSIS)

### 1.1 TrÃ­ch xuáº¥t Ä‘áº·c táº£ FR-08 tá»« System Requirements Specification
> * **FR-08: Thanh toÃ¡n (Checkout)**
>   * Chá»‰ ngÆ°á»i dÃ¹ng **Ä‘Ã£ Ä‘Äƒng nháº­p** má»›i tiáº¿n hÃ nh thanh toÃ¡n Ä‘Æ°á»£c.
>   * **Tá»•ng tiá»n thanh toÃ¡n** Ä‘Æ°á»£c tÃ­nh tá»± Ä‘á»™ng tá»« giá» hÃ ng vÃ  khÃ´ng cho phÃ©p ngÆ°á»i dÃ¹ng chá»‰nh sá»­a trá»±c tiáº¿p.
>   * Giao diá»‡n hiá»ƒn thá»‹ Ä‘áº§y Ä‘á»§ danh sÃ¡ch sáº£n pháº©m Ä‘áº·t mua.
>   * Backend pháº£i tá»± tÃ­nh láº¡i tá»•ng tiá»n; **khÃ´ng cháº¥p nháº­n** giÃ¡ trá»‹ `total_amount` do client gá»­i lÃªn.
>   * Sau thanh toÃ¡n thÃ nh cÃ´ng, **giá» hÃ ng Ä‘Æ°á»£c xÃ³a**.
> * **RÃ ng buá»™c giao diá»‡n & dá»¯ liá»‡u bá»• trá»£ (FR-21, FR-22, FR-18, SEC-02, SEC-04):**
>   * Äá»‹a chá»‰ giao hÃ ng (`shipping_address`): LÃ  trÆ°á»ng báº¯t buá»™c (cÃ³ dáº¥u `*` Ä‘á» cáº¡nh nhÃ£n theo FR-22), Ä‘á»™ dÃ i tá»« 1â€“255 kÃ½ tá»±, khÃ´ng render HTML/JavaScript (SEC-04).
>   * Giá» hÃ ng (`items`): Danh sÃ¡ch sáº£n pháº©m pháº£i cÃ³ tá»‘i thiá»ƒu 1 sáº£n pháº©m ($\ge 1$). KhÃ´ng thá»ƒ thanh toÃ¡n khi giá» rá»—ng.
>   * Sá»‘ lÆ°á»£ng (`quantity`): Báº¯t buá»™c lÃ  sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$ (FR-06, FR-07), cáº¥m sá»‘ 0 hoáº·c sá»‘ Ã¢m.
>   * Má»—i trang cÃ³ Ä‘Ãºng 1 tháº» `<h1>`, Ä‘Æ¡n vá»‹ tiá»n tá»‡ dÃ¹ng kÃ½ hiá»‡u `â‚«` cÃ³ phÃ¢n cÃ¡ch hÃ ng nghÃ¬n (FR-21).

### 1.2 Äiá»u kiá»‡n tiÃªn quyáº¿t (Preconditions)
* NgÆ°á»i dÃ¹ng Ä‘Ã£ Ä‘Äƒng nháº­p thÃ nh cÃ´ng vÃ o há»‡ thá»‘ng vá»›i tÃ i khoáº£n há»£p lá»‡: `test@eshop.com` / `Test1234!` (`id = 2`, `role = 'user'`).
* Token xÃ¡c thá»±c JWT Ä‘Æ°á»£c Ä‘Ã­nh kÃ¨m trong Header `Authorization: Bearer <token>`.

### 1.3 Danh sÃ¡ch biáº¿n Ä‘áº§u vÃ o vÃ  rÃ ng buá»™c ká»¹ thuáº­t

| TÃªn biáº¿n Ä‘áº§u vÃ o | Kiá»ƒu dá»¯ liá»‡u | Miá»n giÃ¡ trá»‹ há»£p lá»‡ (Valid Domain) | RÃ ng buá»™c ká»¹ thuáº­t & Nghiá»‡p vá»¥ (Constraints) |
| :--- | :--- | :--- | :--- |
| **`Authorization` (JWT Token)** | String (HTTP Header) | Chuá»—i `Bearer <valid_token>` há»£p lá»‡, cÃ²n háº¡n. | **Báº¯t buá»™c**: Chá»‰ ngÆ°á»i dÃ¹ng Ä‘Ã£ Ä‘Äƒng nháº­p má»›i Ä‘Æ°á»£c thanh toÃ¡n (FR-08, SEC-02). |
| **`Danh sÃ¡ch sáº£n pháº©m` (`items`)** | Array of Objects | Máº£ng khÃ´ng rá»—ng, chá»©a $\ge 1$ sáº£n pháº©m tá»« giá» hÃ ng. | Báº¯t buá»™c. Giao diá»‡n hiá»ƒn thá»‹ Ä‘áº§y Ä‘á»§ danh sÃ¡ch. Sau thanh toÃ¡n, giá» hÃ ng pháº£i bá»‹ xÃ³a sáº¡ch (FR-08). |
| **`Sá»‘ lÆ°á»£ng` (`quantity`)** | Integer | Sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$. | Báº¯t buá»™c cho tá»«ng dÃ²ng sáº£n pháº©m. Cáº¥m sá»‘ 0, sá»‘ Ã¢m, sá»‘ tháº­p phÃ¢n (FR-06, FR-07). |
| **`Äá»‹a chá»‰ giao hÃ ng` (`shipping_address`)** | String | Chuá»—i kÃ½ tá»± khÃ¡c rá»—ng, Ä‘á»™ dÃ i tá»« $1$ Ä‘áº¿n $255$ kÃ½ tá»±. | Báº¯t buá»™c (cÃ³ dáº¥u `*` theo FR-22). Escape an toÃ n chá»‘ng XSS, khÃ´ng render HTML (SEC-04). |
| **`Tá»•ng tiá»n thanh toÃ¡n` (`total_amount`)** | Number (Integer) | KhÃ´ng cÃ³ miá»n há»£p lá»‡ tá»« Client (Read-only trÃªn UI). | **RÃ ng buá»™c an ninh tá»‘i quan trá»ng (FR-08)**: UI cáº¥m sá»­a trá»±c tiáº¿p; Backend báº¯t buá»™c tá»± tÃ­nh láº¡i tá»« CSDL, cáº¥m tin tÆ°á»Ÿng client. |

---

## 2. PHÃ‚N VÃ™NG TÆ¯Æ NG ÄÆ¯Æ NG (EQUIVALENCE PARTITIONING - EP)

Dá»±a trÃªn cÃ¡c biáº¿n Ä‘áº§u vÃ o thuáº§n tÃºy cá»§a chá»©c nÄƒng Checkout (FR-08), phÃ¢n chia miá»n giÃ¡ trá»‹ thÃ nh cÃ¡c lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng:

| Biáº¿n | Lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng | Loáº¡i | GiÃ¡ trá»‹ Ä‘áº¡i diá»‡n | MÃ´ táº£ kiá»ƒm thá»­ |
| :--- | :--- | :---: | :--- | :--- |
| **`Authorization` (JWT)** | **EC1.1** | Valid | `Bearer <valid_jwt_token>` | NgÆ°á»i dÃ¹ng Ä‘Ã£ Ä‘Äƒng nháº­p vá»›i JWT Token há»£p lá»‡, cÃ²n háº¡n. |
| | **EC1.2** | Invalid | `""` (KhÃ´ng gá»­i Token) | ChÆ°a Ä‘Äƒng nháº­p / Gá»i API khÃ´ng cÃ³ Header Authorization. |
| **`Danh sÃ¡ch SP` (`items`)** | **EC2.1** | Valid | `[{id: 1, quantity: 1}]` | Giá» hÃ ng cÃ³ Ã­t nháº¥t 1 sáº£n pháº©m há»£p lá»‡ ($\text{length} \ge 1$). |
| | **EC2.2** | Invalid | `[]` (Máº£ng rá»—ng) | Giá» hÃ ng trá»‘ng nhÆ°ng gá»­i yÃªu cáº§u thanh toÃ¡n. |
| **`Sá»‘ lÆ°á»£ng` (`quantity`)** | **EC3.1** | Valid | `quantity = 1` | Sá»‘ nguyÃªn dÆ°Æ¡ng há»£p lá»‡ ($\ge 1$). |
| | **EC3.2** | Invalid | `quantity = 0` | Sá»‘ lÆ°á»£ng báº±ng 0. |
| | **EC3.3** | Invalid | `quantity = -2` | Sá»‘ lÆ°á»£ng lÃ  sá»‘ nguyÃªn Ã¢m ($< 0$). |
| **`Äá»‹a chá»‰ giao hÃ ng`** | **EC4.1** | Valid | `"123 LÃª Lá»£i, Q1"` | Chuá»—i Ä‘á»‹a chá»‰ há»£p lá»‡ (1â€“255 kÃ½ tá»±). |
| | **EC4.2** | Invalid | `""` (Rá»—ng) | Bá» trá»‘ng Ä‘á»‹a chá»‰ giao hÃ ng (0 kÃ½ tá»±). |
| | **EC4.3** | Invalid | `"A".repeat(256)` | Chuá»—i Ä‘á»‹a chá»‰ vÆ°á»£t quÃ¡ 255 kÃ½ tá»±. |
| | **EC4.4** | Invalid | `"<script>alert('XSS')</script>"` | Chuá»—i chá»©a mÃ£ Ä‘á»™c HTML/JavaScript (SEC-04). |
| **`Tá»•ng tiá»n thanh toÃ¡n`** | **EC5.1** | Valid | *(Read-only / Backend tá»± tÃ­nh)* | UI khÃ³a chá»‰nh sá»­a, Backend tá»± truy váº¥n Ä‘Æ¡n giÃ¡ tá»« CSDL. |
| | **EC5.2** | Invalid | `total_amount = 1000` | Client cá»‘ tÃ¬nh can thiá»‡p sá»­a giÃ¡ tiá»n ráº» máº¡t Ä‘á»ƒ trá»¥c lá»£i. |

---

## 3. PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN (BOUNDARY VALUE ANALYSIS - 3-VALUE BVA)

Ãp dá»¥ng ká»¹ thuáº­t 3-Value BVA theo chuáº©n ISTQB ($Min-1, Min, Min+1, Nominal, Max-1, Max, Max+1$):

### 3.1 Báº£ng phÃ¢n tÃ­ch giÃ¡ trá»‹ biÃªn 3 giÃ¡ trá»‹ cho FR-08

| Biáº¿n kiá»ƒm thá»­ | Ranh giá»›i (Boundary) | Min - 1 | Min | Min + 1 | Nominal | Max - 1 | Max | Max + 1 |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`items.length`** *(Sá»‘ SP trong giá»)* | Tá»‘i thiá»ƒu 1 sáº£n pháº©m ($\ge 1$) | `0 SP` *(Invalid)* | `1 SP` *(Valid)* | `2 SP` *(Valid)* | `3 SP` | *N/A (Open)* | *N/A (Open)* | *N/A (Open)* |
| **`quantity`** *(Sá»‘ lÆ°á»£ng má»—i SP)* | Sá»‘ nguyÃªn dÆ°Æ¡ng tá»‘i thiá»ƒu 1 ($\ge 1$) | `0` *(Invalid)* | `1` *(Valid)* | `2` *(Valid)* | `5` | *N/A (Open)* | *N/A (Open)* | *N/A (Open)* |
| **`shipping_address`** *(Äá»™ dÃ i chuá»—i)* | Äá»™ dÃ i tá»« 1 Ä‘áº¿n 255 kÃ½ tá»± | `0 kÃ½ tá»±` *(Invalid)* | `1 kÃ½ tá»±` *(Valid)* | `2 kÃ½ tá»±` *(Valid)* | `25 kÃ½ tá»±` | `254 kÃ½ tá»±` *(Valid)* | `255 kÃ½ tá»±` *(Valid)* | `256 kÃ½ tá»±` *(Invalid)* |
| **`total_amount`** *(GiÃ¡ trá»‹ Ä‘Æ¡n hÃ ng)* | Tá»•ng tiá»n lá»›n hÆ¡n 0 ($> 0$) | `0 â‚«` *(Invalid)* | `1.000 â‚«` *(Valid)* | `2.000 â‚«` *(Valid)* | `36.000.000 â‚«` | *N/A (Open)* | *N/A (Open)* | *N/A (Open)* |

---

## 4. Káº¾T QUáº¢ THá»°C THI KIá»‚M THá»¬ THá»°C Táº¾ (TEST EXECUTION RESULTS)

Táº¥t cáº£ 13 ká»‹ch báº£n kiá»ƒm thá»­ Ä‘Ã£ Ä‘Æ°á»£c thá»±c thi trá»±c tiáº¿p trÃªn há»‡ thá»‘ng EShop thá»±c táº¿ (Frontend `http://localhost:5173`, Backend `http://localhost:3000`, CSDL `database.sqlite`). ToÃ n bá»™ hÃ¬nh áº£nh báº±ng chá»©ng Ä‘Ã£ Ä‘Æ°á»£c lÆ°u trá»¯ Ä‘áº§y Ä‘á»§ táº¡i thÆ° má»¥c `screenshots/pool B/`.

| MÃ£ TC | Ká»‹ch báº£n kiá»ƒm thá»­ | Dá»¯ liá»‡u Ä‘áº§u vÃ o (Input) | Káº¿t quáº£ mong Ä‘á»£i (Expected Result) | Káº¿t quáº£ thá»±c táº¿ (Actual Result) | Tráº¡ng thÃ¡i | Báº±ng chá»©ng (Screenshot) | MÃ£ lá»—i (Bug ID) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| **EXE-01** | Kiá»ƒm tra GUI, tháº» `<h1>`, Ä‘Æ¡n vá»‹ `â‚«` & KhÃ³a trÆ°á»ng Tá»•ng tiá»n | Quan sÃ¡t form; Console: `document.querySelectorAll('h1').length` | ÄÃºng 1 tháº» `<h1>`; Ã´ Tá»•ng tiá»n á»Ÿ tráº¡ng thÃ¡i chá»‰ Ä‘á»c (disabled); Ä‘Æ¡n vá»‹ `â‚«`. | Ã” Tá»•ng tiá»n thanh toÃ¡n lÃ  tháº» `<input type="number">` cho phÃ©p ngÆ°á»i dÃ¹ng tá»± do sá»­a Ä‘á»•i sá»‘ tiá»n tÃ¹y Ã½ trÃªn giao diá»‡n thay vÃ¬ á»Ÿ tráº¡ng thÃ¡i chá»‰ Ä‘á»c (Read-only), vi pháº¡m nghiÃªm trá»ng Ä‘áº·c táº£ FR-08; khÃ´ng cÃ³ tháº» `<h1>` nÃ o (`h1 = 0`, vi pháº¡m FR-21). | **FAIL** | `EXE-01_GUI_TotalAmount_And_H1_Check.png` | **BUG-FR08-01**<br>**BUG-FR08-04** |
| **EXE-02** | Kiá»ƒm tra hiá»ƒn thá»‹ danh sÃ¡ch sáº£n pháº©m Ä‘áº·t mua | Giá» hÃ ng cÃ³ 2 SP: iPhone 15 Pro Max x 1, AirPods Pro 2 x 1 | Giao diá»‡n hiá»ƒn thá»‹ Ä‘áº§y Ä‘á»§ danh sÃ¡ch SP, sá»‘ lÆ°á»£ng, Ä‘Æ¡n giÃ¡ vÃ  thÃ nh tiá»n theo FR-08. | Giao diá»‡n Checkout hiá»ƒn thá»‹ Ä‘áº§y Ä‘á»§ danh sÃ¡ch sáº£n pháº©m Ä‘áº·t mua tá»« giá» hÃ ng kÃ¨m sá»‘ lÆ°á»£ng vÃ  thÃ nh tiá»n Ä‘Ãºng Ä‘á»‹nh dáº¡ng phÃ¢n cÃ¡ch hÃ ng nghÃ¬n (â‚«). | **PASS** | `EXE-02_GUI_Display_Cart_Items.png` | â€” |
| **EXE-03** | Kiá»ƒm tra trÆ°á»ng báº¯t buá»™c Äá»‹a chá»‰ giao hÃ ng vÃ  dáº¥u `*` Ä‘á» | Quan sÃ¡t form Checkout; tÃ¬m kiáº¿m Ã´ nháº­p Äá»‹a chá»‰ nháº­n hÃ ng | CÃ³ Ã´ nháº­p Äá»‹a chá»‰ giao hÃ ng cÃ³ dáº¥u `*` Ä‘á» (FR-22) hoáº·c káº¿ thá»«a Ä‘á»‹a chá»‰ máº·c Ä‘á»‹nh tá»« há»“ sÆ¡ cÃ¡ nhÃ¢n. | Giao diá»‡n Checkout hoÃ n toÃ n thiáº¿u Ã´ nháº­p trÆ°á»ng 'Äá»‹a chá»‰ giao hÃ ng' báº¯t buá»™c (FR-22) vÃ  khÃ´ng káº¿ thá»«a Ä‘á»‹a chá»‰ máº·c Ä‘á»‹nh tá»« há»“ sÆ¡ ngÆ°á»i dÃ¹ng (FR-04). Payload gá»­i lÃªn API POST /api/checkout hoÃ n toÃ n khÃ´ng cÃ³ tham sá»‘ `shipping_address`. | **FAIL** | `EXE-03_Error_Missing_Shipping_Address_Field.png` | **BUG-FR08-03** |
| **EXE-04** | Thanh toÃ¡n thÃ nh cÃ´ng luá»“ng chuáº©n (Nominal Flow: 36.000.000 â‚«) | ÄÃ£ Ä‘Äƒng nháº­p, giá» hÃ ng 2 SP ($36,000,000\text{ â‚«}$), báº¥m "XÃ¡c Nháº­n Thanh ToÃ¡n" | Thanh toÃ¡n thÃ nh cÃ´ng (HTTP 200), hiá»ƒn thá»‹ mÃ n hÃ¬nh thÃ nh cÃ´ng, lÆ°u Ä‘Æ¡n hÃ ng `status = 'pending'`. | Thanh toÃ¡n thÃ nh cÃ´ng (HTTP 200 OK), giao diá»‡n hiá»ƒn thá»‹ thÃ´ng bÃ¡o thÃ nh cÃ´ng. Báº£ng orders trong CSDL SQLite ghi nháº­n Ä‘Æ¡n hÃ ng má»›i vá»›i `total_amount = 36,000,000 â‚«`, `status = 'pending'`. | **PASS** | `EXE-04_Valid_Checkout_Nominal.png` | â€” |
| **EXE-05** | Kiá»ƒm tra tráº¡ng thÃ¡i Giá» hÃ ng sau khi thanh toÃ¡n thÃ nh cÃ´ng | Sau khi Ä‘áº·t hÃ ng á»Ÿ EXE-04, truy cáº­p `http://localhost:5173/cart` | Giá» hÃ ng pháº£i Ä‘Æ°á»£c xÃ³a sáº¡ch hoÃ n toÃ n (`items.length = 0`) theo quy Ä‘á»‹nh FR-08 dÃ²ng 108. | Sau khi thanh toÃ¡n thÃ nh cÃ´ng, giá» hÃ ng khÃ´ng há» Ä‘Æ°á»£c lÃ m trá»‘ng. Truy cáº­p trang Giá» hÃ ng váº«n tháº¥y nguyÃªn 2 sáº£n pháº©m cÅ©. | **FAIL** | `EXE-05_Bug_Cart_Not_Cleared_After_Checkout.png` | **BUG-FR08-02** |
| **EXE-06** | Giá» hÃ ng cÃ³ Ä‘Ãºng 1 sáº£n pháº©m (BiÃªn Min há»£p lá»‡ cá»§a `items.length`) | Giá» hÃ ng cÃ³ Ä‘Ãºng 1 SP: iPhone 15 Pro Max ($30,000,000\text{ â‚«}$, SL: 1) | Thanh toÃ¡n thÃ nh cÃ´ng (HTTP 200), Ä‘Æ¡n hÃ ng lÆ°u vÃ o CSDL vá»›i tá»•ng tiá»n 30.000.000 â‚«. | Há»‡ thá»‘ng xá»­ lÃ½ thanh toÃ¡n thÃ nh cÃ´ng vá»›i giá» hÃ ng cÃ³ Ä‘Ãºng 1 sáº£n pháº©m (BiÃªn Min há»£p lá»‡ cá»§a items.length). ÄÆ¡n hÃ ng Ä‘Æ°á»£c táº¡o thÃ nh cÃ´ng vá»›i tá»•ng tiá»n 30.000.000 â‚«. | **PASS** | `EXE-06_BVA_Min_1_Item_Success.png` | â€” |
| **EXE-07** | Äáº·t hÃ ng khi giá» hÃ ng trá»‘ng (BiÃªn Min - 1: `items = []`, tá»•ng tiá»n 0 â‚«) | Gá»­i request `POST /api/checkout` vá»›i `items = []`, `total_amount = 0` | Há»‡ thá»‘ng tá»« chá»‘i thanh toÃ¡n (HTTP 400 Bad Request), thÃ´ng bÃ¡o giá» hÃ ng trá»‘ng theo FR-08. | Há»‡ thá»‘ng cho phÃ©p táº¡o Ä‘Æ¡n hÃ ng khi giá» hÃ ng hoÃ n toÃ n trá»‘ng (`items.length = 0`, tá»•ng tiá»n báº±ng 0 â‚«). Backend `server.js` khÃ´ng kiá»ƒm tra sá»‘ lÆ°á»£ng sáº£n pháº©m hay tá»•ng tiá»n há»£p lá»‡, tráº£ vá» HTTP 200 vÃ  lÆ°u Ä‘Æ¡n hÃ ng rá»—ng vÃ o CSDL. | **FAIL** | `EXE-07_Bug_BVA_Empty_Cart_Accepted.png` | **BUG-FR08-05** |
| **EXE-08** | Äáº·t hÃ ng vá»›i sá»‘ lÆ°á»£ng sáº£n pháº©m lÃ  sá»‘ Ã¢m (`quantity = -2`) | Gá»­i request `POST /api/checkout` vá»›i `quantity = -2`, `total_amount = -60000000` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400), bÃ¡o lá»—i sá»‘ lÆ°á»£ng sáº£n pháº©m pháº£i lÃ  sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$. | Há»‡ thá»‘ng khÃ´ng kiá»ƒm tra tÃ­nh há»£p lá»‡ cá»§a sá»‘ lÆ°á»£ng sáº£n pháº©m, cháº¥p nháº­n Ä‘Æ¡n hÃ ng cÃ³ sá»‘ lÆ°á»£ng Ã¢m (`quantity = -2`) vá»›i message SUCCESS. | **FAIL** | `EXE-08_Bug_Negative_Quantity_Accepted.png` | **BUG-FR08-06** |
| **EXE-09** | Bá» trá»‘ng Äá»‹a chá»‰ giao hÃ ng (BiÃªn Min - 1: chuá»—i rá»—ng 0 kÃ½ tá»±) | Gá»­i request `POST /api/checkout` vá»›i `shipping_address = ""` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400), yÃªu cáº§u nháº­p Ä‘á»‹a chá»‰ báº¯t buá»™c theo FR-22. | Backend khÃ´ng kiá»ƒm tra tÃ­nh báº¯t buá»™c cá»§a trÆ°á»ng Äá»‹a chá»‰ giao hÃ ng, cho phÃ©p Ä‘áº·t hÃ ng thÃ nh cÃ´ng vÃ  lÆ°u chuá»—i Ä‘á»‹a chá»‰ rá»—ng vÃ o báº£ng orders trong CSDL. | **FAIL** | `EXE-09_Bug_BVA_Empty_Address_Accepted.png` | **BUG-FR08-03** |
| **EXE-10** | Äá»‹a chá»‰ giao hÃ ng táº¡i biÃªn dÆ°á»›i há»£p lá»‡ (BiÃªn Min: 1 kÃ½ tá»± `"A"`) | Gá»­i request `POST /api/checkout` vá»›i `shipping_address = "A"` | Cháº¥p nháº­n Ä‘á»™ dÃ i tá»‘i thiá»ƒu 1 kÃ½ tá»±, táº¡o Ä‘Æ¡n hÃ ng thÃ nh cÃ´ng (HTTP 200). | Há»‡ thá»‘ng cháº¥p nháº­n Ä‘á»™ dÃ i tá»‘i thiá»ƒu 1 kÃ½ tá»± cho trÆ°á»ng Äá»‹a chá»‰ giao hÃ ng (BiÃªn Min há»£p lá»‡). ÄÆ¡n hÃ ng lÆ°u thÃ nh cÃ´ng vÃ o CSDL. | **PASS** | `EXE-10_BVA_Address_Min_1Char_Valid.png` | â€” |
| **EXE-11** | Äá»‹a chá»‰ giao hÃ ng vÆ°á»£t biÃªn trÃªn (BiÃªn Max + 1: chuá»—i 256 kÃ½ tá»±) | Gá»­i request `POST /api/checkout` vá»›i `shipping_address = "A".repeat(256)` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400) hoáº·c giá»›i háº¡n Ä‘á»™ dÃ i khÃ´ng vÆ°á»£t quÃ¡ 255 kÃ½ tá»±. | Há»‡ thá»‘ng khÃ´ng kiá»ƒm soÃ¡t giá»›i háº¡n Ä‘á»™ dÃ i tá»‘i Ä‘a 255 kÃ½ tá»± cá»§a Äá»‹a chá»‰ giao hÃ ng. Request chá»©a chuá»—i 256 kÃ½ tá»± váº«n Ä‘Æ°á»£c cháº¥p nháº­n vÃ  lÆ°u vÃ o CSDL mÃ  khÃ´ng cÃ³ cáº£nh bÃ¡o lá»—i. | **FAIL** | `EXE-11_Bug_BVA_Address_256Chars_Accepted.png` | **BUG-FR08-07** |
| **EXE-12** | Gian láº­n giÃ¡ thanh toÃ¡n (Price Tampering: Client gá»­i 1.000 â‚«) | Giá» hÃ ng thá»±c táº¿ $36,000,000\text{ â‚«}$, Client gá»­i payload `total_amount = 1000` | Backend pháº£i tá»± tÃ­nh láº¡i tá»•ng tiá»n tá»« CSDL ($36,000,000\text{ â‚«}$) hoáº·c tá»« chá»‘i Ä‘Æ¡n hÃ ng gian láº­n giÃ¡. | Lá»— há»•ng gian láº­n giÃ¡ nghiÃªm trá»ng: client cÃ³ thá»ƒ sá»­a tá»•ng tiá»n trÃªn trang thanh toÃ¡n mÃ  há»‡ thá»‘ng tin tÆ°á»Ÿng vÃ o sá»‘ tiá»n nÃ y. ÄÆ¡n hÃ ng mua sáº£n pháº©m 36.000.000 â‚« Ä‘Æ°á»£c lÆ°u vÃ o CSDL vá»›i sá»‘ tiá»n chá»‰ 1.000 â‚«. | **FAIL** | `EXE-12_Bug_Critical_Price_Tampering.png` | **BUG-FR08-01** |
| **EXE-13** | Kiá»ƒm tra xÃ¡c thá»±c Token: Thanh toÃ¡n khi chÆ°a Ä‘Äƒng nháº­p | Gá»­i request `POST /api/checkout` khÃ´ng cÃ³ Header `Authorization` | Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u (HTTP 401 Unauthorized), yÃªu cáº§u Ä‘Äƒng nháº­p theo FR-08 vÃ  SEC-02. | Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u thanh toÃ¡n khi khÃ´ng cÃ³ JWT Token xÃ¡c thá»±c. | **PASS** | `EXE-13_Security_Missing_Token_401.png` | â€” |

---

## 5. Tá»”NG Há»¢P CÃC Lá»–I PHÃT HIá»†N (DEFECT SUMMARY)

Tá»•ng sá»‘ test cases thá»±c thi: **13**  
* **Passed:** 5/13 (38.46%)  
* **Failed (PhÃ¡t hiá»‡n lá»—i):** 8/13 (61.54%)  

### Danh má»¥c 7 lá»—i phÃ¡t hiá»‡n Ä‘Æ°á»£c:
1. **BUG-FR08-01 (Critical - Security / Price Tampering)**: Ã” Tá»•ng tiá»n trÃªn giao diá»‡n cho phÃ©p ngÆ°á»i dÃ¹ng tá»± do nháº­p sá»‘ tiá»n tÃ¹y Ã½; Backend `server.js` khÃ´ng tá»± tÃ­nh láº¡i tá»•ng tiá»n tá»« CSDL mÃ  tin tÆ°á»Ÿng hoÃ n toÃ n vÃ o `total_amount` do client gá»­i lÃªn, cho phÃ©p mua Ä‘Æ¡n hÃ ng 36.000.000 â‚« vá»›i giÃ¡ chá»‰ 1.000 â‚« (vi pháº¡m trá»±c tiáº¿p FR-08).
2. **BUG-FR08-02 (High - State Transition / Data Integrity)**: Giá» hÃ ng khÃ´ng Ä‘Æ°á»£c lÃ m sáº¡ch sau khi thanh toÃ¡n thÃ nh cÃ´ng. `Checkout.jsx` dÃ²ng 61 khÃ´ng gá»i hÃ m `clearCart()` vÃ  backend khÃ´ng dá»n sáº¡ch `userCarts`, vi pháº¡m háº­u Ä‘iá»u kiá»‡n cá»§a FR-08.
3. **BUG-FR08-03 (High - Missing Mandatory Field & Validation)**: Giao diá»‡n Checkout thiáº¿u hoÃ n toÃ n Ã´ nháº­p trÆ°á»ng báº¯t buá»™c "Äá»‹a chá»‰ giao hÃ ng", vÃ  backend cháº¥p nháº­n lÆ°u Ä‘Æ¡n hÃ ng khÃ´ng cÃ³ Ä‘á»‹a chá»‰ (rá»—ng) vÃ o CSDL SQLite (vi pháº¡m FR-08, FR-22).
4. **BUG-FR08-04 (Medium - GUI Standard FR-21)**: Trang Checkout khÃ´ng cÃ³ tháº» `<h1>` nÃ o (Ä‘ang dÃ¹ng tháº» `<h2>`), vi pháº¡m quy chuáº©n giao diá»‡n chung FR-21.
5. **BUG-FR08-05 (High - Business Logic / Cart Integrity)**: Backend cho phÃ©p táº¡o Ä‘Æ¡n hÃ ng khi giá» hÃ ng hoÃ n toÃ n trá»‘ng (`items.length = 0`) vÃ  tá»•ng tiá»n báº±ng 0 â‚« vá»›i HTTP 200 OK.
6. **BUG-FR08-06 (High - Data Integrity)**: Backend cháº¥p nháº­n táº¡o Ä‘Æ¡n hÃ ng vá»›i sáº£n pháº©m cÃ³ sá»‘ lÆ°á»£ng Ã¢m (`quantity = -2`) vá»›i HTTP 200 OK, vi pháº¡m tÃ­nh toÃ n váº¹n dá»¯ liá»‡u Ä‘Æ¡n hÃ ng.
7. **BUG-FR08-07 (Medium - Boundary Validation)**: KhÃ´ng kiá»ƒm soÃ¡t giá»›i háº¡n Ä‘á»™ dÃ i tá»‘i Ä‘a 255 kÃ½ tá»± cho Äá»‹a chá»‰ giao hÃ ng, cho phÃ©p lÆ°u chuá»—i 256 kÃ½ tá»± vÃ o CSDL mÃ  khÃ´ng cÃ³ cáº£nh bÃ¡o lá»—i.

