# BÃO CÃO KIá»‚M THá»¬ MIá»€N GIÃ TRá»Š VÃ€ PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN
## TÃNH NÄ‚NG FR-17: QUáº¢N LÃ MÃƒ GIáº¢M GIÃ (COUPON CRUD) (POOL C)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Ká»¹ thuáº­t Ã¡p dá»¥ng:** Domain Testing (Equivalence Partitioning - EP) & Boundary Value Analysis (3-Value BVA theo chuáº©n ISTQB)
* **Thá»i gian thá»±c hiá»‡n:** 2026-10-18

---

## 1. Äáº¶C Táº¢ YÃŠU Cáº¦U & PHÃ‚N TÃCH BIáº¾N Äáº¦U VÃ€O (SPECIFICATION ANALYSIS)

### 1.1 TrÃ­ch xuáº¥t Ä‘áº·c táº£ FR-17 tá»« System Requirements Specification
> * **FR-17: Quáº£n lÃ½ MÃ£ Giáº£m GiÃ¡ (Coupon CRUD)**
>   * PhÃ¢n há»‡ Web Admin cho phÃ©p Quáº£n trá»‹ viÃªn quáº£n lÃ½ danh sÃ¡ch mÃ£ giáº£m giÃ¡.
>   * **ThÃªm má»›i mÃ£ giáº£m giÃ¡ (Create):** Quáº£n trá»‹ viÃªn nháº­p thÃ´ng tin mÃ£ gá»“m:
>     * `code`: MÃ£ giáº£m giÃ¡ (chuá»—i kÃ½ tá»±, duy nháº¥t trong há»‡ thá»‘ng).
>     * `discount_type`: Loáº¡i giáº£m giÃ¡ (theo pháº§n trÄƒm `'percent'` hoáº·c sá»‘ tiá»n cá»‘ Ä‘á»‹nh `'fixed'`).
>     * `discount_value`: GiÃ¡ trá»‹ giáº£m giÃ¡ (pháº£i lÃ  sá»‘ dÆ°Æ¡ng $> 0$; náº¿u loáº¡i `percent` thÃ¬ trong khoáº£ng $1..100$, náº¿u loáº¡i `fixed` thÃ¬ $> 0$).
>     * `min_order_amount`: GiÃ¡ trá»‹ Ä‘Æ¡n hÃ ng tá»‘i thiá»ƒu Ä‘á»ƒ Ã¡p dá»¥ng mÃ£ (sá»‘ nguyÃªn $\ge 0$).
>     * `max_uses_per_user`: Sá»‘ láº§n sá»­ dá»¥ng tá»‘i Ä‘a cá»§a má»—i ngÆ°á»i dÃ¹ng (sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$).
>   * **Xem danh sÃ¡ch (Read):** Hiá»ƒn thá»‹ toÃ n bá»™ mÃ£ giáº£m giÃ¡ Ä‘ang cÃ³ trong há»‡ thá»‘ng (`GET /api/coupons`).
>   * **XÃ³a mÃ£ giáº£m giÃ¡ (Delete):** Cho phÃ©p Quáº£n trá»‹ viÃªn xÃ³a mÃ£ giáº£m giÃ¡ theo `id` (`DELETE /api/admin/coupons/:id`).
> * **RÃ ng buá»™c phÃ¢n quyá»n & an toÃ n há»‡ thá»‘ng (FR-12, FR-22, SEC-02, SEC-03, SEC-04):**
>   * **Kiá»ƒm soÃ¡t truy cáº­p (Access Control - FR-12, SEC-03):** CÃ¡c endpoint quáº£n trá»‹ `/api/admin/*` báº¯t buá»™c pháº£i cÃ³ JWT Token há»£p lá»‡ vÃ  ngÆ°á»i dÃ¹ng pháº£i cÃ³ vai trÃ² Quáº£n trá»‹ viÃªn (`role = 'admin'`). NgÆ°á»i dÃ¹ng thÃ´ng thÆ°á»ng (`role = 'user'`) hoáº·c khÃ¡ch vÃ£ng lai khÃ´ng Ä‘Æ°á»£c phÃ©p táº¡o hoáº·c xÃ³a mÃ£.
>   * **RÃ ng buá»™c biá»ƒu máº«u (FR-22):** CÃ¡c trÆ°á»ng dá»¯ liá»‡u báº¯t buá»™c pháº£i cÃ³ nhÃ£n rÃµ rÃ ng, thÃ´ng bÃ¡o lá»—i validation dá»… hiá»ƒu.
>   * **An ninh chá»‘ng XSS (SEC-04):** TrÆ°á»ng `code` khi nháº­p mÃ£ HTML/Script pháº£i Ä‘Æ°á»£c sanitize hoáº·c escape an toÃ n, khÃ´ng Ä‘Æ°á»£c thá»±c thi script trÃªn trÃ¬nh duyá»‡t.

