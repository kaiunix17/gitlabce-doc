# GitLab Artefakt 

- Qanay yaratiladi
- Qaanqa configlari bor
- Eskirtirish mexanizmi
- Accesslarni boshqarish
- Qayerda saqlaydi
- Backup olish
- Developerlarga access berish
- Global artefakt konfiguratsiyasi

shu tepadagi savollar buyicha javoblarni topamiz

## Berilgan .gitlab-ci.yml Konfiguratsiyasi

## artifacts:paths
Qaysi filelar yoki folderlar artifact qilib saqlash uchun kerak.

**Misol:**
```yaml
job:
  artifacts:
    paths:
      - binaries/
      - .config
```

## artifacts:exclude
Filelarni artifactga qushmaslik uchun foydalaniladi

**Misol:**
```yaml
artifacts:
  paths:
    - binaries/
  exclude:
    - binaries/logs/*-test.log
```

## artifacts:expire_in
Artifactlar qancha vaqt saqlanib turishini belgilaymiz

'42', `42 seconds`, `3 mins 4 sec`, `2 hrs 20 min`, `1 week`, `never`

**Misol:**
```yaml
job:
  artifacts:
    expire_in: 1 week
```

- Vaqt artifact yuklanganda boshlanadi
- Vaqt belgilanmasa, 30 kun davomida saqlanadi keyin o'chirib tashlanadi
- "never" qo'yilsa, hech qachon o'chmaydi

## artifacts:name
Yaratilgan artifact nomini belgilaydi.

**Misol:**
```yaml
job:
  artifacts:
    name: "job1-artifacts-file"
    paths:
      - binaries/
```

- artifacts:name bilan ishlatilmasa, fayl nomi "artifacts" bo'ladi va yuklab olinganda "artifacts.zip" bo'ladi

## artifacts:public
Job artifactlari ommaga ochiq bo'lishi kerekligini belgilaydi.

**Qo'llab-quvvatlanadigan qiymatlar:**
- true (standart) yoki false

**Misol:**
```yaml
job:
  artifacts:
    public: false
```

## artifacts:access
GitLab UI yoki API orqali job artifactlariga kim kirishi mumkinligini belgilaydi.

- `all` (standart): Hamma yuklab olishi mumkin
- `developer`: Faqat Developer roli va yuqoriroq foydalanuvchilar
- `none`: Hech kim yuklab ololmaydi

**Misol:**
```yaml
job:
  artifacts:
    access: 'developer'
```

## artifacts:when
Qachon artifactlarni yuklashni belgilaydi.

**Qo'llab-quvvatlanadigan qiymatlar:**
- `on_success` (standart): Faqat job muvaffaqiyatli bo'lganda
- `on_failure`: Faqat job muvaffaqiyatsiz bo'lganda
- `always`: Har doim (timeout bundan mustasno)

**Misol:**
```yaml
job:
  artifacts:
    when: on_failure
```

Artifact quyidagicha yoziladi configuratsiyasi har bir stage uchun alohida artifact yozamiz.

```yaml
stages:
  - test # stageni belgilaymiz nechta stage buladi shuni

test-runner: # job name belgilaymiz
  stage: test
  script: # qanaqa script buyerda filelarni kuramiz yani artifact filelarini sinab kurish uchun
    - echo "GitLab Runner ishlayapti!" > test.txt
    - echo "Test xatosi" > error.txt
    - mkdir outputs
    - cp *.txt outputs/
  artifacts: # artifacts create qilamiz 
    name: "test-artifacts" # artifact name
    paths: 
      - outputs/ # outputs papkasi saqlanadi
    expire_in: 1 week # 1hafta saqlanadi
    when: always # fail or succes buganda ikkala holatda ham artifactga qushadi
```

## 1. Artefaktlar Qanday Yaratiladi

`Artefaktlar` — bu CI/CD natijasida yaratilgan fayllar yoki papkalar keyinchalik ularni download qilish uchun, joblar o'rtasida almashish yoki arxivlash uchun ishlatsa buladi.

