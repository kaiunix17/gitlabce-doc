# Wiki

Wiki loyiha va guruh hujjatlarini tanish formatda taqdim etadi. Wiki sahifalari:

- Markdown, RDoc, AsciiDoc yoki Org formatlarida texnik hujjatlar, qo'llanmalar va bilimlar bazasini yaratadi.
- GitLab loyihalari va guruhlari bilan bevosita integratsiyalashgan hamkorlik hujjatlarini yaratadi.
- Oflayn kirish va ulashish uchun kontentni PDF fayllari sifatida eksport qiladi.
- Kontentingizni kod bazasidan alohida saqlaydi, lekin ularni bir xil loyihada ushlab turadi.

## Loyiha wikisini ko'rish

Loyiha wikisiga kirish uchun:

1. Projectga kiramiz
2. Wikini ko'rsatish uchun quyidagilardan birini bajaring:
    - Chap yon panelda **Reja > Wiki**ni tanlang.
    - Loyihadagi istalgan sahifada `g + w` wiki klaviatura yorlig'idan foydalaning. 

## Wiki bosh sahifasini yaratish

Wiki yaratilganda, u bo'sh bo'ladi. Birinchi tashrifingizda foydalanuvchilar wikini ko'rganda ko'radigan bosh sahifani yaratishingiz mumkin. 
Bu sahifa wikingizning bosh sahifasi sifatida ishlatilishi uchun maxsus yo'lni talab qiladi. Uni yaratish uchun:

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. **Birinchi sahifangizni yarating**ni tanlang.
4. Ixtiyoriy. Bosh sahifaning **Sarlavha**sini o'zgartiring.
    - GitLab bu birinchi sahifaning `home` yo'liga ega bo'lishini talab qiladi.
5. Matnni yozish uchun **Format**ni tanlang.
6. **Kontent**ga biron nima yozing. Uni keyinroq edit qilish mumkin.
7. **Commit xabari**ni qo'shing. Git commit xabarini talab qiladi, shuning uchun siz o'zingiz kiritmasak uzi auto yozadi, GitLab uni yaratadi.
8. **Create page**ni tanlang.

- `Yangi wiki sahifasini yaratish`: Kamida Developer roli bo'lishi kerak.

## Wiki pagelarni clone qilish va edit qilish

Wikilarni clone qilish mumkin

2. **Reja > Wiki**ni tanlang.
3. **Wiki harakatlari** (⚙) ni, so'ngra **Omborni klonlash**ni tanlang.
4. Ekrandagi ko'rsatmalarga amal qiling.

Quyidagi wiki yozish typelari hisoblanadi.

- **Markdown typelar:** .mdown, .mkd, .mkdn, .md, .markdown
- **AsciiDoc typelar:** .adoc, .ad, .asciidoc
- **Boshqa typelar:** .textile, .rdoc, .org, .creole, .wiki, .mediawiki, .rst

## Fayl va katalog nomlari uchun uzunlik cheklovlari

Ko'pgina umumiy fayl tizimlari fayl va katalog nomlari uchun 255 bayt chegarasiga ega.

- Fayl nomlari uchun 245 bayt (fayl kengaytmasi uchun 10 baytni saqlab qolish).
- Katalog nomlari uchun 255 bayt.
- ASCII bo'lmagan belgilar bir baytdan ko'proq joy egallaydi.

## Wiki sahifasini tahrirlash

**Oldindan talab:**
- Kamida Developer roli bo'lishi kerak.

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. Tahrirlashni istagan sahifaga o'ting va quyidagilardan birini bajaring:
    - `e` wiki klaviatura yorlig'idan foydalaning.
    - **Tahrirlash**ni tanlang.
4. Kontentni tahrirlang.
5. **O'zgarishlarni saqlash**ni tanlang.

Wiki sahifasiga saqlanmagan o'zgarishlar tasodifiy ma'lumot yo'qolishini oldini olish uchun mahalliy brauzer xotirasida saqlanadi.