### 1.2 Äiá»u kiá»‡n tiÃªn quyáº¿t (Preconditions)
* MÃ´i trÆ°á»ng SUT: Frontend cháº¡y táº¡i `http://localhost:5173`, Backend cháº¡y táº¡i `http://localhost:3000`, CSDL `database.sqlite`.
* TÃ i khoáº£n Quáº£n trá»‹ viÃªn (Admin): `admin@eshop.com` / `Admin1234!` (`id = 1`, `role = 'admin'`).
* TÃ i khoáº£n NgÆ°á»i dÃ¹ng thÆ°á»ng (Non-Admin): `test@eshop.com` / `Test1234!` (`id = 2`, `role = 'user'`).

### 1.3 Danh sÃ¡ch biáº¿n Ä‘áº§u vÃ o vÃ  rÃ ng buá»™c ká»¹ thuáº­t

| TÃªn biáº¿n Ä‘áº§u vÃ o | Kiá»ƒu dá»¯ liá»‡u | Miá»n giÃ¡ trá»‹ há»£p lá»‡ (Valid Domain) | RÃ ng buá»™c ká»¹ thuáº­t & Nghiá»‡p vá»¥ (Constraints) |
| :--- | :--- | :--- | :--- |
| **`Authorization` (JWT Token)** | String (HTTP Header) | `Bearer <valid_admin_token>` | **Báº¯t buá»™c**: Báº¯t buá»™c Ä‘Äƒng nháº­p vÃ  ngÆ°á»i dÃ¹ng pháº£i cÃ³ quyá»n Admin (`role = 'admin'`) theo FR-12 vÃ  SEC-03. |
| **`MÃ£ giáº£m giÃ¡` (`code`)** | String | Chuá»—i tá»« $1$ Ä‘áº¿n $50$ kÃ½ tá»±, chá»¯ hoa/sá»‘, duy nháº¥t (Unique). | Báº¯t buá»™c (FR-22). KhÃ´ng trÃ¹ng láº·p vá»›i mÃ£ Ä‘Ã£ cÃ³. Escape an toÃ n chá»‘ng Stored XSS (SEC-04). |
| **`Loáº¡i giáº£m giÃ¡` (`discount_type`)** | Enum String | `'percent'` hoáº·c `'fixed'` | Báº¯t buá»™c chá»n 1 trong 2 loáº¡i há»£p lá»‡. XÃ¡c Ä‘á»‹nh Ã½ nghÄ©a cho `discount_value`. |
| **`Má»©c giáº£m` (`discount_value`)** | Number (Integer/Float) | Náº¿u `percent`: $[1..100]$<br>Náº¿u `fixed`: $[1..\text{Infinity})$ | Báº¯t buá»™c lÃ  sá»‘ dÆ°Æ¡ng $> 0$. KhÃ´ng Ä‘Æ°á»£c báº±ng 0, khÃ´ng Ä‘Æ°á»£c Ã¢m. Loáº¡i `percent` khÃ´ng Ä‘Æ°á»£c vÆ°á»£t quÃ¡ 100%. |
| **`ÄÆ¡n hÃ ng tá»‘i thiá»ƒu` (`min_order_amount`)** | Number (Integer) | $[0..\text{Infinity})$ | Báº¯t buá»™c lÃ  sá»‘ nguyÃªn $\ge 0$. Máº·c Ä‘á»‹nh báº±ng 0 náº¿u khÃ´ng yÃªu cáº§u giÃ¡ trá»‹ Ä‘Æ¡n tá»‘i thiá»ƒu. Cáº¥m sá»‘ Ã¢m. |
| **`LÆ°á»£t dÃ¹ng tá»‘i Ä‘a` (`max_uses_per_user`)** | Number (Integer) | $[1..\text{Infinity})$ | Báº¯t buá»™c lÃ  sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$. Cáº¥m sá»‘ 0 hoáº·c sá»‘ Ã¢m. |
| **`ID mÃ£ giáº£m giÃ¡` (`coupon_id`)** *(cho Delete)* | Integer | Sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$, Ä‘ang tá»“n táº¡i trong báº£ng `coupons`. | Báº¯t buá»™c khi gá»i `DELETE /api/admin/coupons/:id`. ID khÃ´ng tá»“n táº¡i pháº£i tráº£ vá» 404 Not Found. |

