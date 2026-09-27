# ULTRA SCRAPER v3 — Google Maps Lead Extractor

Google Maps ki search list se saare businesses ka data nikal kar ek saaf-suthri
`.xlsx` file bana deta hai — naam, phone, website, social links, aur poora tuta
hua address (Street / City / State / Zip / Country alag alag columns me).

**Files:**

| File | Ye kya hai |
|---|---|
| `G_ULTRA_v3.js` | Asal code — yehi console me paste hota hai |
| `G_ULTRA_v3_code.txt` | Wahi code `.txt` me (Chrome `.js` download block kar deta hai) |
| `README.md` | Ye file |

---

## 1. Chalane ka tareeqa (4 step)

1. **Chrome** me Google Maps kholo aur apna keyword search karo
   → jaise `cafes in Lahore`, `dentists in Islamabad`, `gyms in Multan`
   Left side me results ki list dikhni chahiye.
2. **`F12`** dabao → upar **Console** tab pe jao.
3. Code file kholo → `Ctrl+A` → `Ctrl+C` → console me **paste** → **Enter**.
4. 20–60 second ruko. `.xlsx` khud download ho jayegi.

> Agar console pehli baar "paste karne se mana kare", to usme `allow pasting`
> type karke Enter dabao, phir code paste karo. (Chrome ki security hai, ek hi
> baar karna parta hai.)

---

## 2. ⚠️ NAYI SEARCH KAISE KARNI HAI (ye zaroor parho)

Google Maps ki ek aadat hai: jab tum **search box** me naya word likh kar Enter
dabate ho, wo poori list load hi nahi karta — sirf ek chhoti "preview" bhejta
hai. Us waqt scraper ko sirf **2-3 results** milte hain.

Aur **`F5` bhi is ka hal nahi hai** — kyunki aksar Maps address bar update hi
nahi karta, to F5 dabane par **purani search wapas** aa jati hai. Bahut log
yahin phans jate hain.

**Sahi tareeqe (koi bhi ek):**

- ✅ **Sabse aasan — kuch mat karo.** Search box se hi search karo aur code
  chala do. Agar list adhoori hui to code **khud page ko sahi search pe le
  jayega** aur bolega: *"3-4 second baad code dobara paste karein."* Bas dobara
  paste kar do — poora data aa jayega.
- ✅ **Address bar se search karo:** `google.com/maps/search/gyms+in+Multan`
  Isse page hamesha saaf load hota hai, pehli hi baar me poora data milta hai.

**Asli test ka natija, bilkul same search term pe:**

| Tareeqa | Leads mile |
|---|---|
| Sirf search box, page saaf load nahi hua | **3** ❌ |
| Code ne khud theek kiya → dobara paste | **poore** ✅ |
| Address bar se seedha URL | **poore** ✅ |

Chahe kuch bhi ho jaye — **galat ya adhoora data chupke se file me nahi
aayega.** Code pehle check karta hai, phir hi file banata hai.

---

## 2b. Ek hi search pe kabhi kam kabhi zyada leads kyun?

Ye bhi Google ka behaviour hai, code ka nahi. **Maps ke results map ke view
(zoom aur position) se badalte hain** — jo area screen pe dikh raha hai, uske
hisaab se Google alag list deta hai.

Live naapa, keyword bilkul same (`cafes in Lahore`), sirf zoom badla:

| Map ka zoom | Leads mile |
|---|---|
| `10z` (door se, poora sheher) | **154** |
| `12z` (Google ka default) | **155** |
| `15z` (paas se, sirf ek ilaqa) | **168** |

Aur ek hi zoom pe baar baar chalao to farq sirf **±1** aata hai (154 / 155 /
154) — wo Google ki apni thori si jitter hai, us par kisi ka control nahi.

**Number stable rakhne ka tareeqa:** hamesha **address bar** se saaf URL kholo —
`google.com/maps/search/cafes+in+lahore` — bina `@lat,lng,zoom` wale hisse ke.
Google har baar apna default view lagayega, aur count har baar ek jaisa aayega.

Agar tumne list scroll ki, kisi pin pe click kiya, ya map ko zoom/pan kiya —
to view badal jata hai aur agli baar count alag aayega. Ye galat data nahi,
bas alag ilaqe ka data hai.

> Code ab console me bata bhi deta hai: `🗺️ Map view: 12z — zoom badalne se
> results thore kam/zyada ho sakte hain.`

**Ek bug bhi tha jo isme apna hissa daal raha tha:** pehle agar kisi ek page pe
saare business duplicate nikalte, to code samajhta tha "list khatam" aur wahin
ruk jata tha — halanke aage aur data hota tha. Ab wo sirf tab rukta hai jab
Google bilkul khali page de, ya **lagataar do** page pe koi naya business na aaye.

