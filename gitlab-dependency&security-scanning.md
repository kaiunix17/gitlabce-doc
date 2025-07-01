# GitLab'da Node.js loyihalari uchun Dependency va Xavfsizlik skanerlash bo'yicha qo'llanma

## 1. Umumiy tushuncha

### Dependency Scanning
**Dependency scanning** – bu loyiha bog'liqliklari (masalan, NPM, Yarn, pnpm paketlari) ichidagi oldindan ma'lum zaifliklarni aniqlash jarayonidir. 
GitLab'ning Dependency Scanning xizmati sizning ilovangizning kutubxona bog'liqliklarida xavfsizlik kamchiliklari bor-yo'qligini ishlab chiqish jarayonida aniqlaydi. 
Bu yondoshuv noto'g'ri va zaif kodni ishlab chiqarishga kiritilishidan oldin tuzatishga yordam beradi, natijada foydalanuvchi ma'lumotlari va biznes obro'sini himoya qiladi.

### SAST (Static Application Security Testing)
**SAST** – statik kod tahlili bo'lib, manba kodda zaifliklarni ishlab chiqarishdan oldin aniqlaydi. 
Masalan, GitLab SAST boshqaruvchisi har bir commit bilan kodni avtomatik skanerlab, zaif joylarni topadi va dasturchiga xato paydo bo'lishidan oldin xabar beradi. 
Bu jarayon dastur to'liq ishga tushirilishidan oldin hujum qilish mumkin bo'lgan nuqtalarni bartaraf etishga imkon beradi.

### DAST (Dynamic Application Security Testing)
**DAST** – ishlayotgan veb-ilova yoki API'ga avtomatik pen-test o'tkazish usuli. 
U ilovani tashqi tomondan "hacker" kabi tekshiradi va XSS, SQL inyektsiya, CSRF kabi zaifliklarni aniqlaydi. 
GitLab DAST ishga tushirilgan muhitda (masalan, staging serverida) skan o'tkazib, ishlab chiqarishga qo'yilishdan oldin haqiqiy hujumlarni simulyatsiya qiladi.

### Secret Detection (Maxfiy kalitlarni aniqlash)
**Secret Detection** – loyihaga kiritilgan istalgan maxfiy token, API kaliti yoki xost nomlarini tekshiruvchi jarayon. 
Loyiha kodiga tasodifan yoki bexosdan maxfiy ma'lumotlar commit qilinganida, ular GitLab tomonidan xavfsizlik hisobotida qayd etiladi. 
Masalan, GitLab Push Protection yoki Pipeline Secret Detection scanlari yordamida yuklangan commitdagi kalitlar aniqlanadi va ehtimoliy hujumlarning oldi olinadi.

## 2. GitLab'da xavfsizlik skanerlashni yoqish va sozlash

### Talablar va tayyorgarlik

GitLab Self-Managed muhitida xavfsizlik skanerlash funksiyalarini yoqish uchun, avvalo GitLab Runner konfiguratsiyasini tayyorlash kerak:

- **Runner turi**: Linux asosidagi GitLab Runner (docker yoki kubernetes executor)
- **Operativ xotira**: Kamida 4 GB tavsiya qilinadi
- **Qo'llab-quvvatlanmaydi**: Windows yoki noyob protsessor arxitekturali Runner'lar

### Template'lar yordamida sozlash

GitLab'da skanerlarni yoqishning eng oddiy yo'li – avtomatik tayyorlangan GitLab shablonlarini `.gitlab-ci.yml` ga qo'shishdir:

```yaml
# Dependency Scanning uchun
include:
  - template: Jobs/Dependency-Scanning.gitlab-ci.yml

# SAST uchun  
include:
  - template: Jobs/SAST.gitlab-ci.yml

# Secret Detection uchun
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml
```

### To'liq pipeline namunasi

```yaml
stages:
  - test

include:
  - template: Jobs/Dependency-Scanning.gitlab-ci.yml
  - template: Jobs/SAST.gitlab-ci.yml

# Dependency scanning ishini sozlash
dependency_scanning:
  script:
    - npm install # loyiha paketlarini o'rnating
    - npm audit --json > gl-dependency-scanning-report.json
  artifacts:
    paths:
      - gl-dependency-scanning-report.json
```