---

## 2. PHÃ‚N VÃ™NG TÆ¯Æ NG ÄÆ¯Æ NG (EQUIVALENCE PARTITIONING - EP)

Dá»±a trÃªn cÃ¡c biáº¿n Ä‘áº§u vÃ o thuáº§n tÃºy cá»§a tÃ­nh nÄƒng Quáº£n lÃ½ MÃ£ Giáº£m GiÃ¡ (FR-17), phÃ¢n chia miá»n giÃ¡ trá»‹ thÃ nh cÃ¡c lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng:

| Biáº¿n Ä‘áº§u vÃ o | Lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng | Loáº¡i | GiÃ¡ trá»‹ Ä‘áº¡i diá»‡n | Má»¥c tiÃªu kiá»ƒm thá»­ |
| :--- | :--- | :---: | :--- | :--- |
| **`Authorization`** | **EC1.1** | Valid | Token cÃ³ `role = 'admin'` | TÃ i khoáº£n Quáº£n trá»‹ viÃªn há»£p lá»‡ thá»±c hiá»‡n thao tÃ¡c CRUD. |
| | **EC1.2** | Invalid | Token cÃ³ `role = 'user'` | TÃ i khoáº£n thÃ´ng thÆ°á»ng cá»‘ tÃ¬nh gá»i API Admin (Kiá»ƒm tra RBAC - SEC-03). |
| | **EC1.3** | Invalid | `""` (KhÃ´ng gá»­i Token) | KhÃ´ng cÃ³ Token xÃ¡c thá»±c, gá»i API trÃ¡i phÃ©p (Kiá»ƒm tra 401 Unauthorized). |
| **`MÃ£ code`** | **EC2.1** | Valid | `"SUMMER2026"` | Chuá»—i kÃ½ tá»± há»£p lá»‡, chÆ°a tá»«ng tá»“n táº¡i trong CSDL. |
| | **EC2.2** | Invalid | `""` (Chuá»—i rá»—ng) | Bá» trá»‘ng mÃ£ giáº£m giÃ¡. |
| | **EC2.3** | Invalid | `"WELCOME10"` (ÄÃ£ cÃ³ sáºµn) | Nháº­p mÃ£ bá»‹ trÃ¹ng láº·p vá»›i mÃ£ Ä‘Ã£ tá»“n táº¡i trong CSDL. |
| | **EC2.4** | Security | `"<script>alert(1)</script>"` | Nháº­p payload mÃ£ Ä‘á»™c XSS vÃ o mÃ£ code (SEC-04). |
| **`Loáº¡i giáº£m giÃ¡`** | **EC3.1** | Valid | `'percent'` | Giáº£m giÃ¡ theo tá»· lá»‡ pháº§n trÄƒm (1% â€“ 100%). |
| | **EC3.2** | Valid | `'fixed'` | Giáº£m giÃ¡ theo sá»‘ tiá»n cá»‘ Ä‘á»‹nh (VND). |
| **`Má»©c giáº£m` (`discount_value`)** | **EC4.1** | Valid | `20` (khi type = 'percent') | Tá»· lá»‡ pháº§n trÄƒm há»£p lá»‡ náº±m trong khoáº£ng $[1..100]$. |
| | **EC4.2** | Valid | `50000` (khi type = 'fixed') | Sá»‘ tiá»n giáº£m cá»‘ Ä‘á»‹nh há»£p lá»‡ ($> 0$). |
| | **EC4.3** | Invalid | `0` | Má»©c giáº£m báº±ng 0 (BiÃªn dÆ°á»›i khÃ´ng há»£p lá»‡). |
| | **EC4.4** | Invalid | `-50000` | Má»©c giáº£m lÃ  sá»‘ Ã¢m ($< 0$). |
| | **EC4.5** | Invalid | `150` (khi type = 'percent') | Pháº§n trÄƒm giáº£m vÆ°á»£t quÃ¡ 100% ($> 100$). |
| **`ÄÆ¡n tá»‘i thiá»ƒu` (`min_order_amount`)** | **EC5.1** | Valid | `0` | BiÃªn Min há»£p lá»‡: ÄÆ¡n hÃ ng tá»‘i thiá»ƒu báº±ng 0 â‚« (Ã¡p dá»¥ng má»i Ä‘Æ¡n). |
| | **EC5.2** | Valid | `200000` | Sá»‘ tiá»n Ä‘Æ¡n tá»‘i thiá»ƒu há»£p lá»‡ ($> 0$). |
| | **EC5.3** | Invalid | `-10000` | Sá»‘ tiá»n Ä‘Æ¡n tá»‘i thiá»ƒu lÃ  sá»‘ Ã¢m ($< 0$). |
| **`LÆ°á»£t dÃ¹ng tá»‘i Ä‘a` (`max_uses_per_user`)** | **EC6.1** | Valid | `1` | BiÃªn Min há»£p lá»‡: Má»—i user Ä‘Æ°á»£c dÃ¹ng 1 láº§n. |
| | **EC6.2** | Valid | `5` | Sá»‘ láº§n dÃ¹ng há»£p lá»‡ ($> 1$). |
| | **EC6.3** | Invalid | `0` | LÆ°á»£t dÃ¹ng báº±ng 0 (BiÃªn dÆ°á»›i khÃ´ng há»£p lá»‡). |
| | **EC6.4** | Invalid | `-1` | LÆ°á»£t dÃ¹ng lÃ  sá»‘ Ã¢m ($< 0$). |
| **`ID mÃ£ giáº£m giÃ¡` (`coupon_id`)** | **EC7.1** | Valid | `ID = 2` | ID sá»‘ nguyÃªn dÆ°Æ¡ng tá»“n táº¡i trong CSDL (XÃ³a thÃ nh cÃ´ng). |
| | **EC7.2** | Invalid | `ID = 99999` | ID sá»‘ nguyÃªn dÆ°Æ¡ng khÃ´ng tá»“n táº¡i trong CSDL (Pháº£i tráº£ vá» 404). |