## Mundarija yaratish

Kontentida sarlavhalari bo'lgan wiki sahifalari yon panelda mundarija bo'limini avtomatik ravishda ko'rsatadi.

Shuningdek, sahifaning o'zida alohida mundarija bo'limini ko'rsatishni tanlashingiz mumkin. Wiki sahifasining quyi sarlavhalaridan mundarija yaratish uchun `[[_TOC_]]` tegidan foydalaning.

## Wiki sahifasini o'chirish

**Oldindan talab:**
- Kamida Developer roli bo'lishi kerak.

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. O'chirmoqchi bo'lgan sahifaga o'ting.
4. **Wiki harakatlari** (⚙) ni, so'ngra **Sahifani o'chirish**ni tanlang.
5. O'chirishni tasdiqlang.

## Wiki sahifasini ko'chirish yoki qayta nomlash

GitLab 17.1 va undan keyingi versiyalarda sahifani ko'chirganda yoki qayta nomlaganda eski sahifadan yangi sahifaga avtomatik yo'naltirish o'rnatiladi. Yo'naltirishlar ro'yxati Wiki omboridagi `.gitlab/redirects.yml` faylida saqlanadi.

**Oldindan talab:**
- Kamida Developer roli bo'lishi kerak.

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. Ko'chirmoqchi yoki qayta nomlamoqchi bo'lgan sahifaga o'ting.
4. **Tahrirlash**ni tanlang.
5. Sahifani ko'chirish uchun **Yo'l** maydoniga yangi yo'lni qo'shing.
6. Sahifani qayta nomlash uchun **Yo'l**ni o'zgartiring.
7. **O'zgarishlarni saqlash**ni tanlang.

## Wiki sahifasini eksport qilish

Wiki sahifasini PDF fayl sifatida eksport qilishingiz mumkin:

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. Eksport qilmoqchi bo'lgan sahifaga o'ting.
4. Yuqori o'ng tomonda **Wiki harakatlari** (⚙) ni, so'ngra **PDF sifatida chop etish**ni tanlang.
5. Wiki sahifasining PDF fayli yaratiladi.

## Draw.io yordamida wikida diagrammalar yaratish

diagrams.net integratsiyasi bilan wiki sahifalarida SVG diagrammalarini yaratish va joylashtishingiz mumkin! Diagramma muharriri oddiy matn muharriri va boy matn muharriri ikkalasida ham mavjud.

GitLab.com-da bu integratsiya barcha SaaS foydalanuvchilari uchun yoqilgan va qo'shimcha sozlashni talab qilmaydi.

GitLab Self-Managed-da siz bepul diagrams.net veb-sayti bilan integratsiyalashishingiz yoki oflayn muhitlarda o'zingizning diagrams.net saytingizni joylashtirish mumkin.

## Wiki sahifa shablonlari

Yangi sahifalar yaratishda foydalanish yoki mavjud sahifalarga qo'llash uchun shablonlar yaratishingiz mumkin. Shablonlar wiki omboridagi `templates/` katalogida saqlanadigan wiki sahifalaridir.

### Shablon yaratish

**Oldindan talab:**
- Kamida Developer roli bo'lishi kerak.

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. **Wiki harakatlari** (⚙) ni, so'ngra **Shablonlar**ni tanlang.
4. **Yangi shablon**ni tanlang.
5. Oddiy wiki sahifasi yaratgandek shablon sarlavhasi, format va kontentini kiriting.

Ma'lum formatdagi shablonlar faqat bir xil formatdagi sahifalarga qo'llanilishi mumkin.

### Shablon qo'llash

Wiki sahifasini yaratayotganda yoki tahrirlayotganda shablon qo'llashingiz mumkin.

**Oldindan talab:**
- Kamida bitta shablon yaratgan bo'lishingiz kerak.