`Artefaktlar` .gitlab-ci.yml artifacts kalit so'zi yordamida belgilanadi.

**Misol:**

```yaml
test-runner:
  stage: test
  script:
    - echo "GitLab Runner ishlayapti!" > test.txt
    - echo "Test xatosi" > error.txt
    - mkdir outputs
    - cp *.txt outputs/
  artifacts:
    paths:
      - outputs/
```

Bu `test-runner` job `test.txt` va `error.txt` fayllarini yaratadi, ularni `outputs/` papkasiga ko'chiradi 
va `outputs/` va `test_log.txt` fayllarini artefakt sifatida belgilaydi.


**Eslatma:** Konfiguratsiyada `test_log.txt` fayli `artifacts: paths` da ko'rsatilgan, ammo `script` bo'limida yaratilmagan, shuning uchun agar u mavjud bo'lmasa, e'tiborga olinmaydi.

### Kengaytirilgan Misol (Build Bosqichi bilan):

`build` bosqichi bilan artefakt yaratishni namoyish qilish uchun `build-job` qo'shamiz:

```yaml
build-job:
  stage: build
  script:
    - mkdir build
    - echo "Build yakunlandi" > build/app.txt
    - echo "Build log" > build/build_log.txt
  artifacts:
    paths:
      - build/
```

- `build-job` `build/` papkasini yaratadi va unda `app.txt` va `build_log.txt` fayllarini hosil qiladi.
- `artifacts: paths` direktivasi butun `build/` papkasini saqlaydi.
- Vazifa bajarilgandan so'ng, bu fayllar GitLab UI'da (CI/CD > Jobs) yuklab olish uchun mavjud bo'ladi.

## 2. Artifact configure

### Kengaytirilgan .gitlab-ci.yml:

```yaml
.default-artifacts:
  artifacts:
    name: "artifacts-$CI_JOB_NAME-$CI_COMMIT_REF_NAME"
    paths:
      - outputs/
    exclude:
      - outputs/error*.txt
    expire_in: 1 week
    untracked: true
    when: always

stages:
  - build
  - test

build-job:
  extends: .default-artifacts
  stage: build
  script:
    - mkdir -p outputs
    - echo "Build yakunlandi" > outputs/app.txt
    - echo "Build log" > build_log.txt
    - echo "Build xatosi" > outputs/error_build.txt
  artifacts:
    paths:
      - outputs/
      - build_log.txt
    exclude:
      - outputs/error_build.txt

test-runner:
  extends: .default-artifacts
  stage: test
  needs:
    - job: build-job
      artifacts: true
  script:
    - echo "GitLab Runner ishlayapti!" > test.txt
    - echo "Test xatosi" > error_test.txt
    - echo "Test log" > test_log.txt
    - mkdir -p outputs
    - cp test.txt outputs/
    - cp error_test.txt outputs/
  artifacts:
    paths:
      - outputs/
      - test_log.txt
    exclude:
      - outputs/error_test.txt
```

### Konfiguratsiya Imkoniyatlari:

- **name:** Artefakt arxivining nomini belgilaydi. `$CI_JOB_NAME` va `$CI_COMMIT_REF_NAME` yordamida dinamik nomlar yaratiladi (masalan, `artifacts-build-job-main`).
- **paths:** Saqlanadigan fayl yoki papkalarni ro'yxatga oladi (masalan, `outputs/` va `test_log.txt`).
- **exclude:** Muayyan fayl yoki naqshlarni chiqarib tashlaydi (masalan, `outputs/error*.txt` xato fayllarini chiqaradi).
- **expire_in:** Artefaktning saqlanish muddatini belgilaydi (masalan, `1 week`).
- **untracked:** Git tomonidan kuzatilmaydigan fayllarni qo'shadi (masalan, `true` `outputs/` dagi kuzatilmaydigan fayllarni qamrab oladi).
- **when:** Artefaktlar qachon yuklanishini belgilaydi (`always`, `on_success`, yoki `on_failure`).
- **needs:** Oldingi vazifalardan artefaktlarni olish imkonini beradi (masalan, `test-runner` `build-job` artefaktlarini ishlatadi).