### GitLab Web Interface orqali sozlash

Agar `.gitlab-ci.yml` faylingiz bo'lmasa:
1. GitLab veb-interfeysida **Secure > Security Configuration** bo'limiga o'ting
2. **Configure with merge request** tugmasini bosing
3. Kerakli template'larni tanlang
4. Avtomatik merge request yaratiladi

### Security Policies

GitLab **Security Policies** xususiyatlari orqali guruh yoki loyihalar bo'yicha majburiy skanerlashlar belgilash mumkin:

- **Scan execution policy**: Barcha loyihalarda Dependency Scanningni majburiy qilish
- **Pipeline security policies**: Ma'lum tarmoqlarda yoki holatlarda skanerlash qoidalari

## 3. Node.js loyihalari uchun dependency va security skanerlash

### GitLab Dependency Scanning

Node.js loyihalarida GitLab Dependency Scanning quyidagi fayllarni tahlil qiladi:
- `package-lock.json`
- `npm-shrinkwrap.json`
- `yarn.lock`
- `pnpm-lock.yaml`

Skanerlash avtomatik ravishda barcha bog'liqliklarni, shu jumladan transitive (ichki) bog'liqliklarni tekshiradi va CVE ma'lumotlar bazasidagi zaifliklarni qidiradi.

### GitLab SAST Node.js uchun

Hozirgi kunda **Semgrep** asosidagi GitLab Advanced SAST JavaScript va TypeScript kodini skanerlaydi. Bu vosita:
- Common zaifliklar (CWE) bo'yicha qidiruvlar olib boradi
- Siyosatlar asosida kod tahlil qiladi
- Community ESLint kabi ochiq kodli vositalar bilan integratsiya qilish mumkin

### NPM va Yarn integratsiyasi

Xavfsizlik skanerlashni amalga oshirish uchun `npm audit` buyruqlarini CI skriptida chaqirish mumkin:

```yaml
test_dependencies:
  stage: test
  script:
    - npm install
    - npm audit --audit-level high
  allow_failure: false
```

### Snyk bilan integratsiya

```yaml
snyk_scan:
  stage: test
  script:
    - npm install -g snyk
    - snyk auth $SNYK_TOKEN
    - snyk test
    - snyk test --json > snyk_data_file.json
    # Python skripti bilan GitLab formatiga o'girish
    - python convert_to_gitlab_format.py
  artifacts:
    reports:
      dependency_scanning: gl-dependency-scanning-report.json
```

### Uchinchi tomon vositalari

GitLab ichki vositalari bilan birga qo'shimcha skanerlash vositalarini kiritish mumkin:
- **OWASP Dependency Check**
- **Snyk**
- **WhiteSource Bolt**

Bu vositalar Docker konteyner shaklida pipeline'ga qo'shiladi va natijalarni SARIF yoki GitLab formatida beradi.

## 4. CI/CD pipeline'ga integratsiya

### Asosiy konfiguratsiya

```yaml
stages:
  - build
  - test

include:
  - template: Jobs/SAST.gitlab-ci.yml
  - template: Jobs/Dependency-Scanning.gitlab-ci.yml
  - template: Jobs/Secret-Detection.gitlab-ci.yml

build_app:
  stage: build
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test_app:
  stage: test
  script:
    - npm test
  dependencies:
    - build_app
```

### Natijalarni ko'rish

Pipeline tugagach:
1. **Build > Pipelines** bo'limiga o'ting
2. Tegishli pipeline'ni tanlang
3. **Security** tab'ini oching
4. Topilgan zaifliklarni ko'ring

### Zaifliklarni tuzatish

GitLab har bir topilgan muammo uchun quyidagilarni taklif etadi:
- **Issue** yoki **Merge Request** yaratish
- **False-positive** sifatida belgilash
- **Hal qilingan** deb belgilash
- **AI yordamida** avtomatik tuzatish takliflari (GitLab 18+)

### Merge Request widget

GitLab Ultimate versiyasida har bir commit va merge request'da zaifliklar avtomatik ko'rinadi va xavfsizlik vidjetida ko'rsatiladi.

## 5. Xavfsizlik va muvofiqlik (compliance) talablari

### Security Dashboard

