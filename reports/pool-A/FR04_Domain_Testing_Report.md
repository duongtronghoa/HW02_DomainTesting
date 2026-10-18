# BÃO CÃO KIá»‚M THá»¬ MIá»€N GIÃ TRá»Š VÃ€ PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN
## TÃNH NÄ‚NG FR-04: QUáº¢N LÃ Há»’ SÆ  CÃ NHÃ‚N (POOL A)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform (`http://localhost:5173` / Backend API `http://localhost:3000`)
* **Ká»¹ thuáº­t Ã¡p dá»¥ng:** Domain Testing (Equivalence Partitioning - EP) & Boundary Value Analysis (3-Value BVA theo chuáº©n ISTQB)
* **Thá»i gian thá»±c hiá»‡n:** 2026-10-18

---

## 1. Äáº¶C Táº¢ YÃŠU Cáº¦U & PHÃ‚N TÃCH BIáº¾N Äáº¦U VÃ€O (SPECIFICATION ANALYSIS)

### 1.1 TrÃ­ch xuáº¥t Ä‘áº·c táº£ FR-04 tá»« System Requirements Specification
> * NgÆ°á»i dÃ¹ng Ä‘Ã£ Ä‘Äƒng nháº­p cÃ³ thá»ƒ cáº­p nháº­t: **Há» TÃªn**, **Sá»‘ Ä‘iá»‡n thoáº¡i**, **Äá»‹a chá»‰ giao hÃ ng máº·c Ä‘á»‹nh**.
> * **Sá»‘ Ä‘iá»‡n thoáº¡i há»£p lá»‡**: báº¯t Ä‘áº§u báº±ng sá»‘ `0`, tá»« 10â€“11 chá»¯ sá»‘.
> * Email khÃ´ng Ä‘Æ°á»£c phÃ©p thay Ä‘á»•i qua giao diá»‡n.
> * NgÆ°á»i dÃ¹ng chá»‰ cÃ³ thá»ƒ cáº­p nháº­t há»“ sÆ¡ cá»§a chÃ­nh mÃ¬nh; khÃ´ng thá»ƒ tá»± thay Ä‘á»•i thuá»™c tÃ­nh `role`.
> * *RÃ ng buá»™c giao diá»‡n chung (FR-21, FR-22)*: Má»—i trang chá»‰ cÃ³ 1 tháº» `<h1>`, thÃ´ng bÃ¡o lá»—i hiá»ƒn thá»‹ á»Ÿ phÃ­a trÃªn nÃºt Submit, cÃ¡c trÆ°á»ng báº¯t buá»™c cÃ³ dáº¥u `*`.

### 1.2 Äiá»u kiá»‡n tiÃªn quyáº¿t (Preconditions)
* NgÆ°á»i dÃ¹ng Ä‘Ã£ Ä‘Äƒng nháº­p thÃ nh cÃ´ng vÃ o há»‡ thá»‘ng EShop vá»›i tÃ i khoáº£n há»£p lá»‡: `test@eshop.com` / `Test1234!` (`id = 2`, `role = 'user'`).
* Token xÃ¡c thá»±c JWT Ä‘Æ°á»£c Ä‘Ã­nh kÃ¨m há»£p lá»‡ trong Header `Authorization: Bearer <token>`.

### 1.3 Danh sÃ¡ch biáº¿n Ä‘áº§u vÃ o vÃ  rÃ ng buá»™c