---

## 3. PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN (BOUNDARY VALUE ANALYSIS - 3-VALUE BVA)

Ãp dá»¥ng ká»¹ thuáº­t 3-Value BVA theo chuáº©n ISTQB ($Min-1, Min, Min+1, Nominal, Max-1, Max, Max+1$):

### 3.1 Báº£ng phÃ¢n tÃ­ch giÃ¡ trá»‹ biÃªn 3 giÃ¡ trá»‹ cho FR-17

| Biáº¿n kiá»ƒm thá»­ | Ranh giá»›i (Boundary) | Min - 1 | Min | Min + 1 | Nominal | Max - 1 | Max | Max + 1 |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`discount_value`** *(percent)* | Äoáº¡n há»£p lá»‡ $[1..100]$ | `0` *(Invalid)* | `1` *(Valid)* | `2` *(Valid)* | `20` | `99` *(Valid)* | `100` *(Valid)* | `101` *(Invalid)* |
| **`discount_value`** *(fixed)* | Sá»‘ dÆ°Æ¡ng $\ge 1$ | `0` *(Invalid)* | `1` *(Valid)* | `2` *(Valid)* | `50.000` | *N/A (Open)* | *N/A (Open)* | *N/A (Open)* |
| **`min_order_amount`** | Sá»‘ khÃ´ng Ã¢m $\ge 0$ | `-1` *(Invalid)* | `0` *(Valid)* | `1` *(Valid)* | `200.000` | *N/A (Open)* | *N/A (Open)* | *N/A (Open)* |
| **`max_uses_per_user`** | Sá»‘ nguyÃªn dÆ°Æ¡ng $\ge 1$ | `0` *(Invalid)* | `1` *(Valid)* | `2` *(Valid)* | `3` | *N/A (Open)* | *N/A (Open)* | *N/A (Open)* |
| **`coupon_id`** *(Delete)* | ID tá»“n táº¡i trong CSDL | `0` *(Invalid)* | `1` *(Valid)* | `2` *(Valid)* | `Existing ID` | â€” | `Max ID` | `99999` *(Invalid)* |

---

## 4. Káº¾T QUáº¢ THá»°C THI KIá»‚M THá»¬ THá»°C Táº¾ (TEST EXECUTION RESULTS)