---

## 3. Excel me kaunse columns aate hain

| Column | Kya hota hai | Misal |
|---|---|---|
| `Keyword` | Jo search tumne ki | `cafes in Lahore` |
| `Business Name` | Business ka naam | `Arcadian Café` |
| `Category` | Google ki category | `Cafe`, `Pharmacy` |
| `Rating` | Sitare | `4.4` |
| `Reviews` | Kitne reviews | `9412` |
| `Street` | Sirf street/area (city-country nikal ke) | `Sir Syed Rd, Block K Gulberg 2` |
| `City` | Sheher | `Lahore` |
| `State` | Suba (Pakistan me aksar khali, US/UAE me bharta hai) | `Illinois` |
| `Zip` | Postal code | `54660` |
| `Country` | Mulk | `Pakistan` |
| `Phone` | **Saaf** — na country code na zero | `345-845-6753` |
| `Phone (Original)` | Google ka **asli** number, code ke saath | `+92 345 8456753` |
| `Website` | Business ki website (clickable) | `https://cafezouk.com` |
| `Facebook` / `Instagram` / `WhatsApp` … | Jo social milen (clickable) | |
| `Plus Code` | Google plus code (Street se nikal kar yahan) | `GC29+FQH` |
| `Hours` | Abhi khula/band | `Closes 2 AM` |
| `Price` | Price range agar ho | `Rs 2,000–3,000` |
| `Google Maps URL` | **Seedha listing ka link** (clickable) | `…place_id:ChIJ…` |
| `Full Address` | Sab kuch ek line me | |

**Do phone columns kyun?**
`Phone` dialer / CRM / bulk calling ke liye — saaf digits.
`Phone (Original)` WhatsApp aur international dialing ke liye — country code
ke saath seedha copy-paste.

**Social columns:** sirf wahi columns banenge jo us search me kisi na kisi ke
paas mile (Facebook hamesha aata hai). Isliye har file me columns thore alag
ho sakte hain — ye normal hai.

---

## 4. Console kya kehta hai, aur kyun

Code chalte waqt console me messages aate hain. Har ek ka matlab:

| Message | Matlab | Tumhe kya karna hai |
|---|---|---|
| `🔎 Search term: "…"` se pehle kuch nahi | Code ab `.xlsx` khud banata hai, koi library download nahi karta | — |
| `🔎 Search term: "…"` | Code ne ye search term pakra hai | **Check karo ye wahi hai jo tumne search kiya.** Galat ho to F5 karo |
| `⏳ Is search ka request dhoond rahe hain…` | Data abhi nahi mila, khud scroll karke dhoond raha hai | Kuch nahi, 5 sec ruko |
| `✅ Sahi request mil gayi (3 me se, pehle page pe 20 results)` | Kai requests aazma kar sabse achhi chun li | Ruk jao |
| `page 1: +18 naye \| total: 18` | Har page ka hisaab | Ruk jao |
| `(list khatam)` | Google ke paas aur results nahi | Normal — ab file banegi |
| `✅ TOTAL 153 leads \| phone: 141 …` | Kaam ho gaya, ye summary hai | 👍 |
| `🎉 DONE — 153 leads. Download: …xlsx` | File download ho gayi | Downloads folder dekho |

### 🔄 Auto-fix wala message (sabse aam)

**`⚠️ Maps ne poori list load nahi ki thi (sirf 3 results mil rahe the).`**
**`🔄 Page ko sahi search pe le ja raha hoon (F5 mat dabaein…)`**
**`👉 Page load hone ke 3-4 second baad ye code DOBARA paste karein.`**

- **Kyun:** Search box se search karne par Maps ne poori list load nahi ki thi.
- **Kya hua:** Code ne khud page ko saaf URL pe bhej diya.
- **Tum kya karo:** Page load hone do → 3-4 second ruko → **code dobara paste
  karo**. Ab poora data aayega. (Ye sirf ek baar hota hai, loop nahi banega.)

Asli test: pehle `3` results → auto-fix → dobara paste → `17` (jitne wahan
sach me the).

### ❌ Rukne wale messages

**`❌ Is search term ka data nahi mila.`**
**`👉 Ye link address bar me kholein aur phir code paste karein: …`**

- **Kyun:** Auto-fix pehle hi ek baar try kar chuka hai aur phir bhi nahi bana.
- **Hal:** Jo link console me likha hai, use copy karke **address bar** me kholo,
  page load hone do, phir code paste karo.
- **Ye ruka kyun:** Warna file ban to jati, magar usme **purani search ka data**
  hota naye keyword ke naam se. Ye sabse khatarnak galti hai — isliye code jaan
  boojh kar rukta hai.