| TÃªn biáº¿n Ä‘áº§u vÃ o | Kiá»ƒu dá»¯ liá»‡u | Miá»n giÃ¡ trá»‹ há»£p lá»‡ (Valid Domain) | RÃ ng buá»™c ká»¹ thuáº­t & Nghiá»‡p vá»¥ (Constraints) |
| :--- | :--- | :--- | :--- |
| **`Há» TÃªn` (name)** | String | Chuá»—i kÃ½ tá»± khÃ¡c rá»—ng, Ä‘á»™ dÃ i tá»« $1$ Ä‘áº¿n $255$ kÃ½ tá»±. | Báº¯t buá»™c (cÃ³ dáº¥u `*`). KhÃ´ng Ä‘Æ°á»£c Ä‘á»ƒ trá»‘ng. |
| **`Sá»‘ Ä‘iá»‡n thoáº¡i` (phone)** | String (Numeric) | Chuá»—i sá»‘ báº¯t Ä‘áº§u báº±ng kÃ½ tá»± `'0'`, Ä‘á»™ dÃ i tá»« $10$ Ä‘áº¿n $11$ chá»¯ sá»‘. | Báº¯t buá»™c (`*`). Äá»‹nh dáº¡ng chuáº©n: `^0\d{9,10}$`. KhÃ´ng chá»©a chá»¯ cÃ¡i, khoáº£ng tráº¯ng hay kÃ½ tá»± Ä‘áº·c biá»‡t. |
| **`Äá»‹a chá»‰ giao hÃ ng` (shipping_address)** | String | Chuá»—i kÃ½ tá»± mÃ´ táº£ Ä‘á»‹a chá»‰, Ä‘á»™ dÃ i tá»« $1$ Ä‘áº¿n $255$ kÃ½ tá»±. | Báº¯t buá»™c (`*`). KhÃ´ng Ä‘Æ°á»£c Ä‘á»ƒ trá»‘ng. |
| **`Email`** *(Biáº¿n kiá»ƒm thá»­ Ã¢m)* | Email String | KhÃ´ng cÃ³ (Read-only trÃªn UI). | **Cáº¥m thay Ä‘á»•i**: UI khÃ³a chá»‰nh sá»­a. API Backend pháº£i bá» qua hoáº·c tá»« chá»‘i náº¿u Client cá»‘ tÃ¬nh gá»­i kÃ¨m email má»›i. |
| **`role`** *(Biáº¿n kiá»ƒm thá»­ báº£o máº­t)* | String | KhÃ´ng cÃ³ (KhÃ´ng nháº­n tá»« Client). | **RÃ ng buá»™c an ninh (SEC-06)**: Client cáº¥m tá»± Ä‘á»•i quyá»n thÃ nh `'admin'`. Backend báº¯t buá»™c pháº£i cháº·n hoáº·c phá»›t lá». |
| **`id` / `user_id`** *(Biáº¿n kiá»ƒm thá»­ IDOR)* | Integer | `id = 2` (TrÃ¹ng ID giáº£i mÃ£ tá»« Token). | NgÆ°á»i dÃ¹ng chá»‰ cÃ³ quyá»n cáº­p nháº­t há»“ sÆ¡ cá»§a chÃ­nh mÃ¬nh, cáº¥m sá»­a sang ID cá»§a ngÆ°á»i khÃ¡c (SEC-02). |

---

## 2. PHÃ‚N VÃ™NG TÆ¯Æ NG ÄÆ¯Æ NG (EQUIVALENCE PARTITIONING - EP)

Dá»±a trÃªn cÃ¡c biáº¿n Ä‘áº§u vÃ o, phÃ¢n chia miá»n giÃ¡ trá»‹ thÃ nh cÃ¡c lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng há»£p lá»‡ (Valid) vÃ  khÃ´ng há»£p lá»‡ (Invalid):