Táº¥t cáº£ 14 ká»‹ch báº£n kiá»ƒm thá»­ Ä‘Ã£ Ä‘Æ°á»£c thá»±c thi trá»±c tiáº¿p trÃªn há»‡ thá»‘ng EShop thá»±c táº¿ (Frontend `http://localhost:5173`, Backend `http://localhost:3000`, CSDL `database.sqlite`). ToÃ n bá»™ hÃ¬nh áº£nh báº±ng chá»©ng Ä‘Ã£ Ä‘Æ°á»£c lÆ°u trá»¯ Ä‘áº§y Ä‘á»§ táº¡i thÆ° má»¥c `screenshots/pool C/`.

| MÃ£ TC | Ká»‹ch báº£n kiá»ƒm thá»­ | Dá»¯ liá»‡u Ä‘áº§u vÃ o (Input) | Káº¿t quáº£ mong Ä‘á»£i (Expected Result) | Káº¿t quáº£ thá»±c táº¿ (Actual Result) | Tráº¡ng thÃ¡i | Báº±ng chá»©ng (Screenshot) | MÃ£ lá»—i (Bug ID) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| **EXE-01** | Táº¡o mÃ£ giáº£m giÃ¡ loáº¡i pháº§n trÄƒm há»£p lá»‡ (Nominal Percent) | `code: "SUMMER2026"`, `type: "percent"`, `value: 20`, `min_order: 200000`, `max_uses: 1` | ThÃªm má»›i thÃ nh cÃ´ng (HTTP 200), hiá»ƒn thá»‹ trong danh sÃ¡ch mÃ£ vÃ  lÆ°u vÃ o CSDL. | ThÃªm má»›i thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ loáº¡i pháº§n trÄƒm SUMMER2026. | **PASS** | `EXE-01_Valid_Create_Percent_Coupon.png` | â€” |
| **EXE-02** | Táº¡o mÃ£ giáº£m giÃ¡ loáº¡i cá»‘ Ä‘á»‹nh vá»›i Ä‘Æ¡n tá»‘i thiá»ƒu báº±ng 0 (BiÃªn Min `min_order_amount`) | `code: "FREE50K"`, `type: "fixed"`, `value: 50000`, `min_order: 0`, `max_uses: 3` | Táº¡o thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ (HTTP 200), cháº¥p nháº­n biÃªn Min `min_order = 0`. | Táº¡o thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ loáº¡i cá»‘ Ä‘á»‹nh FREE50K vá»›i Ä‘Æ¡n tá»‘i thiá»ƒu báº±ng 0 â‚« (BiÃªn Min há»£p lá»‡ cá»§a min_order_amount). | **PASS** | `EXE-02_Valid_Create_Fixed_Coupon_ZeroMin.png` | â€” |
| **EXE-03** | XÃ³a mÃ£ giáº£m giÃ¡ Ä‘ang tá»“n táº¡i há»£p lá»‡ | `DELETE /api/admin/coupons/:id` vá»›i ID cá»§a coupon Ä‘ang tá»“n táº¡i | XÃ³a mÃ£ thÃ nh cÃ´ng (HTTP 200), mÃ£ biáº¿n máº¥t khá»i danh sÃ¡ch vÃ  báº£ng `coupons`. | XÃ³a thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ Ä‘ang tá»“n táº¡i. | **PASS** | `EXE-03_Valid_Delete_Coupon.png` | â€” |
| **EXE-04** | Kiá»ƒm tra phÃ¢n quyá»n: NgÆ°á»i dÃ¹ng thÆ°á»ng táº¡o mÃ£ giáº£m giÃ¡ | Token cá»§a `test@eshop.com` (`role = 'user'`), payload táº¡o mÃ£ giáº£m 1.000.000 â‚« | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 403 Forbidden), chá»‰ cho phÃ©p Admin theo FR-12, SEC-03. | Lá»— há»•ng phÃ¢n quyá»n nghiÃªm trá»ng: TÃ i khoáº£n thÆ°á»ng (`test@eshop.com`) cÃ³ thá»ƒ tá»± táº¡o mÃ£ giáº£m giÃ¡ 1.000.000 â‚« thÃ nh cÃ´ng (HTTP 200 OK). Backend khÃ´ng kiá»ƒm tra quyá»n admin. | **FAIL** | `EXE-04_Bug_Critical_NonAdmin_Create_Coupon.png` | **BUG-FR17-01** |
| **EXE-05** | Kiá»ƒm tra phÃ¢n quyá»n: NgÆ°á»i dÃ¹ng thÆ°á»ng xÃ³a mÃ£ giáº£m giÃ¡ | Token cá»§a `test@eshop.com` (`role = 'user'`), gá»i `DELETE /api/admin/coupons/:id` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 403 Forbidden), chá»‰ cho phÃ©p Admin xÃ³a mÃ£. | Há»‡ thá»‘ng cho phÃ©p ngÆ°á»i dÃ¹ng thÃ´ng thÆ°á»ng gá»­i request xÃ³a thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ trong há»‡ thá»‘ng (HTTP 200 OK). Backend thiáº¿u kiá»ƒm tra `role === 'admin'`. | **FAIL** | `EXE-05_Bug_Critical_NonAdmin_Delete_Coupon.png` | **BUG-FR17-01** |
| **EXE-06** | Kiá»ƒm tra xÃ¡c thá»±c Token: Gá»i API táº¡o mÃ£ khi khÃ´ng cÃ³ Token | Gá»­i request `POST /api/admin/coupons` khÃ´ng cÃ³ Header `Authorization` | Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u (HTTP 401 Unauthorized) theo SEC-02. | Há»‡ thá»‘ng tá»« chá»‘i yÃªu cáº§u táº¡o mÃ£ giáº£m giÃ¡ khi khÃ´ng cÃ³ Token xÃ¡c thá»±c, tráº£ vá» mÃ£ lá»—i HTTP 401. | **PASS** | `EXE-06_Security_Missing_Token_401.png` | â€” |
| **EXE-07** | Nháº­p mÃ£ giáº£m giÃ¡ bá»‹ trÃ¹ng láº·p (Duplicate Code) | Táº¡o coupon vá»›i `code = "WELCOME10"` (Ä‘Ã£ tá»“n táº¡i trong há»‡ thá»‘ng) | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request / 409 Conflict), thÃ´ng bÃ¡o mÃ£ code Ä‘Ã£ tá»“n táº¡i. | Khi nháº­p mÃ£ bá»‹ trÃ¹ng láº·p, Backend khÃ´ng xá»­ lÃ½ lá»—i validation mÃ  Ä‘á»ƒ CSDL SQLite vÄƒng ngoáº¡i lá»‡ `SQLITE_CONSTRAINT: UNIQUE constraint failed: coupons.code`, tráº£ vá» HTTP 500 Internal Server Error lÃ m treo luá»“ng xá»­ lÃ½. | **FAIL** | `EXE-07_Bug_Duplicate_Code_500_Error.png` | **BUG-FR17-02** |
| **EXE-08** | Má»©c giáº£m báº±ng 0 (BiÃªn Min - 1 cá»§a `discount_value`) | `code: "ZEROOFF"`, `type: "fixed"`, `discount_value: 0`, `min_order: 100000` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request), yÃªu cáº§u má»©c giáº£m pháº£i lÃ  sá»‘ dÆ°Æ¡ng $> 0$. | Backend khÃ´ng kiá»ƒm tra rÃ ng buá»™c sá»‘ dÆ°Æ¡ng cá»§a trÆ°á»ng `discount_value`, cho phÃ©p táº¡o thÃ nh cÃ´ng mÃ£ giáº£m giÃ¡ cÃ³ má»©c giáº£m báº±ng 0 (HTTP 200 OK). | **FAIL** | `EXE-08_Bug_BVA_DiscountValue_Zero_Accepted.png` | **BUG-FR17-03** |
| **EXE-09** | Má»©c giáº£m lÃ  sá»‘ Ã¢m (`discount_value = -50000`) | `code: "NEGOFF"`, `type: "fixed"`, `discount_value: -50000`, `min_order: 100000` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request), cáº¥m giÃ¡ trá»‹ Ã¢m. | TÆ°Æ¡ng tá»±, Backend cho phÃ©p táº¡o mÃ£ giáº£m giÃ¡ cÃ³ `discount_value` Ã¢m (-50.000 â‚«) vá»›i HTTP 200 OK. | **FAIL** | `EXE-09_Bug_Negative_DiscountValue_Accepted.png` | **BUG-FR17-03** |
| **EXE-10** | Giáº£m giÃ¡ pháº§n trÄƒm vÆ°á»£t quÃ¡ 100% (BiÃªn Max + 1: `discount_value = 150`) | `code: "OVER100"`, `type: "percent"`, `discount_value: 150`, `min_order: 100000` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request), thÃ´ng bÃ¡o pháº§n trÄƒm chá»‰ tá»« 1% Ä‘áº¿n 100%. | Äá»‘i vá»›i loáº¡i giáº£m giÃ¡ percent, Backend cho phÃ©p nháº­p giÃ¡ trá»‹ 150 (> 100%), vi pháº¡m biÃªn Max [1..100], tráº£ vá» HTTP 200 OK. | **FAIL** | `EXE-10_Bug_BVA_Percent_Over100_Accepted.png` | **BUG-FR17-04** |
| **EXE-11** | GiÃ¡ trá»‹ Ä‘Æ¡n hÃ ng tá»‘i thiá»ƒu lÃ  sá»‘ Ã¢m (`min_order_amount = -10000`) | `code: "NEGMIN"`, `type: "fixed"`, `discount_value: 20000`, `min_order: -10000` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request), yÃªu cáº§u Ä‘Æ¡n tá»‘i thiá»ƒu $\ge 0$. | Backend cho phÃ©p táº¡o mÃ£ vá»›i `min_order_amount` Ã¢m (-10.000 â‚«) vá»›i HTTP 200 OK. | **FAIL** | `EXE-11_Bug_BVA_MinOrderAmount_Negative_Accepted.png` | **BUG-FR17-03** |
| **EXE-12** | LÆ°á»£t dÃ¹ng tá»‘i Ä‘a báº±ng 0 (BiÃªn Min - 1 cá»§a `max_uses_per_user`) | `code: "ZEROUSES"`, `type: "fixed"`, `discount_value: 30000`, `max_uses: 0` | Há»‡ thá»‘ng tá»« chá»‘i (HTTP 400 Bad Request), yÃªu cáº§u sá»‘ lÆ°á»£t dÃ¹ng $\ge 1$. | Backend cho phÃ©p táº¡o mÃ£ vá»›i `max_uses_per_user = 0` (ngoÃ i biÃªn há»£p lá»‡ [1..n]), tuy nhiÃªn khi ngÆ°á»i dÃ¹ng F5 táº£i láº¡i trang Web Admin thÃ¬ giao diá»‡n láº¡i tá»± Ä‘á»™ng hiá»ƒn thá»‹ sá»‘ 1 (báº¥t nháº¥t giá»¯a CSDL vÃ  UI hiá»ƒn thá»‹). | **FAIL** | `EXE-12_Bug_BVA_MaxUses_Zero_Accepted.png` | **BUG-FR17-03** |
| **EXE-13** | XÃ³a mÃ£ giáº£m giÃ¡ vá»›i ID khÃ´ng tá»“n táº¡i (`coupon_id = 99999`) | Gá»­i request `DELETE /api/admin/coupons/99999` | Há»‡ thá»‘ng tráº£ vá» HTTP 404 Not Found kÃ¨m thÃ´ng bÃ¡o khÃ´ng tÃ¬m tháº¥y mÃ£ giáº£m giÃ¡. | Gá»­i request DELETE tá»›i ID mÃ£ giáº£m giÃ¡ khÃ´ng tá»“n táº¡i (99999), API tráº£ vá» HTTP 200 OK kÃ¨m thÃ´ng bÃ¡o "Coupon deleted" thay vÃ¬ 404 Not Found. | **FAIL** | `EXE-13_Bug_Delete_NonExistent_ID_200OK.png` | **BUG-FR17-05** |
| **EXE-14** | Kiá»ƒm tra an toÃ n trÆ°á»›c táº¥n cÃ´ng Stored XSS trong mÃ£ Code | Táº¡o coupon vá»›i `code = "<script>alert('XSS')</script>"` | Há»‡ thá»‘ng escape an toÃ n, hiá»ƒn thá»‹ nguyÃªn chuá»—i plain text, khÃ´ng thá»±c thi mÃ£ JavaScript (SEC-04). | Há»‡ thá»‘ng an toÃ n trÆ°á»›c táº¥n cÃ´ng XSS, chuá»—i HTML/script trong mÃ£ giáº£m giÃ¡ Ä‘Æ°á»£c React frontend tá»± Ä‘á»™ng escape vÃ  hiá»ƒn thá»‹ an toÃ n dÆ°á»›i dáº¡ng plain text. | **PASS** | `EXE-14_Security_XSS_Code_Escaped.png` | â€” |

