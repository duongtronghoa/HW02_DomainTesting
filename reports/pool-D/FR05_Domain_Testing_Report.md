# BÃO CÃO KIá»‚M THá»¬ MIá»€N GIÃ TRá»Š VÃ€ PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN
## TÃNH NÄ‚NG FR-05: XEM DANH SÃCH & TÃŒM KIáº¾M Sáº¢N PHáº¨M (MOBILE - POOL D)

* **MÃ£ sinh viÃªn:** 23120127
* **Há» vÃ  tÃªn:** DÆ°Æ¡ng Trá»ng HÃ²a
* **Há»‡ thá»‘ng Ä‘Æ°á»£c kiá»ƒm thá»­ (SUT):** EShop Platform â€” PhÃ¢n há»‡ Mobile (`frontend-mobile` Expo React Native / Backend API `http://localhost:3000`)
* **Ká»¹ thuáº­t Ã¡p dá»¥ng:** Domain Testing (Equivalence Partitioning - EP) & Boundary Value Analysis (3-Value BVA theo chuáº©n ISTQB)
* **Thá»i gian thá»±c hiá»‡n:** 2026-10-18

---

## 1. Äáº¶C Táº¢ YÃŠU Cáº¦U & PHÃ‚N TÃCH BIáº¾N Äáº¦U VÃ€O (SPECIFICATION ANALYSIS)

### 1.1 TrÃ­ch xuáº¥t Ä‘áº·c táº£ FR-05 tá»« System Requirements Specification
> * **FR-05: Xem danh sÃ¡ch & TÃ¬m kiáº¿m sáº£n pháº©m (Mobile)**
>   * MÃ n hÃ¬nh chÃ­nh hiá»ƒn thá»‹ danh sÃ¡ch táº¥t cáº£ sáº£n pháº©m dáº¡ng danh sÃ¡ch/lÆ°á»›i (grid/cards).
>   * Má»—i sáº£n pháº©m hiá»ƒn thá»‹: **áº¢nh** (tá»· lá»‡ chuáº©n, cÃ³ alt text mÃ´ táº£), **TÃªn sáº£n pháº©m**, **GiÃ¡** (Ä‘Æ¡n vá»‹: â‚«, Ä‘á»‹nh dáº¡ng phÃ¢n cÃ¡ch hÃ ng nghÃ¬n).
>   * **Thanh tÃ¬m kiáº¿m:** TÃ¬m theo tÃªn sáº£n pháº©m. Tá»« khÃ³a tÃ¬m kiáº¿m pháº£i Ä‘Æ°á»£c **hiá»ƒn thá»‹ an toÃ n** (khÃ´ng render HTML).
>   * Khi Ä‘ang táº£i dá»¯ liá»‡u pháº£i hiá»ƒn thá»‹ tráº¡ng thÃ¡i **loading**.
>   * Khi khÃ´ng cÃ³ káº¿t quáº£ tÃ¬m kiáº¿m pháº£i hiá»ƒn thá»‹ thÃ´ng bÃ¡o **empty state** phÃ¹ há»£p (kÃ¨m hÃ¬nh minh há»a vÃ  thÃ´ng bÃ¡o rÃµ rÃ ng).
>   * **RÃ ng buá»™c an ninh & pháº¡m vi (SEC-01, SEC-04):**
>     * Tá»« khÃ³a chá»‰ tÃ¬m kiáº¿m trÃªn trÆ°á»ng TÃªn sáº£n pháº©m, khÃ´ng tÃ¬m trÃªn MÃ´ táº£ hay Danh má»¥c.
>     * PhÃ²ng chá»‘ng SQL Injection: Backend báº¯t buá»™c pháº£i sá»­ dá»¥ng Parameterized Query (SEC-01).
>     * PhÃ²ng chá»‘ng Cross-Site Scripting (XSS): Chuá»—i nháº­p HTML/Script pháº£i Ä‘Æ°á»£c escape an toÃ n (SEC-04).