| Biáº¿n | Lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng | Loáº¡i | GiÃ¡ trá»‹ Ä‘áº¡i diá»‡n | MÃ´ táº£ kiá»ƒm thá»­ |
| :--- | :--- | :---: | :--- | :--- |
| **`Há» TÃªn`** | **EC1.1** | Valid | `"Nguyá»…n VÄƒn A"` | Chuá»—i há» tÃªn thÃ´ng thÆ°á»ng há»£p lá»‡ (1â€“255 kÃ½ tá»±). |
| | **EC1.2** | Invalid | `""` (Rá»—ng) | Bá» trá»‘ng Há» TÃªn (0 kÃ½ tá»±). |
| | **EC1.3** | Invalid | `"A".repeat(256)` | Chuá»—i vÆ°á»£t quÃ¡ 255 kÃ½ tá»± (256 kÃ½ tá»±). |
| **`Äá»‹a chá»‰ giao hÃ ng`** | **EC2.1** | Valid | `"123 LÃª Lá»£i, Quáº­n 1, TP.HCM"` | Chuá»—i Ä‘á»‹a chá»‰ há»£p lá»‡ (1â€“255 kÃ½ tá»±). |
| | **EC2.2** | Invalid | `""` (Rá»—ng) | Bá» trá»‘ng Äá»‹a chá»‰ giao hÃ ng (0 kÃ½ tá»±). |
| | **EC2.3** | Invalid | `"B".repeat(256)` | Chuá»—i Ä‘á»‹a chá»‰ vÆ°á»£t quÃ¡ 255 kÃ½ tá»±. |
| **`Sá»‘ Ä‘iá»‡n thoáº¡i`** | **EC3.1** | Valid | `"0912345678"` | Báº¯t Ä‘áº§u báº±ng sá»‘ `0`, Ä‘á»™ dÃ i Ä‘Ãºng **10 chá»¯ sá»‘**. |
| | **EC3.2** | Valid | `"01234567890"` | Báº¯t Ä‘áº§u báº±ng sá»‘ `0`, Ä‘á»™ dÃ i Ä‘Ãºng **11 chá»¯ sá»‘**. |
| | **EC3.3** | Invalid | `""` (Rá»—ng) | Bá» trá»‘ng Sá»‘ Ä‘iá»‡n thoáº¡i. |
| | **EC3.4** | Invalid | `"091234567"` | Báº¯t Ä‘áº§u báº±ng `0`, nhÆ°ng Ä‘á»™ dÃ i **< 10 chá»¯ sá»‘** (9 sá»‘). |
| | **EC3.5** | Invalid | `"012345678901"` | Báº¯t Ä‘áº§u báº±ng `0`, nhÆ°ng Ä‘á»™ dÃ i **> 11 chá»¯ sá»‘** (12 sá»‘). |
| | **EC3.6** | Invalid | `"1912345678"` | Äá»§ 10 sá»‘ nhÆ°ng **khÃ´ng báº¯t Ä‘áº§u báº±ng sá»‘ `0`** (báº¯t Ä‘áº§u báº±ng 1-9). |
| | **EC3.7** | Invalid | `"091234567a"` | Chá»©a kÃ½ tá»± chá»¯ cÃ¡i hoáº·c kÃ½ tá»± Ä‘áº·c biá»‡t. |
| **`Email`** | **EC4.1** | Valid | *(Read-only / Giá»¯ nguyÃªn)* | Email khÃ´ng bá»‹ can thiá»‡p trÃªn UI. |
| | **EC4.2** | Invalid | `"hacker_moi@eshop.com"` | Cá»‘ tÃ¬nh truyá»n `email` má»›i trong body request API. |
| **`role`** | **EC5.1** | Valid | *(KhÃ´ng gá»­i trong body)* | KhÃ´ng truyá»n thuá»™c tÃ­nh `role`. |
| | **EC5.2** | Invalid | `"role": "admin"` | Cá»‘ tÃ¬nh truyá»n `role: "admin"` Ä‘á»ƒ nÃ¢ng quyá»n (SEC-06). |
| **`Target User ID`** | **EC6.1** | Valid | `id = 2` | Thao tÃ¡c trÃªn Ä‘Ãºng há»“ sÆ¡ cá»§a mÃ¬nh. |
| | **EC6.2** | Invalid | `id = 1` | Cá»‘ tÃ¬nh gá»­i request sá»­a há»“ sÆ¡ cá»§a user khÃ¡c (IDOR). |

---

## 3. PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN (BOUNDARY VALUE ANALYSIS - 3-VALUE BVA)

Ãp dá»¥ng ká»¹ thuáº­t 3-Value BVA theo chuáº©n ISTQB ($Min-1, Min, Min+1, Nominal, Max-1, Max, Max+1$):

### 3.1 Báº£ng phÃ¢n tÃ­ch giÃ¡ trá»‹ biÃªn 3 giÃ¡ trá»‹