## 3. Eskirish Mexanizmi

**Ta'rifi:** Artefaktlar saqlash joyini boshqarish uchun belgilangan muddatdan so'ng avtomatik o'chiriladi.
- `expire_in` kalit so'zi muddatni belgilaydi (masalan, `1 day`, `1 week`, `1 month`, yoki `never`).

```yaml
expire_in: 1 week
```

Artefaktlar 7 kundan keyin o'chiriladi. Artefaktlarni doimiy saqlash uchun:

1. GitLab UI'da CI/CD > Jobs ga o'ting.
2. Vazifani tanlang va "Keep" tugmasini bosing.

### Avtomatik Saqlashni O'chirish:

Eng so'nggi pipeline artefaktlarini saqlashni o'chirish uchun:

1. Settings > CI/CD ga o'ting.
2. "Keep artifacts from most recent successful jobs" belgisini olib tashlang.

## 4. Kirish Huquqlarini Boshqarish

Artifactni ishlatish bu rollarga bogliq va ularni permissionlariga bogliq

### Rollar va Ruxsatlar:

- **Guest:** Agar loyiha umumiy bo'lsa yoki Guest'ga ruxsat berilgan bo'lsa, ko'rish va yuklab olish mumkin.
- **Reporter:** Artefaktlarni ko'rish va yuklab olish mumkin.
- **Developer:** Ko'rish, yuklab olish va pipeline'ni ishga tushirish mumkin.
- **Maintainer:** Qo'shimcha ravishda artefaktlarni o'chirish huquqiga ega.

## 5. Saqlash Joyi

**Ta'rifi:** Artefaktlar GitLab serverining lokal diskida yoki tashqi ob'ekt saqlash xizmatida saqlanadi.

**Lokal Disk va Docker:** O'z serverida o'rnatilgan GitLab uchun standart, odatda `/var/opt/gitlab/gitlab-rails/shared/artifacts` papkasida.

Saqlash papkasini tekshirish:
```bash
ls /var/opt/gitlab/gitlab-rails/shared/artifacts
```

O'zgarishlarni qo'llash:
```bash
sudo gitlab-ctl reconfigure
```

## 6. Zaxira Olish Jarayoni

**Ta'rifi:** Zaxiralar artefaktlarni tiklash uchun saqlanadi.

### Zaxira Usullari:

1. **Lokal Saqlash:**
    - Artefaktlar GitLabning standart zaxirasiga kiritiladi.
    - Buyruq:
      ```bash
      sudo gitlab-backup create
      ```
    - Zaxiralar `/var/opt/gitlab/backups` papkasida saqlanadi.

**Misol:**
- Lokal saqlash uchun zaxira buyruqni ishlatgandan so'ng `/var/opt/gitlab/backups` da zaxira faylini tekshiring.
- S3 uchun muntazam sinxronlashni rejalashtiring.

## 7. Dasturchilarga Kirish/Login Berish

**Ta'rifi:** Dasturchilar artefaktlarga GitLab UI yoki API orqali, loyiha rollariga qarab kirishlari mumkin.

### Usullar:

1. **UI orqali Kirish:**
    - CI/CD > Jobs ga o'ting, vazifani tanlang va artefaktni (masalan, `test-artifacts.zip`) yuklab oling.
    - Kamida Reporter roli talab qilinadi.

2. **API orqali Kirish:**
    - User Settings > Access Tokens dan shaxsiy kirish tokenini oling.
    - Artefaktlarni yuklash uchun:
      ```bash
      curl --header "PRIVATE-TOKEN: <your_access_token>" \
      "https://gitlab.example.com/api/v4/projects/<project_id>/jobs/<job_id>/artifacts" \
      -o artifacts.zip
      ```