**`⚠️ Is search me sirf 8 results mile — shayad itne hi maujood hain.`**
- Auto-fix ho chuka hai aur ab bhi kam hain → matlab sach me itne hi businesses
  hain. File theek hai.

**`Kuch nahi mila.`**
- Search me koi result hi nahi, ya page load nahi hua tha. Address bar wala
  tareeqa use karo.

**`XLSX fail — CSV fallback.`** (purane version ka message)
- Ab ye aa hi nahi sakta. Pehle code Excel library internet se load karta tha,
  aur Google Maps ki security (CSP) usse rok deti thi — isliye `.csv` ban jati
  thi. Ab code `.xlsx` **khud** banata hai, koi library/internet nahi chahiye.

---

## 5. Settings (code ke bilkul upar)

```js
const MAX_PAGES  = 15;     // 15 x 20 = 300 leads tak
const PAGE_DELAY = 1200;   // har page ke beech wait (ms)
const FORCE_CITY = '';     // city khud pakri jati hai; yahan likh do to wahi lagegi
```

- **Zyada leads chahiye?** `MAX_PAGES` ko `25` ya `30` kar do.
  (Google khud aksar 100–200 pe list khatam kar deta hai — us se zyada nahi milta.)
- **Google slow lag raha ho / block ka dar ho?** `PAGE_DELAY` ko `2000` kar do.
- **City galat aa rahi ho?** `FORCE_CITY = 'Lahore'` likh do, har row me wahi
  aayegi.

---

## 6. Ye code andar kya karta hai (short me)

1. Page se **search term** pakarta hai (search box se — URL se nahi, kyunki URL
   peeche reh jata hai).
2. Browser ki network history se Maps ki **saari** data requests dhoondta hai
   jo isi search term ki hain — phir **har ek ko aazma kar** dekhta hai kaunsi
   sabse zyada results deti hai. (Maps chhoti "preview" requests bhi bhejta hai
   jo sirf 2-3 result deti hain — yehi 3-lead wale masle ki jad thi.)
3. Us request ko page-by-page (20, 40, 60…) khud call karta hai. **Koi listing
   kholi nahi jati, koi click nahi** — isliye bahut tez hai.
4. Har business ka address Google ki **apni structured fields** se leta hai
   (guess karke comma se todta nahi) — isliye City/Street kabhi mix nahi hote.
5. Address saaf karta hai: plus code alag, duplicate tukde hataye
   (`F-7 Markaz F7 Markaz F-7` → `F-7 Markaz`), asterisk `**` jaisa kachra saaf.
6. Har business ka **asli place_id** le kar seedha khulne wala Maps link banata hai.
   (Jis row ka place_id na ho, wo asli business hai hi nahi — Google ka internal
   token hai — us ko chhod deta hai.)
7. Website chunte waqt Google ki apni tasveerein/files (`googleapis`,
   `googleusercontent`, `.png`, `.jpg`, `/thumbnail`) aur delivery/aggregator
   sites (foodpanda, zomato, yelp…) reject karta hai — sirf asli business site leta hai.
8. Sab kuch `.xlsx` me — links clickable, autofilter laga hua.

---

## 7. Masle aur unka hal

| Masla | Hal |
|---|---|
| `.js` file download nahi ho rahi | Chrome `.js` block karta hai — `.txt` wali file use karo, ya `chrome://downloads` me "Keep" dabao |
| Console me paste nahi hone deta | Console me `allow pasting` likh kar Enter, phir paste |
| Har baar wahi purana data aa raha hai | Search box ke bajaye **address bar** se search karo: `google.com/maps/search/keyword` |
| Sirf 2-3 leads aa rahe hain | Code khud page theek kar dega — bas **dobara paste** karo. Ya address bar wala tareeqa use karo |
| City ki jagah kuch ajeeb likha hai | Us business ke owner ne Google pe galat likha hai. Code aksar theek kar deta hai; na kare to `FORCE_CITY` laga do |
| Category "Restaurant" aa raha hai jabke "cafes" search kiya | Ye Google khud deta hai — uski apni list me bhi wahi hai. Excel me `Category` pe filter laga lo |
| Phone khali hai | Us business ne Google pe number diya hi nahi |
| `.xlsx` ki jagah `.csv` mili | Purana version chala rahe ho. Naya code library use hi nahi karta — hamesha `.xlsx` deta hai |
| Website khali, magar Twitter column me website pari hai | Purane version ka bug (`complex.com` ke andar `x.com` match ho jata tha). Naya version theek hai |
| Website column me `streetviewpixels-pa.googleapis.com` jaisa link | Purane version ka bug — wo Google ki **tasveer** ka link tha, website nahi. Naya version har aisa link block karta hai |
| Business Name me ajeeb code jaisa `0ahUKEwi1-YfD4c...` | Google ke internal tokens the. Ab har row ka asli place_id lazmi hai — aise rows bante hi nahi |
| Sirf 20 leads aaye | `MAX_PAGES` check karo, aur address bar se saaf URL kholo |
| Har baar leads ki tadaad alag aa rahi hai | Map ka zoom badal gaya hai — section **2b** parho. Address bar se saaf URL use karo |