| Biáº¿n kiá»ƒm thá»­ | Ranh giá»›i (Boundary) | Min - 1 | Min | Min + 1 | Nominal | Max - 1 | Max | Max + 1 |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`Sá»‘ Ä‘iá»‡n thoáº¡i`** *(Äá»™ dÃ i sá»‘)* | $[10, 11]$ chá»¯ sá»‘ | `9 sá»‘` *(Invalid)* | `10 sá»‘` *(Valid)* | `11 sá»‘` *(Valid - Max)* | `10 sá»‘` | `10 sá»‘` *(Min)* | `11 sá»‘` *(Valid)* | `12 sá»‘` *(Invalid)* |
| **`Sá»‘ Ä‘iá»‡n thoáº¡i`** *(KÃ½ tá»± Ä‘áº§u - ASCII)* | Báº¯t Ä‘áº§u báº±ng `'0'` (ASCII 48) | ASCII 47 `'/'` *(Invalid)* | ASCII 48 `'0'` *(Valid)* | ASCII 49 `'1'` *(Invalid)* | `'0'` | *(BiÃªn Ä‘Æ¡n Ä‘iá»ƒm)* | ASCII 48 `'0'` | ASCII 49 `'1'` |
| **`Há» TÃªn`** *(Äá»™ dÃ i chuá»—i)* | $[1, 255]$ kÃ½ tá»± | `0 kÃ½ tá»±` *(Invalid)* | `1 kÃ½ tá»±` *(Valid)* | `2 kÃ½ tá»±` *(Valid)* | `12 kÃ½ tá»±` | `254 kÃ½ tá»±` *(Valid)* | `255 kÃ½ tá»±` *(Valid)* | `256 kÃ½ tá»±` *(Invalid)* |
| **`Äá»‹a chá»‰`** *(Äá»™ dÃ i chuá»—i)* | $[1, 255]$ kÃ½ tá»± | `0 kÃ½ tá»±` *(Invalid)* | `1 kÃ½ tá»±` *(Valid)* | `2 kÃ½ tá»±` *(Valid)* | `25 kÃ½ tá»±` | `254 kÃ½ tá»±` *(Valid)* | `255 kÃ½ tá»±` *(Valid)* | `256 kÃ½ tá»±` *(Invalid)* |

---

## 4. Káº¾T QUáº¢ THá»°C THI KIá»‚M THá»¬ THá»°C Táº¾ (TEST EXECUTION RESULTS)

Táº¥t cáº£ 16 ká»‹ch báº£n kiá»ƒm thá»­ Ä‘Ã£ Ä‘Æ°á»£c thá»±c thi trá»±c tiáº¿p trÃªn há»‡ thá»‘ng EShop thá»±c táº¿ (Frontend `http://localhost:5173`, Backend `http://localhost:3000`, CSDL `database.sqlite`). HÃ¬nh áº£nh báº±ng chá»©ng Ä‘Ã£ Ä‘Æ°á»£c lÆ°u trá»¯ Ä‘áº§y Ä‘á»§ táº¡i thÆ° má»¥c `screenshots/pool A/`.