---

## 5. Tá»”NG Há»¢P CÃC Lá»–I PHÃT HIá»†N (DEFECT SUMMARY)

Tá»•ng sá»‘ test cases thá»±c thi: **14**  
* **Passed:** 5/14 (35.71%)  
* **Failed (PhÃ¡t hiá»‡n lá»—i):** 9/14 (64.29%)  

### Danh má»¥c 5 nhÃ³m lá»—i phÃ¡t hiá»‡n Ä‘Æ°á»£c:
1. **BUG-FR17-01 (Critical - Security / Broken Access Control)**: Thiáº¿u hoÃ n toÃ n cÆ¡ cháº¿ kiá»ƒm tra quyá»n Quáº£n trá»‹ viÃªn (`role === 'admin'`) táº¡i cÃ¡c endpoint `POST /api/admin/coupons` vÃ  `DELETE /api/admin/coupons/:id`. Báº¥t ká»³ ngÆ°á»i dÃ¹ng thÃ´ng thÆ°á»ng nÃ o Ä‘Ã£ Ä‘Äƒng nháº­p (`role = 'user'`) Ä‘á»u cÃ³ thá»ƒ tá»± Ã½ táº¡o mÃ£ giáº£m giÃ¡ tÃ¹y Ã½ (vÃ­ dá»¥ tá»± giáº£m 1.000.000 â‚«) hoáº·c xÃ³a mÃ£ giáº£m giÃ¡ cá»§a há»‡ thá»‘ng (vi pháº¡m nghiÃªm trá»ng FR-12 vÃ  SEC-03).
2. **BUG-FR17-02 (High - Crash / Error Handling)**: Khi táº¡o mÃ£ giáº£m giÃ¡ cÃ³ `code` trÃ¹ng láº·p vá»›i mÃ£ Ä‘Ã£ cÃ³ sáºµn, Backend khÃ´ng báº¯t lá»—i xÃ¡c thá»±c mÃ  Ä‘á»ƒ CSDL SQLite vÄƒng ngoáº¡i lá»‡ `SQLITE_CONSTRAINT: UNIQUE constraint failed: coupons.code`, lÃ m tráº£ vá» HTTP 500 Internal Server Error thay vÃ¬ HTTP 400 Bad Request hoáº·c 409 Conflict.
3. **BUG-FR17-03 (High - Data Validation & Boundary Violations)**: Backend thiáº¿u hoÃ n toÃ n táº§ng kiá»ƒm tra dá»¯ liá»‡u Ä‘áº§u vÃ o cho cÃ¡c trÆ°á»ng sá»‘: cháº¥p nháº­n `discount_value = 0` vÃ  Ã¢m (`-50000`), cháº¥p nháº­n `min_order_amount = -10000`, cháº¥p nháº­n `max_uses_per_user = 0` (Ä‘áº·c biá»‡t khi F5 Web Admin thÃ¬ giao diá»‡n láº¡i tá»± hiá»ƒn thá»‹ thÃ nh 1, gÃ¢y báº¥t nháº¥t dá»¯ liá»‡u nghiÃªm trá»ng).
4. **BUG-FR17-04 (High - Business Logic Violation)**: Äá»‘i vá»›i loáº¡i giáº£m giÃ¡ `percent`, Backend khÃ´ng kiá»ƒm tra giá»›i háº¡n tráº§n 100%, cho phÃ©p táº¡o mÃ£ giáº£m giÃ¡ cÃ³ tá»· lá»‡ 150% (lá»›n hÆ¡n cáº£ giÃ¡ trá»‹ Ä‘Æ¡n hÃ ng), vi pháº¡m logic nghiá»‡p vá»¥ thÆ°Æ¡ng máº¡i Ä‘iá»‡n tá»­.
5. **BUG-FR17-05 (Low/Medium - RESTful API Standards)**: Khi gá»­i request xÃ³a mÃ£ giáº£m giÃ¡ vá»›i `id` khÃ´ng tá»“n táº¡i (`99999`), Backend khÃ´ng kiá»ƒm tra `this.changes === 0` mÃ  váº«n tráº£ vá» HTTP 200 OK "Coupon deleted", vi pháº¡m chuáº©n thiáº¿t káº¿ RESTful API (láº½ ra pháº£i tráº£ vá» HTTP 404 Not Found).