### 1.2 Äiá»u kiá»‡n tiÃªn quyáº¿t & Cáº¥u hÃ¬nh mÃ´i trÆ°á»ng (Preconditions & Environment Setup)
* **Backend API:** Node.js Express server cháº¡y táº¡i `http://localhost:3000`, CSDL SQLite `database.sqlite` Ä‘Ã£ Ä‘Æ°á»£c náº¡p dá»¯ liá»‡u máº«u (5 sáº£n pháº©m).
* **Ná»n táº£ng kiá»ƒm thá»­ Mobile:** Cháº¡y Expo thÃ´ng qua **Web Mobile View** (`npx expo start --web` káº¿t há»£p Chrome DevTools giáº£ láº­p kÃ­ch thÆ°á»›c Ä‘iá»‡n thoáº¡i `iPhone 12/14 Pro` / `Samsung Galaxy`) vÃ  mÃ¡y áº£o **Android Studio Emulator (Pixel 6)**.
* **Xá»­ lÃ½ sá»± cá»‘ káº¿t ná»‘i máº¡ng cá»¥c bá»™ (`API_URL`):**
  * Trong mÃ£ nguá»“n ban Ä‘áº§u cá»§a SUT táº¡i [`frontend-mobile/App.js` dÃ²ng 16](file:///c:/Users/duong/Downloads/New%20folder%20%2820%29/eshop-sut/frontend-mobile/App.js#L16), Ä‘á»‹a chá»‰ Backend bá»‹ gÃ¡n cá»©ng lÃ :
    ```javascript
    const API_URL = "http://192.168.10.13:3000/api";
    ```
  * Äá»‹a chá»‰ IP LAN `192.168.10.13` nÃ y khÃ´ng tá»“n táº¡i trong máº¡ng thá»­ nghiá»‡m, dáº«n Ä‘áº¿n viá»‡c á»©ng dá»¥ng Mobile khi khá»Ÿi cháº¡y trÃªn mÃ¡y áº£o Pixel 6 hoáº·c trÃªn trÃ¬nh duyá»‡t Web Ä‘á»u gáº·p lá»—i káº¿t ná»‘i `TypeError: Network request failed` / `Failed to fetch`, lÃ m tÃª liá»‡t toÃ n bá»™ tÃ­nh nÄƒng táº£i danh sÃ¡ch vÃ  tÃ¬m kiáº¿m sáº£n pháº©m.
  * Äá»ƒ phá»¥c vá»¥ kiá»ƒm thá»­ thá»±c táº¿, tester Ä‘Ã£ chá»§ Ä‘á»™ng can thiá»‡p ká»¹ thuáº­t vÃ o `App.js`, cáº¥u hÃ¬nh láº¡i `API_URL` linh hoáº¡t theo ná»n táº£ng thá»±c thi (`Platform.OS`):
    ```javascript
    import { Platform } from "react-native";

    const API_URL =
      Platform.OS === "android"
        ? "http://10.0.2.2:3000/api"
        : "http://localhost:3000/api";
    ```
    *(Trong Ä‘Ã³, `10.0.2.2` lÃ  Ä‘á»‹a chá»‰ loopback chuáº©n trá» vá» mÃ¡y host cho mÃ¡y áº£o Android Studio, vÃ  `localhost:3000` phá»¥c vá»¥ kiá»ƒm thá»­ trÃªn ná»n táº£ng Web).*

### 1.3 Danh sÃ¡ch biáº¿n Ä‘áº§u vÃ o vÃ  rÃ ng buá»™c ká»¹ thuáº­t

| TÃªn biáº¿n Ä‘áº§u vÃ o | Kiá»ƒu dá»¯ liá»‡u | Miá»n giÃ¡ trá»‹ há»£p lá»‡ (Valid Domain) | RÃ ng buá»™c ká»¹ thuáº­t & Nghiá»‡p vá»¥ (Constraints) |
| :--- | :--- | :--- | :--- |
| **`search_keyword` (Tá»« khÃ³a tÃ¬m kiáº¿m)** | String | Chuá»—i kÃ½ tá»± Unicode cÃ³ Ä‘á»™ dÃ i tá»« $0$ Ä‘áº¿n $255$ kÃ½ tá»±. | â€¢ **Chuá»—i rá»—ng (`""`):** Táº£i vÃ  hiá»ƒn thá»‹ toÃ n bá»™ 5 sáº£n pháº©m.<br>â€¢ **Khá»›p tÃªn:** TÃ¬m kiáº¿m khÃ´ng phÃ¢n biá»‡t hoa thÆ°á»ng.<br>â€¢ **Pháº¡m vi:** Chá»‰ tÃ¬m theo `name`, khÃ´ng tÃ¬m trong `description` hay `category`.<br>â€¢ **An toÃ n:** Escape an toÃ n chá»‘ng XSS, dÃ¹ng Parameterized Query chá»‘ng SQLi. |

---

## 2. PHÃ‚N VÃ™NG TÆ¯Æ NG ÄÆ¯Æ NG (EQUIVALENCE PARTITIONING - EP)

Dá»±a trÃªn hÃ nh vi xá»­ lÃ½ cá»§a há»‡ thá»‘ng Ä‘á»‘i vá»›i tá»« khÃ³a tÃ¬m kiáº¿m trÃªn Mobile, phÃ¢n chia miá»n giÃ¡ trá»‹ thÃ nh cÃ¡c lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng:

| Biáº¿n Ä‘áº§u vÃ o | Lá»›p tÆ°Æ¡ng Ä‘Æ°Æ¡ng | Loáº¡i | GiÃ¡ trá»‹ Ä‘áº¡i diá»‡n | Má»¥c tiÃªu kiá»ƒm thá»­ |
| :--- | :--- | :---: | :--- | :--- |
| **`search_keyword`** | **EC1** | Valid | `""` (Chuá»—i rá»—ng) | Táº£i toÃ n bá»™ danh sÃ¡ch sáº£n pháº©m máº·c Ä‘á»‹nh khi má»Ÿ app. |
| | **EC2** | Valid | `"iPhone 15 Pro Max"` | Khá»›p chÃ­nh xÃ¡c 100% tÃªn má»™t sáº£n pháº©m Ä‘ang cÃ³ trong CSDL. |
| | **EC3** | Valid | `"iPhone"` | Khá»›p má»™t pháº§n tÃªn sáº£n pháº©m (chuá»—i con - Substring). |
| | **EC4** | Valid | `"iphone"` | Khá»›p tÃªn khÃ´ng phÃ¢n biá»‡t chá»¯ hoa / chá»¯ thÆ°á»ng. |
| | **EC5** | Valid | `"  iPhone  "` | CÃ³ khoáº£ng tráº¯ng thá»«a á»Ÿ Ä‘áº§u/cuá»‘i (Kiá»ƒm tra cáº¯t trim whitespace). |
| | **EC6** | Valid (Káº¿t quáº£ rá»—ng) | `"XYZ999"` | KhÃ´ng khá»›p báº¥t ká»³ sáº£n pháº©m nÃ o (Kiá»ƒm tra hiá»ƒn thá»‹ Empty State). |
| | **EC7** | Boundary / Scope | `"Äiá»‡n thoáº¡i"` | Tá»« khÃ³a chá»‰ cÃ³ trong MÃ´ táº£ sáº£n pháº©m, khÃ´ng cÃ³ trong TÃªn (Kiá»ƒm tra pháº¡m vi). |
| | **EC8** | Security (SQLi) | `"' OR 1=1 --"` | Chuá»—i táº¥n cÃ´ng SQL Injection (Kiá»ƒm tra Parameterized Query). |
| | **EC9** | Security (Crash) | `"'"` | Dáº¥u nhÃ¡y Ä‘Æ¡n lÃ m há»ng cÃº phÃ¡p SQL (Kiá»ƒm tra Error Handling / lá»™ mÃ£ HTML). |
| | **EC10** | Security (XSS) | `"<h1><script>alert(1)</script></h1>"` | Chuá»—i chá»©a mÃ£ Ä‘á»™c HTML/Script (Kiá»ƒm tra cÆ¡ cháº¿ Escape HTML cá»§a React Native). |

---

## 3. PHÃ‚N TÃCH GIÃ TRá»Š BIÃŠN (BOUNDARY VALUE ANALYSIS - 3-VALUE BVA)

Ãp dá»¥ng ká»¹ thuáº­t 3-Value BVA theo chuáº©n ISTQB cho Ä‘á»™ dÃ i cá»§a chuá»—i tá»« khÃ³a `search_keyword` (vá»›i biÃªn dÆ°á»›i $Min = 0$ kÃ½ tá»± vÃ  biÃªn trÃªn lÃ½ thuyáº¿t theo Ä‘á»™ dÃ i tá»‘i Ä‘a tÃªn sáº£n pháº©m $Max = 255$ kÃ½ tá»±):

### 3.1 Báº£ng phÃ¢n tÃ­ch giÃ¡ trá»‹ biÃªn 3 giÃ¡ trá»‹ cho FR-05

| Biáº¿n kiá»ƒm thá»­ | Ranh giá»›i (Boundary) | Min - 1 | Min | Min + 1 | Nominal | Max - 1 | Max | Max + 1 |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`search_keyword.length`** *(Äá»™ dÃ i chuá»—i)* | $0$ Ä‘áº¿n $255$ kÃ½ tá»± | *N/A (KhÃ´ng cÃ³ Ä‘á»™ dÃ i Ã¢m)* | `0 kÃ½ tá»±` (`""`) *(Valid - Táº£i táº¥t cáº£ SP)* | `1 kÃ½ tá»±` (`"i"`) *(Valid - Lá»c SP)* | `10 kÃ½ tá»±` (`"iPhone 15"`) *(Valid)* | `254 kÃ½ tá»±` *(Valid)* | `255 kÃ½ tá»±` *(Valid)* | `256 kÃ½ tá»±` *(Valid - Empty State)* |

---

## 4. Káº¾T QUáº¢ THá»°C THI KIá»‚M THá»¬ THá»°C Táº¾ (TEST EXECUTION RESULTS)

Táº¥t cáº£ 14 ká»‹ch báº£n kiá»ƒm thá»­ Ä‘Ã£ Ä‘Æ°á»£c thá»±c thi trá»±c tiáº¿p trÃªn há»‡ thá»‘ng Mobile SUT (Expo React Native, Backend API `http://localhost:3000`, CSDL `database.sqlite`). ToÃ n bá»™ hÃ¬nh áº£nh báº±ng chá»©ng Ä‘Ã£ Ä‘Æ°á»£c lÆ°u trá»¯ Ä‘áº§y Ä‘á»§ táº¡i thÆ° má»¥c `screenshots/pool D/`.

| MÃ£ TC | Ká»‹ch báº£n kiá»ƒm thá»­ | Dá»¯ liá»‡u Ä‘áº§u vÃ o (Input) | Káº¿t quáº£ mong Ä‘á»£i (Expected Result) | Káº¿t quáº£ thá»±c táº¿ (Actual Result) | Tráº¡ng thÃ¡i | Báº±ng chá»©ng (Screenshot) | MÃ£ lá»—i (Bug ID) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| **EXE-01** | Kiá»ƒm tra danh sÃ¡ch máº·c Ä‘á»‹nh & áº¢nh sáº£n pháº©m | `search = ""` | Táº£i 5 SP, giÃ¡ cÃ³ `â‚«`, áº£nh Ä‘Ãºng tá»· lá»‡ chuáº©n, cÃ³ alt | Giao diá»‡n Mobile táº£i Ä‘áº§y Ä‘á»§ 5 sáº£n pháº©m máº·c Ä‘á»‹nh kÃ¨m giÃ¡ tiá»n cÃ³ phÃ¢n cÃ¡ch hÃ ng nghÃ¬n (â‚«). Tuy nhiÃªn áº£nh sáº£n pháº©m bá»‹ mÃ©o tá»· lá»‡ do `resizeMode='stretch'` vÃ  khÃ´ng cÃ³ thuá»™c tÃ­nh alt text mÃ´ táº£. | **FAIL** | `EXE-01_GUI_Default_Product_List.png` | **BUG-FR05-04** |
| **EXE-02** | Khá»›p chÃ­nh xÃ¡c 100% tÃªn sáº£n pháº©m | `"iPhone 15 Pro Max"` | Tráº£ vá» duy nháº¥t 1 SP iPhone 15 Pro Max | Há»‡ thá»‘ng tÃ¬m kiáº¿m chÃ­nh xÃ¡c vÃ  hiá»ƒn thá»‹ duy nháº¥t sáº£n pháº©m iPhone 15 Pro Max vá»›i Ä‘áº§y Ä‘á»§ thÃ´ng tin tÃªn, áº£nh vÃ  giÃ¡ tiá»n 30.000.000 â‚«. | **PASS** | `EXE-02_Search_Exact_Match_Success.png` | â€” |
| **EXE-03** | Khá»›p má»™t pháº§n tÃªn sáº£n pháº©m (Substring) | `"iPhone"` | Tráº£ vá» cÃ¡c SP cÃ³ tÃªn chá»©a 'iPhone' | Há»‡ thá»‘ng tÃ¬m kiáº¿m theo chuá»—i con thÃ nh cÃ´ng, tráº£ vá» sáº£n pháº©m iPhone 15 Pro Max khi nháº­p tá»« khÃ³a 'iPhone'. | **PASS** | `EXE-03_Search_Partial_Match_Success.png` | â€” |
| **EXE-04** | TÃ¬m kiáº¿m khÃ´ng phÃ¢n biá»‡t hoa / thÆ°á»ng | `"iphone"` | Tráº£ vá» cÃ¡c SP cÃ³ tÃªn chá»©a 'iPhone' | Há»‡ thá»‘ng tÃ¬m kiáº¿m khÃ´ng phÃ¢n biá»‡t chá»¯ hoa chá»¯ thÆ°á»ng, tá»« khÃ³a 'iphone' váº«n tÃ¬m Ä‘Æ°á»£c sáº£n pháº©m 'iPhone 15 Pro Max'. | **PASS** | `EXE-04_Search_Case_Insensitive_Success.png` | â€” |
| **EXE-05** | Tá»« khÃ³a cÃ³ khoáº£ng tráº¯ng Ä‘áº§u vÃ  cuá»‘i | `"  iPhone  "` | Tá»± Ä‘á»™ng trim khoáº£ng tráº¯ng vÃ  tÃ¬m tháº¥y SP | Backend khÃ´ng cáº¯t bá» khoáº£ng tráº¯ng thá»«a (untrimmed whitespace), khiáº¿n tá»« khÃ³a '  iPhone  ' khÃ´ng tÃ¬m tháº¥y báº¥t ká»³ sáº£n pháº©m nÃ o. | **FAIL** | `EXE-05_Bug_Untrimmed_Whitespace_Fail.png` | **BUG-FR05-02** |
| **EXE-06** | Tá»« khÃ³a khÃ´ng tá»“n táº¡i trong CSDL | `"XYZ999"` | Hiá»ƒn thá»‹ giao diá»‡n Empty State thÃ¢n thiá»‡n kÃ¨m hÃ¬nh minh há»a | Khi khÃ´ng cÃ³ káº¿t quáº£ tÃ¬m kiáº¿m, giao diá»‡n Mobile hoÃ n toÃ n tráº¯ng trÆ¡n, thiáº¿u component Empty State kÃ¨m hÃ¬nh áº£nh minh há»a theo yÃªu cáº§u cá»§a FR-05. | **FAIL** | `EXE-06_Bug_Missing_Empty_State.png` | **BUG-FR05-01** |
| **EXE-07** | TÃ¬m theo tá»« khÃ³a trong MÃ´ táº£ / Danh má»¥c | `"Äiá»‡n thoáº¡i"` | Chá»‰ tÃ¬m theo TÃªn, khÃ´ng ra SP nÃ o | Há»‡ thá»‘ng tuÃ¢n thá»§ Ä‘Ãºng pháº¡m vi Ä‘áº·c táº£: chá»‰ tÃ¬m kiáº¿m theo trÆ°á»ng TÃªn sáº£n pháº©m, khÃ´ng tÃ¬m theo MÃ´ táº£ (tá»« khÃ³a 'Äiá»‡n thoáº¡i' khÃ´ng khá»›p vá»›i tÃªn sáº£n pháº©m nÃ o). | **PASS** | `EXE-07_Scope_Search_Name_Only.png` | â€” |
| **EXE-08** | Táº¥n cÃ´ng báº£o máº­t SQL Injection | `"' OR 1=1 --"` | Xá»­ lÃ½ an toÃ n nhÆ° chuá»—i text, 0 káº¿t quáº£ | Lá»— há»•ng SQL Injection cá»±c ká»³ nghiÃªm trá»ng: Backend ná»‘i chuá»—i trá»±c tiáº¿p vÃ o cÃ¢u lá»‡nh SQL. Payload '' OR 1=1 --' lÃ m vÃ´ hiá»‡u hÃ³a bá»™ lá»c vÃ  lÃ m lá»™ toÃ n bá»™ danh sÃ¡ch sáº£n pháº©m. | **FAIL** | `EXE-08_Bug_Critical_SQL_Injection.png` | **BUG-FR05-03** |
| **EXE-09** | Táº¥n cÃ´ng gÃ¢y lá»—i cÃº phÃ¡p SQL & Lá»™ mÃ£ HTML | `"'"` | BÃ¡o lá»—i thÃ¢n thiá»‡n 400 Bad Request | Khi nháº­p kÃ½ tá»± nhÃ¡y Ä‘Æ¡n ('), Backend gáº·p lá»—i cÃº phÃ¡p SQL vÃ  tráº£ vá» HTTP 500 kÃ¨m mÃ£ HTML lá»—i. á»¨ng dá»¥ng Mobile render nguyÃªn chuá»—i HTML thÃ´ lÃªn giao diá»‡n ngÆ°á»i dÃ¹ng. | **FAIL** | `EXE-09_Bug_SQL_Error_HTML_Leak.png` | **BUG-FR05-03** |
| **EXE-10** | Táº¥n cÃ´ng XSS & Tháº» H1 trong tá»« khÃ³a | `"<h1><script>alert(1)</script></h1>"` | Escape HTML an toÃ n, khÃ´ng cháº¡y script | Há»‡ thá»‘ng an toÃ n trÆ°á»›c táº¥n cÃ´ng XSS. React Native tá»± Ä‘á»™ng escape chuá»—i tháº» HTML/script vÃ  hiá»ƒn thá»‹ an toÃ n dÆ°á»›i dáº¡ng vÄƒn báº£n thuáº§n. | **PASS** | `EXE-10_Security_XSS_Safely_Escaped.png` | â€” |
| **EXE-11** | BVA BiÃªn dÆ°á»›i Min (0 kÃ½ tá»±) | `""` (0 kÃ½ tá»±) | Táº£i láº¡i toÃ n bá»™ 5 sáº£n pháº©m | Táº¡i biÃªn Min (0 kÃ½ tá»±), há»‡ thá»‘ng táº£i vÃ  hiá»ƒn thá»‹ láº¡i toÃ n bá»™ danh sÃ¡ch 5 sáº£n pháº©m dáº¡ng danh sÃ¡ch card. | **PASS** | `EXE-11_BVA_Min_0_Char_All_Products.png` | â€” |
| **EXE-12** | BVA BiÃªn dÆ°á»›i há»£p lá»‡ Min+1 (1 kÃ½ tá»±) | `"i"` (1 kÃ½ tá»±) | Tráº£ vá» cÃ¡c SP cÃ³ chá»¯ 'i' | Táº¡i biÃªn Min+1 (1 kÃ½ tá»± 'i'), há»‡ thá»‘ng lá»c chÃ­nh xÃ¡c 4 sáº£n pháº©m cÃ³ tÃªn chá»©a chá»¯ 'i'. | **PASS** | `EXE-12_BVA_MinPlus1_1_Char_Success.png` | â€” |
| **EXE-13** | BVA BiÃªn trÃªn há»£p lá»‡ Max (255 kÃ½ tá»±) | Chuá»—i 255 kÃ½ tá»± | Tiáº¿p nháº­n tá»‘t, hiá»ƒn thá»‹ Empty State | Há»‡ thá»‘ng tiáº¿p nháº­n tá»‘t chuá»—i tÃ¬m kiáº¿m táº¡i biÃªn Max 255 kÃ½ tá»± mÃ  khÃ´ng bá»‹ treo hay vá»¡ khung. Tuy nhiÃªn mÃ n hÃ¬nh bá»‹ tráº¯ng trÆ¡n do thiáº¿u Empty State. | **FAIL** | `EXE-13_BVA_Max_255_Chars.png` | **BUG-FR05-01** |
| **EXE-14** | BVA VÆ°á»£t biÃªn trÃªn Max+1 (256 kÃ½ tá»±) | Chuá»—i 256 kÃ½ tá»± | Tiáº¿p nháº­n tá»‘t, hiá»ƒn thá»‹ Empty State | Há»‡ thá»‘ng khÃ´ng crash khi nháº­n chuá»—i 256 kÃ½ tá»± (Max+1), nhÆ°ng bá»‹ lá»—i giao diá»‡n tráº¯ng trÆ¡n do thiáº¿u Empty State. | **FAIL** | `EXE-14_BVA_MaxPlus1_256_Chars.png` | **BUG-FR05-01** |

---

## 5. Tá»”NG Há»¢P CÃC Lá»–I PHÃT HIá»†N (DEFECT SUMMARY)

Tá»•ng sá»‘ test cases thá»±c thi: **14**  
* **Passed:** 7/14 (50.00%)  
* **Failed (PhÃ¡t hiá»‡n lá»—i):** 7/14 (50.00%)  

### Danh má»¥c 4 nhÃ³m lá»—i phÃ¡t hiá»‡n Ä‘Æ°á»£c:
1. **BUG-FR05-01 (High - Missing Empty State)**: MÃ n hÃ¬nh dÆ°á»›i thanh tÃ¬m kiáº¿m hoÃ n toÃ n tráº¯ng tinh, thiáº¿u háº³n component Empty State kÃ¨m hÃ¬nh minh há»a vÃ  thÃ´ng Ä‘iá»‡p thÃ¢n thiá»‡n khi khÃ´ng tÃ¬m tháº¥y sáº£n pháº©m (`EXE-06`, `EXE-13`, `EXE-14`).
2. **BUG-FR05-02 (Medium - Data Handling / Untrimmed String)**: Backend `server.js` khÃ´ng cáº¯t bá» khoáº£ng tráº¯ng thá»«a Ä‘áº§u cuá»‘i (`trim()`), khiáº¿n tá»« khÃ³a `"  iPhone  "` tráº£ vá» 0 káº¿t quáº£ dÃ¹ sáº£n pháº©m cÃ³ tá»“n táº¡i (`EXE-05`).
3. **BUG-FR05-03 (Critical - Security / SQL Injection & Raw HTML Leak)**: Backend ná»‘i chuá»—i SQL trá»±c tiáº¿p (`WHERE name LIKE '%${searchQuery}%'`), cho phÃ©p bypass bá»™ lá»c báº±ng `' OR 1=1 --` vÃ  lÃ m sáº­p 500 khi gáº·p dáº¥u nhÃ¡y `'`, vÄƒng chuá»—i HTML thÃ´ `<h1>Database Error</h1>` lÃªn mÃ n hÃ¬nh Mobile (`EXE-08`, `EXE-09`).
4. **BUG-FR05-04 (Low/Medium - GUI Standards & Accessibility)**: áº¢nh sáº£n pháº©m bá»‹ mÃ©o mÃ³ do cáº¥u hÃ¬nh `resizeMode="stretch"` trong `App.js` dÃ²ng 460 (vi pháº¡m yÃªu cáº§u áº£nh tá»· lá»‡ chuáº©n), Ä‘á»“ng thá»i tháº» `<Image>` thiáº¿u thuá»™c tÃ­nh `accessibilityLabel` / `alt text` mÃ´ táº£ áº£nh (`EXE-01`).