| MÃ£ TC | Ká»‹ch báº£n kiá»ƒm thá»­ | Dá»¯ liá»‡u Ä‘áº§u vÃ o (Input) | Káº¿t quáº£ mong Ä‘á»£i (Expected Result) | Káº¿t quáº£ thá»±c táº¿ (Actual Result) | Tráº¡ng thÃ¡i | Báº±ng chá»©ng (Screenshot) | MÃ£ lá»—i (Bug ID) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| **EXE-01** | Kiá»ƒm tra GUI, tháº» `<h1>` & KhÃ³a trÆ°á»ng Email | Quan sÃ¡t giao diá»‡n; Console Ä‘áº¿m `<h1>` | ÄÃºng 1 tháº» `<h1>`; trÆ°á»ng Email disabled; nhÃ£n cÃ³ `*` Ä‘á»; nÃºt LÆ°u mÃ u xanh. | ÄÃºng 1 tháº» h1. TrÆ°á»ng Email cÃ³ thuá»™c tÃ­nh disabled, khÃ´ng cho phÃ©p nháº­p liá»‡u. NÃºt LÆ°u mÃ u xanh. | **PASS** | `EXE-01_GUI_Email_Check.png` | â€” |
| **EXE-02** | Cáº­p nháº­t há»£p lá»‡ â€” SÄT 10 sá»‘ (BiÃªn Min) | Há» tÃªn: `"Nguyá»…n VÄƒn A"`<br>SÄT: `"0912345678"` (10 sá»‘)<br>Ä/c: `"123 LÃª Lá»£i"` | Cáº­p nháº­t thÃ nh cÃ´ng (HTTP 200). LÆ°u SÄT 10 sá»‘ vÃ o CSDL. | Há»‡ thá»‘ng tá»« chá»‘i vÃ  hiá»ƒn thá»‹ thÃ´ng bÃ¡o lá»—i: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p 9-10 chá»¯ sá»‘"*. | **FAIL** | `EXE-02_Valid_10Digits_Phone.png` | **BUG-FR04-01** |
| **EXE-03** | Cáº­p nháº­t há»£p lá»‡ â€” SÄT 11 sá»‘ (BiÃªn Max) | Há» tÃªn: `"Tráº§n Thá»‹ B"`<br>SÄT: `"01234567890"` (11 sá»‘)<br>Ä/c: `"456 Nguyá»…n Huá»‡"` | Cáº­p nháº­t thÃ nh cÃ´ng (HTTP 200). LÆ°u SÄT 11 sá»‘ vÃ o CSDL. | Há»‡ thá»‘ng tá»« chá»‘i vÃ  hiá»ƒn thá»‹ thÃ´ng bÃ¡o lá»—i: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p 9-10 chá»¯ sá»‘"*. | **FAIL** | `EXE-03_Valid_11Digits_Phone.png` | **BUG-FR04-01** |
| **EXE-04** | BiÃªn dÆ°á»›i Há» tÃªn & Äá»‹a chá»‰ (Min = 1 kÃ½ tá»±) | Há» tÃªn: `"A"` (1 char)<br>SÄT: `"0912345678"`<br>Ä/c: `"1"` (1 char) | Cáº­p nháº­t thÃ nh cÃ´ng (HTTP 200). | Há»‡ thá»‘ng cháº¥p nháº­n Ä‘á»™ dÃ i tá»‘i thiá»ƒu 1 kÃ½ tá»± cho Há» TÃªn vÃ  Äá»‹a chá»‰. LÆ°u thÃ nh cÃ´ng. | **PASS** | `EXE-04_BVA_Min_1Char.png` | â€” |
| **EXE-05** | BiÃªn trÃªn Há» tÃªn & Äá»‹a chá»‰ (Max = 255 kÃ½ tá»±) | Há» tÃªn: `"A".repeat(255)`<br>Ä/c: `"B".repeat(255)` | LÆ°u thÃ nh cÃ´ng chuá»—i 255 kÃ½ tá»±. Giao diá»‡n an toÃ n. | Há»‡ thá»‘ng lÆ°u thÃ nh cÃ´ng chuá»—i 255 kÃ½ tá»± cho Há» TÃªn vÃ  Äá»‹a chá»‰. Giao diá»‡n hiá»ƒn thá»‹ an toÃ n, khÃ´ng bá»‹ trÃ n khung. | **PASS** | `EXE-05_BVA_Max_255Chars.png` | â€” |
| **EXE-06** | Äá»ƒ trá»‘ng Há» tÃªn (Min - 1 = 0 kÃ½ tá»±) | Há» tÃªn: `""`<br>SÄT: `"0912345678"`<br>Ä/c: `"123 LÃª Lá»£i"` | Tá»« chá»‘i cáº­p nháº­t. BÃ¡o lá»—i phÃ­a TRÃŠN nÃºt báº¥m (FR-22). | Há»‡ thá»‘ng cháº·n cáº­p nháº­t khi Ä‘á»ƒ trá»‘ng Há» tÃªn. ThÃ´ng bÃ¡o lá»—i hiá»ƒn thá»‹ báº±ng tiáº¿ng Viá»‡t rÃµ rÃ ng á»Ÿ phÃ­a trÃªn nÃºt báº¥m theo Ä‘Ãºng FR-22. | **PASS** | `EXE-06_Error_Empty_Name.png` | â€” |
| **EXE-07** | Äá»ƒ trá»‘ng Äá»‹a chá»‰ (Min - 1 = 0 kÃ½ tá»±) | Há» tÃªn: `"Nguyá»…n VÄƒn A"`<br>SÄT: `"0912345678"`<br>Ä/c: `""` | Há»‡ thá»‘ng tá»« chá»‘i vÃ  yÃªu cáº§u nháº­p Ä‘á»‹a chá»‰ báº¯t buá»™c. | Há»‡ thá»‘ng cho phÃ©p lÆ°u Ä‘á»‹a chá»‰ rá»—ng, hiá»ƒn thá»‹ thÃ´ng bÃ¡o cáº­p nháº­t thÃ nh cÃ´ng vÃ  ghi chuá»—i rá»—ng vÃ o CSDL. | **FAIL** | `EXE-07_Error_Empty_Address.png` | **BUG-FR04-02** |
| **EXE-08** | Há» tÃªn & Äá»‹a chá»‰ vÆ°á»£t biÃªn trÃªn (Max + 1 = 256 kÃ½ tá»±) | Há» tÃªn: `"A".repeat(256)`<br>Ä/c: `"B".repeat(256)` | Cháº·n khÃ´ng cho nháº­p/lÆ°u quÃ¡ 255 kÃ½ tá»±. | Input cho phÃ©p nháº­p quÃ¡ 255 kÃ½ tá»±. Há»‡ thá»‘ng gá»­i request vÃ  tráº£ vá» cáº­p nháº­t thÃ nh cÃ´ng, khÃ´ng cháº·n giá»›i háº¡n Ä‘á»™ dÃ i. | **FAIL** | `EXE-08_BVA_MaxPlus1_256Chars.png` | **BUG-FR04-03** |
| **EXE-09** | Bá» trá»‘ng Sá»‘ Ä‘iá»‡n thoáº¡i | SÄT: `""` (Rá»—ng) | Cháº·n vÃ  hiá»ƒn thá»‹ lá»—i SÄT báº¯t buá»™c. | Há»‡ thá»‘ng hiá»ƒn thá»‹: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p Ä‘Ãºng 9-10 chá»¯ sá»‘"*. Cháº·n thÃ nh cÃ´ng. | **PASS** | `EXE-09_Error_Empty_Phone.png` | â€” |
| **EXE-10** | SÄT ngáº¯n hÆ¡n quy Ä‘á»‹nh (Min - 1 = 9 chá»¯ sá»‘) | SÄT: `"091234567"` (9 sá»‘) | Tá»« chá»‘i cáº­p nháº­t vÃ¬ SÄT pháº£i tá»« 10â€“11 sá»‘. | Há»‡ thá»‘ng tá»« chá»‘i cáº­p nháº­t, hiá»ƒn thá»‹: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p Ä‘Ãºng 9-10 chá»¯ sá»‘"*. | **PASS** | `EXE-10_BVA_Phone_9Digits_Fail.png` | â€” |
| **EXE-11** | SÄT dÃ i hÆ¡n quy Ä‘á»‹nh (Max + 1 = 12 chá»¯ sá»‘) | SÄT: `"012345678901"` (12 sá»‘) | Tá»« chá»‘i cáº­p nháº­t vÃ¬ vÆ°á»£t quÃ¡ 11 sá»‘. | Há»‡ thá»‘ng tá»« chá»‘i cáº­p nháº­t, hiá»ƒn thá»‹: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p Ä‘Ãºng 9-10 chá»¯ sá»‘"*. | **PASS** | `EXE-11_BVA_Phone_12Digits_Fail.png` | â€” |
| **EXE-12** | SÄT khÃ´ng báº¯t Ä‘áº§u báº±ng sá»‘ 0 (BiÃªn ASCII Min + 1) | SÄT: `"1912345678"` (10 sá»‘, báº¯t Ä‘áº§u báº±ng `'1'`) | Tá»« chá»‘i cáº­p nháº­t vÃ¬ vi pháº¡m quy táº¯c báº¯t Ä‘áº§u báº±ng sá»‘ `0`. | Há»‡ thá»‘ng cháº¥p nháº­n sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng báº¯t Ä‘áº§u báº±ng sá»‘ khÃ´ng. Há»‡ thá»‘ng hiá»ƒn thá»‹ Cáº­p nháº­t thÃ nh cÃ´ng. | **FAIL** | `EXE-12_BVA_Phone_NotStartWithZero.png` | **BUG-FR04-04** |
| **EXE-13** | SÄT chá»©a chá»¯ cÃ¡i / kÃ½ tá»± Ä‘áº·c biá»‡t | SÄT: `"091234567a"` | Tá»« chá»‘i cáº­p nháº­t vÃ¬ chá»©a kÃ½ tá»± khÃ´ng pháº£i chá»¯ sá»‘. | Há»‡ thá»‘ng cháº·n cáº­p nháº­t, hiá»ƒn thá»‹: *"Sá»‘ Ä‘iá»‡n thoáº¡i khÃ´ng há»£p lá»‡. Vui lÃ²ng nháº­p Ä‘Ãºng 9-10 chá»¯ sá»‘"*. | **PASS** | `EXE-13_Error_Phone_With_Letters.png` | â€” |
| **EXE-14** | Cá»‘ tÃ¬nh can thiá»‡p thay Ä‘á»•i Email qua API payload | Body gá»­i: `"email": "hacker_moi@eshop.com"` | Backend bá» qua hoáº·c tá»« chá»‘i, CSDL giá»¯ nguyÃªn email gá»‘c. | Backend bá» qua trÆ°á»ng email trong payload, CSDL SQLite váº«n giá»¯ nguyÃªn email gá»‘c 'test@eshop.com'. Äáº¡t chuáº©n an toÃ n FR-04. | **PASS** | `EXE-14_Security_Tamper_Email.png` | â€” |
| **EXE-15** | Cá»‘ tÃ¬nh nÃ¢ng quyá»n `role = "admin"` qua API | Body gá»­i: `"role": "admin"` | Backend cáº¥m sá»­a `role` (HTTP 400/403). CSDL giá»¯ `role = 'user'`. | Backend cáº­p nháº­t trá»±c tiáº¿p `role = 'admin'` vÃ o SQLite. DÃ¹ng tÃ i khoáº£n `test@eshop.com` Ä‘Äƒng nháº­p Ä‘Æ°á»£c luÃ´n vÃ o Web Admin `http://localhost:5174/`! | **FAIL** | `EXE-15_Bug_Privilege_Escalation.png` | **BUG-FR04-05** |
| **EXE-16** | Cá»‘ tÃ¬nh sá»­a há»“ sÆ¡ tÃ i khoáº£n Admin (IDOR) | Body gá»­i: `id = 1, name = "Hacker"` | Tá»« chá»‘i cáº­p nháº­t hoáº·c chá»‰ sá»­a há»“ sÆ¡ chÃ­nh mÃ¬nh. Admin giá»¯ nguyÃªn. | Há»‡ thá»‘ng chá»‰ cáº­p nháº­t theo ID trong JWT Token (`id = 2`). Há»“ sÆ¡ cá»§a Admin (`id = 1`) Ä‘Æ°á»£c báº£o vá»‡ an toÃ n. | **PASS** | `EXE-16_Security_IDOR_Check.png` | â€” |