GitLab bir platformada turli skanerlash natijalarini markazlashtiradi:
- **Security Dashboard**: Umumiy ko'rinish
- **Vulnerability Report**: Batafsil hisobotlar
- **Merge Request widget**: Real-time xabarlar

### Audit va hisobotlar

**Vulnerability Report** sahifasida:
- CVSS ballari
- CVE ma'lumotlari
- Tavsiya etilgan yechimlar
- Issue/merge request bog'lanishlari

**Eksport imkoniyatlari**:
- CSV formatida
- JSON formatida
- Tashqi auditlar uchun

### Compliance Frameworks

GitLab'da xavfsizlik siyosatlarini belgilash:
- **PCI-DSS** standartlari
- **HIPAA** talablari
- **Custom compliance** qoidalar

**Scan Execution Policies** orqali:
- Majburiy skanerlash turlari
- Flexible policy management
- Dasturchilar tomonidan o'tkazib yuborilmaydigan tekshiruvlar

## 6. Qo'shimcha resurslar va amaliy maslahatlar

### GitLab rasmiy hujjatlari

Batafsil ma'lumot uchun GitLab Documentation saytining Application Security bo'limiga murojaat qiling:
- **Dependency Scanning** sahifasi
- **SAST** sahifasi
- **DAST** sahifasi
- **Secret Detection** sahifasi
- **Tutorial: Dependency Scanning** - Node.js loyihasida amaliy misollar

### Yaxshi amaliyotlar (Best Practices)

1. **Doimiy yangilanish**: Bog'liqliklarni muntazam yangilab boring
2. **Erta skanerlash**: `npm audit` yoki `yarn audit` vositalarini CI/CD da ishlating
3. **False-positive boshqaruvi**: Kerakmas sozlamalarni belgilang:
   ```yaml
   variables:
     SAST_EXCLUDED_PATHS: "spec,test,tests,tmp"
     DS_EXCLUDED_PATHS: "spec,test,tests,tmp"
   ```
4. **Keng qamrovli tekshiruv**: Faqat merge pipeline emas, balki muntazam Security Dashboard'dan ham kuzatib boring

### Alternativ vositalar

GitLab ichki skanerlaridan tashqari:

| Vosita | Afzalliklari | Kamchiliklari |
|--------|--------------|---------------|
| **Snyk** | Ko'p qo'llab-quvvatlash, obuna asosida | Pullik xizmat |
| **OWASP Dependency Check** | Bepul, keng auditoriya | Qo'shimcha sozlash kerak |
| **WhiteSource Bolt** | Enterprise xususiyatlari | Murakkab sozlash |

### SecDevOps madaniyati

Xavfsizlikni ta'minlash - texnik usuldan ko'ra jarayon va madaniy masala:

1. **Ta'lim**: Dasturchilarni xavfsizlik bo'yicha o'qitish
2. **Code Review**: Kod ko'rib chiqishda xavfsizlikka e'tibor qaratish
3. **Avtomatlashtirish**: Skanerlashni joriy etib barqaror muhit yaratish
4. **Monitoring**: Natijalarni muntazam tekshirish
5. **Tuzatish**: Aniqlangan zaifliklarni issue/merge request orqali hal qilish

### Amaliy maslahatlar

- **Kichik loyihalar uchun**: GitLab ichki template'laridan foydalaning
- **Katta loyihalar uchun**: Uchinchi tomon vositalari bilan integratsiya qiling
- **Enterprise muhit uchun**: Security policies va compliance framework'larni sozlang
- **Jamoaviy ish uchun**: Security Dashboard va Merge Request widget'larini faol ishlating

## Xulosa

Yuqoridagi tavsiyalar va misollar Node.js loyihangizni GitLab Self-Managed muhitida xavfsizlik bo'yicha yetakchi amaliyotlarga muvofiq skanerlashda yo'l-yo'riq beradi. Har bir loyiha alohida, shuning uchun GitLab'ning turli opsiyalaridan foydalanib, o'zingizga eng mos konfiguratsiyani tanlang.

**Manbalar**: GitLab'ning rasmiy hujjatlari va blog maqolalari xavfsizlik skanerlash konseptini va amaliy sozlamalarni tasdiqlaydi.