1. **Kontent** bo'limida **Shablon tanlash** ochiladigan ro'yxatini tanlang.
2. Ro'yxatdan shablonni tanlang.
3. **Shablonni qo'llash**ni tanlang.

## Wiki sahifasining tarixini ko'rish

Wiki sahifasining vaqt o'tishi bilan o'zgarishlari wikining Git omborida yozib olinadi. Tarix sahifasi quyidagilarni ko'rsatadi:

- Sahifaning versiyasi
- Sahifa muallifi
- Commit xabari
- Oxirgi yangilanish
- Oldingi versiyalar

Wiki sahifasining o'zgarishlarini ko'rish uchun:

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. Tarixini ko'rmoqchi bo'lgan sahifaga o'ting.
4. **Wiki harakatlari** (⚙) ni, so'ngra **Sahifa tarixi**ni tanlang.

## Yon panel

Wiki sahifalari wikidagi sahifalar ro'yxatini o'z ichiga olgan yon panelni ko'rsatadi. Bu ro'yxat ichki daraxt ko'rinishida ko'rsatiladi, qardosh sahifalar alifbo tartibida joylashtiriladi.

Yon paneldagi qidiruv qutisi yordamida sahifani sarlavhasi bo'yicha tezda topishingiz mumkin.

Ishlash sabablari tufayli yon panel 5000 ta yozuvgacha ko'rsatish bilan cheklangan. Barcha sahifalar ro'yxatini ko'rish uchun yon panelda **Barcha sahifalarni ko'rish**ni tanlang.

### Yon panelni sozlash

Yon panel navigatsiyasining kontentini qo'lda tahrirlashingiz mumkin.

**Oldindan talab:**
- Kamida Developer roli bo'lishi kerak.

Bu jarayon `_sidebar` nomli wiki sahifasini yaratadi va standart yon panel navigatsiyasini to'liq almashtiradi:

1. Chap yon panelda **Qidirish**ni tanlang yoki o'tib, loyiha yoki guruhingizni toping.
2. **Reja > Wiki**ni tanlang.
3. Sahifaning yuqori o'ng burchagida **Maxsus yon panel qo'shish** (⚙) ni tanlang.
4. Tugagach, **O'zgarishlarni saqlash**ni tanlang.

## Loyiha wikisini yoqish yoki o'chirish

Wikilar GitLab-da standart ravishda yoqilgan. Loyiha administratorlari "Ulashish va ruxsatlar"dagi ko'rsatmalarga amal qilgan holda loyiha wikisini yoqishi yoki o'chirishi mumkin.

GitLab Self-Managed administratorlari qo'shimcha wiki sozlamalarini sozlashi mumkin.

Guruh wikilarini guruh sozlamalaridan o'chirishingiz mumkin.

## Tashqi wikini bog'lash

Loyihaning chap yon paneliga tashqi wiki havolasini qo'shish uchun:

1. Chap yon panelda **Qidirish**ni tanlang va loyihangizni toping.
2. **Sozlamalar > Integratsiyalar**ni tanlang.
3. **Tashqi wiki**ni tanlang.
4. Tashqi wikingizning URL manzilini qo'shing.
5. Ixtiyoriy. **Sozlamalarni sinash**ni tanlang.
6. **O'zgarishlarni saqlash**ni tanlang.

Endi loyihangizning chap yon panelidan **Tashqi wiki** variantini ko'rishingiz mumkin.

## Loyiha wikisini o'chirish

Loyihaning ichki wikisini o'chirish uchun:

1. Chap yon panelda **Qidirish**ni tanlang va loyihangizni toping.
2. **Sozlamalar > Umumiy**ni tanlang.
3. **Ko'rinish, loyiha xususiyatlari, ruxsatlar**ni kengaytiring.
4. Pastga aylanib, **Wiki** tugmasini toping va uni o'chiring.
5. **O'zgarishlarni saqlash**ni tanlang.