---

## 7b. Data kahan se aata hai (ye sabse ahem badlav hai)

Pehle code **poore data me regex se dhoondta tha** — "koi URL mil jaye to wo
website hai", "koi phone jaisa number mil jaye to wo phone hai". Isi wajah se
kachra ghusta tha (Street View ki tasveer website ban jati thi waghera).

**Ab aisa nahi hota.** Har cheez Google ke apne mukarrar khane se aati hai:

| Column | Google ka khana |
|---|---|
| Business Name | `b[11]` |
| Place ID (row asli hai ya nahi) | `b[78]` |
| Street / City / State / Zip / Country | `b[183][1]` |
| **Website** | `b[7][0]` |
| **Phone** | `b[178][0][0]` |
| Rating / Reviews | `b[4][7]`, `b[4][8]` |
| Price | `b[4][2]` |
| Hours | `b[203][1]` |
| Category | `b[13][0]` |

Aur **har value likhne se pehle validator pass karti hai** — phone phone jaisa
ho, URL asli domain ho, rating 0–5 ho, zip zip jaisa ho. Jo fail kare wo khana
**khali** chhod diya jata hai. Galat value file me ja hi nahi sakti.

Isi liye ab naya kachra bhi apne aap ruk jayega — chahe wo aaj mojood hi na ho.
Aur run ke aakhir me code khud check karke batata hai:

```
🔍 QC pass — koi kharab value nahi mili.
```

Agar kuch reject hua ho to wo bhi likh deta hai, jaise:
`(validator ne reject kiye: website x3 — wo khane khali chhode gaye)`

---

## 8. Kya kya test ho chuka hai

Har build ke baad **15 alag search terms** par live chala kar 11 cheezein
check ki jati hain (junk website, social ka website me ghusna, ghalat phone /
rating / reviews / zip / hours, token wale naam, ghalat place_id, duplicate,
city ki jagah suba). **Aakhri run: 2,196 rows — koi fail nahi.**

| Search | Mulk | Rows | |
|---|---|---|---|
| cafes in Lahore | 🇵🇰 Pakistan | 153 | ✅ |
| dentists in Islamabad | 🇵🇰 Pakistan | 206 | ✅ |
| bakeries in Karachi | 🇵🇰 Pakistan | 184 | ✅ |
| coffee shops in Chicago | 🇺🇸 USA | 105 | ✅ |
| law firms in New York | 🇺🇸 USA | 299 | ✅ |
| plumbers in London | 🇬🇧 UK | 165 | ✅ |
| supermarkets in Berlin | 🇩🇪 Germany | 128 | ✅ |
| hotels in Dubai | 🇦🇪 UAE | 292 | ✅ |
| car rental in Riyadh | 🇸🇦 Saudi Arabia | 88 | ✅ |
| car repair in Toronto | 🇨🇦 Canada | 160 | ✅ |
| ramen in Tokyo | 🇯🇵 Japan | 141 | ✅ |
| restaurantes em Sao Paulo | 🇧🇷 Brazil | 102 | ✅ |
| textile shops in Mumbai | 🇮🇳 India | 147 | ✅ |
| logistics companies in Lagos | 🇳🇬 Nigeria | 181 | ✅ |
| warung di Jakarta | 🇮🇩 Indonesia | 107 | ✅ |

**10 mulk, 4 rasm-ul-khat** (angrezi, urdu, japani, portugali/hurūf-e-shosha),
**15 alag industries**, aur non-angrezi search words (`restaurantes em`,
`warung di`) bhi.

Aakhri test se pehle poori file **end-to-end** bhi chalayi gayi (Sao Paulo) —
`.xlsx` bani, 21 columns, links clickable, `🔍 QC pass`.

---

## 9. Dhyan rakhne wali baatein

- Ye Google Maps ka **publicly dikhne wala** data leta hai — wahi jo koi bhi
  apni aankh se list me dekh sakta hai.
- Ek hi baithak me bohot zyada baar mat chalao. Google rate-limit laga sakta
  hai. Do searches ke beech thora waqfa do.
- Google apna internal data format kabhi kabhi badalta hai. Agar kisi din code
  achanak kaam karna band kar de, to yehi wajah hogi — code ko update karna parega.
- Marketing/outreach ke liye numbers use karte waqt local rules (spam/DNC) ka
  khayal rakhna tumhari zimmedari hai.