---

## 5. Tá»”NG Há»¢P CÃC Lá»–I PHÃT HIá»†N (DEFECT SUMMARY)

Tá»•ng sá»‘ test cases thá»±c thi: **16**  
* **Passed:** 10/16 (62.50%)  
* **Failed (PhÃ¡t hiá»‡n lá»—i):** 6/16 (37.50%)  
*(Ghi chÃº: CÃ³ 6 ca kiá»ƒm thá»­ tháº¥t báº¡i do EXE-02 vÃ  EXE-03 Ä‘á»u phÃ¡t hiá»‡n lá»—i BUG-FR04-01).*  

### Danh má»¥c 5 lá»—i phÃ¡t hiá»‡n Ä‘Æ°á»£c:
1. **BUG-FR04-01 (High - Validation Logic Sai Lá»‡ch)**: Há»‡ thá»‘ng cháº·n cÃ¡c sá»‘ Ä‘iá»‡n thoáº¡i 10 sá»‘ vÃ  11 sá»‘ há»£p lá»‡ theo Ä‘áº·c táº£, Ä‘Æ°a ra thÃ´ng bÃ¡o sai lá»‡ch yÃªu cáº§u "9-10 chá»¯ sá»‘".
2. **BUG-FR04-02 (Medium - Data Integrity)**: Há»‡ thá»‘ng cho phÃ©p cáº­p nháº­t vÃ  lÆ°u Äá»‹a chá»‰ giao hÃ ng rá»—ng vÃ o CSDL.
3. **BUG-FR04-03 (Medium - Boundary Validation)**: KhÃ´ng kiá»ƒm soÃ¡t giá»›i háº¡n Ä‘á»™ dÃ i tá»‘i Ä‘a 255 kÃ½ tá»± trÃªn giao diá»‡n vÃ  backend, cho phÃ©p lÆ°u chuá»—i 256 kÃ½ tá»±.
4. **BUG-FR04-04 (High - Boundary Validation)**: Há»‡ thá»‘ng bá» qua rÃ ng buá»™c báº¯t Ä‘áº§u báº±ng sá»‘ `0`, cháº¥p nháº­n lÆ°u cÃ¡c sá»‘ Ä‘iá»‡n thoáº¡i báº¯t Ä‘áº§u báº±ng chá»¯ sá»‘ khÃ¡c (`1-9`).
5. **BUG-FR04-05 (Critical - Security / Privilege Escalation)**: Lá»— há»•ng Mass Assignment cho phÃ©p ngÆ°á»i dÃ¹ng tá»± thÄƒng quyá»n thÃ nh Admin thÃ´ng qua payload API `PUT /api/users/me`, vi pháº¡m nghiÃªm trá»ng Ä‘áº·c táº£ FR-04 vÃ  chuáº©n an ninh SEC-06.

