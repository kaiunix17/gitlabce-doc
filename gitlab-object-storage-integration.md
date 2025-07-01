### Ob'ekt saqlash
**Daraja:** Bepul, Premium, Ultimate  
**Taklif:** GitLab o'z-o'zini boshqaradigan

GitLab ko'plab ma'lumot turlarini saqlash uchun ob'ekt saqlash xizmatidan foydalanishni qo'llab-quvvatlaydi. Bu NFS-ga nisbatan tavsiya etiladi va umuman olganda, katta tizimlarda ob'ekt saqlash odatda ancha samarali, ishonchli va kengaytiriladigan hisoblanadi.

Ob'ekt saqlashni sozlash uchun ikkita variant mavjud:

1. **Tavsiya etiladi.** Barcha ob'ekt turlari uchun yagona saqlash ulanishini sozlash: Barcha qo'llab-quvvatlanadigan ob'ekt turlari uchun yagona hisob ma'lumotlari ishlatiladi. Bu **konsolidatsiyalangan shakl** deb ataladi.
2. Har bir ob'ekt turi uchun alohida saqlash ulanishini belgilash: Har bir ob'ekt o'zining saqlash ulanishi va konfiguratsiyasini aniqlaydi. Bu **saqlashga xos shakl** deb ataladi.

Agar siz allaqachon saqlashga xos shakldan foydalanayotgan bo'lsangiz, konsolidatsiyalangan shaklga o'tish bo'yicha ko'rsatmalarni ko'ring.  
Agar ma'lumotlar mahalliy saqlanayotgan bo'lsa, ob'ekt saqlashga o'tish bo'yicha ko'rsatmalarni ko'ring.

### Qo'llab-quvvatlanadigan ob'ekt saqlash provayderlari
GitLab Fog kutubxonasi bilan mahkam integratsiyalangan, shuning uchun GitLab bilan qaysi provayderlardan foydalanish mumkinligini ko'rishingiz mumkin.

Xususan, GitLab quyidagi ob'ekt saqlash provayderlarida sinovdan o'tkazilgan:

- **Amazon S3** (Ob'ekt blokirovkasi qo'llab-quvvatlanmaydi, qo'shimcha ma'lumot uchun #335775 masalasiga qarang)
- **Google Cloud Storage**
- **Digital Ocean Spaces** (S3-ga mos)
- **Oracle Cloud Infrastructure**
- **OpenStack Swift** (S3-ga mos rejim)
- **Azure Blob storage**
- **MinIO** (S3-ga mos)
- Mahalliy apparat va turli xil saqlash sotuvchilarining qurilmalari, ularning ro'yxati rasmiy ravishda aniqlanmagan.

### Barcha ob'ekt turlari uchun yagona saqlash ulanishini sozlash (konsolidatsiyalangan shakl)
CI artefaktlari, LFS fayllari va yuklangan ilovalar kabi ob'ektlarning ko'p turlari bir nechta chelaklar bilan yagona hisob ma'lumotlari yordamida ob'ekt saqlashda saqlanishi mumkin.

GitLab Helm jadvallari uchun konsolidatsiyalangan shaklni sozlash bo'yicha ko'rsatmalarni ko'ring.

Konsolidatsiyalangan shaklni sozlashning bir qator afzalliklari mavjud:

- Ulanish tafsilotlari ob'ekt turlari orasida umumiy bo'lgani uchun GitLab konfiguratsiyasini soddalashtiradi.
- S3 shifrlangan chelaklardan foydalanishni yoqadi.
- Fayllarni S3-ga Content-MD5 sarlavhalari bilan yuklaydi.
  Konsolidatsiyalangan shakl ishlatilganda, to'g'ridan-to'g'ri yuklash avtomatik ravishda yoqiladi. Shunday qilib, faqat quyidagi provayderlar ishlatilishi mumkin:

- Amazon S3-ga mos provayderlar
- Google Cloud Storage
- Azure Blob storage

Konsolidatsiyalangan shakl konfiguratsiyasi zaxira yoki Mattermost uchun ishlatilmaydi. Zaxira server tomonidan shifrlash bilan alohida sozlanishi mumkin. Qo'llab-quvvatlanadigan ob'ekt saqlash turlari ro'yxatini jadvaldan ko'ring.

Konsolidatsiyalangan shaklni yoqish barcha ob'ekt turlari uchun ob'ekt saqlashni faollashtiradi. Agar barcha chelaklar ko'rsatilmagan bo'lsa, quyidagi kabi xato xabari paydo bo'lishi mumkin:

```
Ob'ekt saqlash uchun <ob'ekt turi> uchun chelak ko'rsatilishi kerak
```

Agar ma'lum ob'ekt turlari uchun mahalliy saqlashdan foydalanmoqchi bo'lsangiz, muayyan funksiyalar uchun ob'ekt saqlashni o'chirib qo'yishingiz mumkin.

### Umumiy parametrlarni sozlash
Konsolidatsiyalangan shaklda `object_store` bo'limi umumiy parametrlarni aniqlaydi.

| Sozlama               | Tavsif                                                                 |
|-----------------------|------------------------------------------------------------------------------|
| **enabled**           | Ob'ekt saqlashni yoqish yoki o'chirish.                                      |
| **proxy_download**    | Agar `true` bo'lsa, barcha fayllarni proksi orqali yuklab olishni yoqadi. Bu mijozlarga to'g'ridan-to'g'ri masofaviy saqlashdan yuklab olish imkonini beradi. |
| **connection**        | Quyida tasvirlangan turli xil ulanish sozlamalari.                          |
| **storage_options**   | Yangi ob'ektlarni saqlashda ishlatiladigan sozlamalar, masalan, server tomonidan shifrlash. |
| **objects**           | Ob'ektga xos konfiguratsiya.                                                |

Masalan, konsolidatsiyalangan shakl va Amazon S3 bilan qanday ishlatilishini ko'ring.

### Har bir ob'ektning parametrlarni sozlash
Har bir ob'ekt turi kamida o'zi saqlanadigan chelak nomini aniqlashi kerak.

Quyidagi jadvalda ishlatilishi mumkin bo'lgan ob'ekt turlari keltirilgan:

| Tur                   | Tavsif                              |
|-----------------------|-------------------------------------|
| **artifacts**         | CI/CD ish artefaktlari             |
| **external_diffs**    | Birlashtirish so'rovlari farqlari  |
| **uploads**           | Foydalanuvchi yuklamalari          |
| **lfs**               | Git Large File Storage ob'ektlari  |
| **packages**          | Loyiha paketlari (masalan, PyPI, Maven, NuGet) |
| **dependency_proxy**  | Bog'liqlik proksisi                |
| **terraform_state**   | Terraform holat fayllari           |
| **pages**             | Sahifalar                          |
| **ci_secure_files**   | Xavfsiz fayllar                    |

Har bir ob'ekt turi uchun uchta parametr aniqlanishi mumkin:

| Sozlama           | Majburiy? | Tavsif                              |
|-------------------|-----------|-------------------------------------|
| **bucket**        | Ha*       | Ob'ekt turi uchun chelak nomi. Agar `enabled` `false` bo'lsa, talab qilinmaydi. |
| **enabled**       | Yo'q      | Umumiy parametrni bekor qiladi.     |
| **proxy_download**| Yo'q      | Umumiy parametrni bekor qiladi.     |

Masalan, konsolidatsiyalangan shakl va Amazon S3 bilan qanday ishlatilishini ko'ring.

### Muayyan funksiyalar uchun ob'ekt saqlashni o'chirish
Oldin ko'rilganidek, muayyan turlar uchun ob'ekt saqlashni `enabled` bayrog'ini `false` ga o'rnatish orqali o'chirish mumkin. Masalan, CI artefaktlari uchun ob'ekt saqlashni o'chirish uchun:

```ruby
gitlab_rails['object_store']['objects']['artifacts']['enabled'] = false
```

Agar funksiya butunlay o'chirilgan bo'lsa, chelak kerak emas. Masalan, CI artefaktlari quyidagi sozlama bilan o'chirilgan bo'lsa, chelak talab qilinmaydi:

```ruby
gitlab_rails['artifacts_enabled'] = false
```

### Har bir ob'ekt turi uchun alohida saqlash ulanishini sozlash (saqlashga xos shakl)
Saqlashga xos shaklda har bir ob'ekt o'zining ob'ekt saqlash ulanishi va konfiguratsiyasini aniqlaydi. Konsolidatsiyalangan shakldan foydalanish tavsiya etiladi, faqat konsolidatsiyalangan shakl tomonidan qo'llab-quvvatlanmaydigan saqlash turlari bundan mustasno. GitLab Helm jadvallari bilan ishlashda, ob'ekt saqlash uchun konsolidatsiyalangan shaklni qanday boshqarishini ko'ring.

Shifrlangan S3 chelaklarini konsolidatsiyalanmagan shakl bilan ishlatish qo'llab-quvvatlanmaydi. Agar shunday qilsangiz, ETag mos kelmaslik xatolari paydo bo'lishi mumkin.

Saqlashga xos shakl uchun to'g'ridan-to'g'ri yuklash standart bo'lishi mumkin, chunki u umumiy papkani talab qilmaydi.

Konsolidatsiyalangan shakl tomonidan qo'llab-quvvatlanmaydigan saqlash turlari uchun quyidagi qo'llanmalarga qarang:

| Ob'ekt saqlash turi               | Konsolidatsiyalangan shakl tomonidan qo'llab-quvvatlanadimi? |
|-----------------------------------|-------------------------------------------------------------|
| Zaxira                           | Yo'q                                                       |
| Konteyner reestri (ixtiyoriy funksiya) | Yo'q                                                       |
| Mattermost                       | Yo'q                                                       |
| Avtomatik masshtablash yuguruvchi kesh (samara oshirish uchun ixtiyoriy) | Yo'q                                                       |
| Xavfsiz fayllar                  | Ha                                                         |
| Ish artefaktlari, shu jumladan arxivlangan ish jurnallari | Ha                                                         |
| LFS ob'ektlari                   | Ha                                                         |
| Yuklamalar                       | Ha                                                         |
| Birlashtirish so'rovlari farqlari | Ha                                                         |
| Paketlar (ixtiyoriy funksiya)    | Ha                                                         |
| Bog'liqlik proksisi (ixtiyoriy funksiya) | Ha                                                         |
| Terraform holat fayllari         | Ha                                                         |
| Sahifalar mazmuni                | Ha                                                         |

### Ulanish sozlamalarini sozlash
Konsolidatsiyalangan va saqlashga xos shakllar uchun ulanish sozlanishi kerak. Quyidagi bo'limlarda ulanish sozlamalarida ishlatilishi mumkin bo'lgan parametrlar tasvirlanadi.

#### Amazon S3
Ulanish sozlamalari fog-aws tomonidan taqdim etilganlarga mos keladi:

| Sozlama                           | Tavsif                                                                 | Standart qiymat |
|-----------------------------------|----------------------------------------------------------------------|---------------|
| **provider**                     | Mos keluvchi xostlar uchun doimo AWS.                                | AWS           |
| **aws_access_key_id**            | AWS hisob ma'lumotlari yoki mos keluvchilar.                         |               |
| **aws_secret_access_key**        | AWS hisob ma'lumotlari yoki mos keluvchilar.                         |               |
| **aws_signature_version**        | Ishlatiladigan AWS imzo versiyasi. 2 yoki 4 mumkin. Digital Ocean Spaces va boshqa provayderlar uchun 2 kerak bo'lishi mumkin. | 4             |
| **enable_signature_v4_streaming**| HTTP chunked o'tkazmalarni AWS v4 imzolarni bilan yoqish uchun `true` qiling. Oracle Cloud S3 uchun bu `false` bo'lishi kerak. GitLab 17.4 da standart `true` dan `false` ga o'zgardi. | false         |
| **region**                       | AWS mintaqasi.                                                      |               |
| **host**                         | O'CHIRILGAN: O'rniga `endpoint` ishlating. AWS bo'lmagan holatlar uchun S3-ga mos xost, masalan, `localhost` yoki `storage.example.com`. HTTPS va 443-port nazarda tutiladi. | s3.amazonaws.com |
| **endpoint**                     | MinIO kabi S3-ga mos xizmatni sozlashda ishlatiladi, masalan, `http://127.0.0.1:9000`. Bu `host` dan ustunlik qiladi. Konsolidatsiyalangan shakl uchun doimo `endpoint` ishlating. | (ixtiyoriy)   |
| **path_style**                   | `host/bucket_name/object` uslubidagi yo'llarni ishlatish uchun `true` qiling. MinIO uchun `true`, AWS S3 uchun `false`. | false         |
| **use_iam_profile**              | Kirish kalitlari o'rniga IAM profilidan foydalanish uchun `true` qiling. | false         |
| **aws_credentials_refresh_threshold_seconds** | IAM da vaqtinchalik hisob ma'lumotlaridan foydalanganda avtomatik yangilash chegarasini soniyalarda o'rnatadi. | 15            |
| **disable_imds_v2**              | X-aws-ec2-metadata-token ni olish uchun IMDS v2 endpointiga kirishni o'chirish orqali IMDS v1 ni majburlash. | false         |

#### Amazon instance profillaridan foydalanish
AWS kirish va maxfiy kalitlarni ob'ekt saqlash konfiguratsiyasida taqdim etish o'rniga, GitLab Amazon Identity Access and Management (IAM) rollaridan foydalanish uchun sozlanishi mumkin. Bu holatda, GitLab har bir S3 chelakka kirishda vaqtinchalik hisob ma'lumotlarini oladi, shuning uchun konfiguratsiyada qattiq kodlangan qiymatlar kerak emas.

**Old shartlar:**

- GitLab instans metadata endpointiga ulanishi kerak.
- Agar GitLab internet proksidan foydalansa, endpoint IP manzili `no_proxy` ro'yxatiga qo'shilishi kerak.
- IMDS v2 kirish uchun hop chegarasi yetarli bo'lishi kerak. Agar GitLab konteynerda ishlayotgan bo'lsa, chegarani 1 dan 2 ga ko'tarish kerak bo'lishi mumkin.

**Instans profilini sozlash uchun:**

1. Kerakli ruxsatlarga ega IAM rolini yarating. Quyidagi misol `test-bucket` nomli S3 chelak uchun rol:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::test-bucket/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::test-bucket"
        }
    ]
}
```

2. Ushbu rolni GitLab instansini joylashtirgan EC2 instansiga biriktiring.
3. GitLab konfiguratsiyasida `use_iam_profile` sozlamasini `true` ga o'rnating.

#### Shifrlangan S3 chelaklari
Instans profili yoki konsolidatsiyalangan shakl bilan sozlanganda, GitLab Workhorse SSE-S3 yoki SSE-KMS shifrlash yoqilgan S3 chelaklariga fayllarni to'g'ri yuklaydi. AWS KMS kalitlari va SSE-C shifrlash qo'llab-quvvatlanmaydi, chunki bu har bir so'rovda shifrlash kalitlarini yuborishni talab qiladi.

**Server tomonidagi shifrlash sarlavhalari**
S3 chelakda standart shifrlashni o'rnatish shifrlashni yoqishning eng oson yo'li, ammo faqat shifrlangan ob'ektlar yuklanishini ta'minlash uchun chelak siyosatini o'rnatishni xohlashingiz mumkin. Buning uchun GitLab-ni `storage_options` konfiguratsiya bo'limida to'g'ri shifrlash sarlavhalarini yuborish uchun sozlashingiz kerak:

| Sozlama                           | Tavsif                                                                 |
|-----------------------------------|----------------------------------------------------------------------|
| **server_side_encryption**        | Shifrlash rejimi (`AES256` yoki `aws:kms`).                          |
| **server_side_encryption_kms_key_id** | Amazon Resource Name. Faqat `aws:kms` ishlatilganda kerak. Amazon hujjatlariga qarang. |

Ushbu sozlamalar faqat Workhorse S3 mijozi yoqilgan bo'lsa ishlaydi. Quyidagi shartlardan biri bajarilishi kerak:

- Ulanish sozlamalarida `use_iam_profile` `true` bo'lishi kerak.
- Konsolidatsiyalangan shakl ishlatilishi kerak.

Agar server tomonidagi shifrlash sarlavhalari Workhorse S3 mijozi yoqilmagan holda ishlatilsa, ETag mos kelmaslik xatolari yuzaga keladi.

#### Oracle Cloud S3
Oracle Cloud S3 quyidagi sozlamalarni ishlatishi kerak:

| Sozlama                           | Qiymat |
|-----------------------------------|--------|
| **enable_signature_v4_streaming** | `false` |
| **path_style**                    | `true`  |

Agar `enable_signature_v4_streaming` `true` ga o'rnatilgan bo'lsa, `production.log` da quyidagi xato paydo bo'lishi mumkin:

```
STREAMING-AWS4-HMAC-SHA256-PAYLOAD qo'llab-quvvatlanmaydi
```

#### Google Cloud Storage (GCS)
GCS uchun haqiqiy ulanish parametrlari:

| Sozlama                           | Tavsif                              | Misol                               |
|-----------------------------------|-------------------------------------|-------------------------------------|
| **provider**                     | Provayder nomi.                    | Google                              |
| **google_project**               | GCP loyiha nomi.                   | gcp-project-12345                   |
| **google_json_key_location**     | JSON kalit yo'li.                  | /path/to/gcp-project-12345-abcde.json |
| **google_json_key_string**       | JSON kalit satri.                  | { "type": "service_account", "project_id": "example-project-382839", ... } |
| **google_application_default**   | Google Cloud Application Default Credentials dan foydalanish uchun `true` qiling. |                                     |

GitLab `google_json_key_location`, so'ngra `google_json_key_string`, va nihoyat `google_application_default` qiymatini o'qiydi. Birinchi qiymat topilgan sozlama ishlatiladi.

Xizmat hisobi chelakka kirish uchun ruxsatga ega bo'lishi kerak. Qo'shimcha ma'lumot uchun Cloud Storage autentifikatsiya hujjatlariga qarang.

#### Google Cloud Application Default Credentials
Google Cloud Application Default Credentials (ADC) odatda GitLab bilan standart xizmat hisobini yoki Workload Identity Federation ni ishlatish uchun qo'llaniladi. `google_application_default` ni `true` ga o'rnating va `google_json_key_location` va `google_json_key_string` ni o'tkazib yuboring.

Agar ADC ishlatilsa, quyidagilarni ta'minlang:

- Ishlatilayotgan xizmat hisobi `iam.serviceAccounts.signBlob` ruxsatiga ega bo'lishi kerak. Odatda bu xizmat hisobiga Service Account Token Creator rolini berish orqali amalga oshiriladi.
- Agar Google Compute virtual mashinalaridan foydalanayotgan bo'lsangiz, ular Google Cloud API lariga kirish uchun to'g'ri kirish doiralariga ega bo'lishi kerak. Agar mashinalarda to'g'ri doira bo'lmasa, xato jurnallarida quyidagi xabar paydo bo'lishi mumkin:

```
Google::Apis::ClientError (insufficientPermissions: So'rovda autentifikatsiya doiralari yetarli emas.)
```

Mijoz tomonidan boshqariladigan shifrlash kalitlari bilan chelak shifrlashdan foydalanish uchun konsolidatsiyalangan shakldan foydalaning.

#### Linux paketi (Omnibus)
`/etc/gitlab/gitlab.rb` ni tahrir qiling va kerakli qiymatlarni almashtirib, quyidagi qatorlarni qo'shing:

```ruby
gitlab_rails['object_store']['connection'] = {
 'provider' => 'Google',
 'google_project' => '<GOOGLE PROJECT>',
 'google_json_key_location' => '<FILENAME>'
}
```

ADC ishlatish uchun o'rniga `google_application_default` dan foydalaning:

```ruby
gitlab_rails['object_store']['connection'] = {
 'provider' => 'Google',
 'google_project' => '<GOOGLE PROJECT>',
 'google_application_default' => true
}
```

Faylni saqlang va GitLab-ni qayta sozlang:

```shell
sudo gitlab-ctl reconfigure
```

#### Azure Blob storage
Azure "container" so'zini bloblar to'plami uchun ishlatadi, lekin GitLab "chelak" atamasini standartlashtiradi. Azure konteyner nomlarini chelak sozlamalarida sozlashni unutmang.

Azure Blob storage faqat konsolidatsiyalangan shakl bilan ishlatilishi mumkin, chunki bir nechta konteynerlarga kirish uchun yagona hisob ma'lumotlari ishlatiladi. Saqlashga xos shakl qo'llab-quvvatlanmaydi. Qo'shimcha ma'lumot uchun konsolidatsiyalangan shaklga o'tish bo'yicha ko'rsatmalarni ko'ring.

Azure uchun haqiqiy ulanish parametrlari:

| Sozlama                           | Tavsif                                                                 | Misol                               |
|-----------------------------------|----------------------------------------------------------------------|-------------------------------------|
| **provider**                     | Provayder nomi.                                                     | AzureRM                             |
| **azure_storage_account_name**   | Saqlashga kirish uchun ishlatiladigan Azure Blob Storage hisob nomi. | azuretest                           |
| **azure_storage_access_key**     | Konteynerga kirish uchun saqlash hisobi kaliti. Odatda 512-bitli shifrlangan kalit base64 da kodlangan. Azure ish yuki va boshqariladigan identifikatorlar uchun ixtiyoriy. | czV2OHkvQj9FKEgrTWJRZVRoV21ZcTN0Nnc5eiRDJkYpSkBOY1JmVWpYbjJy\nNHU3eCFBJUQqRy1LYVBkU2dWaw==\n |
| **azure_storage_domain**         | Azure Blob Storage API bilan bog'lanish uchun ishlatiladigan domen nomi (ixtiyoriy). Standart: `blob.core.windows.net`. Azure China, Azure Germany, Azure US Government yoki boshqa maxsus Azure domenlari uchun o'rnating. | blob.core.windows.net |

#### Linux paketi (Omnibus)
`/etc/gitlab/gitlab.rb` ni tahrir qiling va kerakli qiymatlarni almashtirib, quyidagi qatorlarni qo'shing:

```ruby
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AzureRM',
  'azure_storage_account_name' => '<AZURE STORAGE ACCOUNT NAME>',
  'azure_storage_access_key' => '<AZURE STORAGE ACCESS KEY>',
  'azure_storage_domain' => '<AZURE STORAGE DOMAIN>'
}
gitlab_rails['object_store']['objects']['artifacts']['bucket'] = 'gitlab-artifacts'
gitlab_rails['object_store']['objects']['external_diffs']['bucket'] = 'gitlab-mr-diffs'
gitlab_rails['object_store']['objects']['lfs']['bucket'] = 'gitlab-lfs'
gitlab_rails['object_store']['objects']['uploads']['bucket'] = 'gitlab-uploads'
gitlab_rails['object_store']['objects']['packages']['bucket'] = 'gitlab-packages'
gitlab_rails['object_store']['objects']['dependency_proxy']['bucket'] = 'gitlab-dependency-proxy'
gitlab_rails['object_store']['objects']['terraform_state']['bucket'] = 'gitlab-terraform-state'
gitlab_rails['object_store']['objects']['ci_secure_files']['bucket'] = 'gitlab-ci-secure-files'
gitlab_rails['object_store']['objects']['pages']['bucket'] = 'gitlab-pages'
```

Agar ish yuki identifikatoridan foydalanayotgan bo'lsangiz, `azure_storage_access_key` ni o'tkazib yuboring:

```ruby
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AzureRM',
  'azure_storage_account_name' => '<AZURE STORAGE ACCOUNT NAME>',
  'azure_storage_domain' => '<AZURE STORAGE DOMAIN>'
}
```

Faylni saqlang va GitLab-ni qayta sozlang:

```shell
sudo gitlab-ctl reconfigure
```

#### Azure ish yuki va boshqariladigan identifikatorlar
Agar `azure_storage_access_key` bo'sh bo'lsa, GitLab quyidagilarni amalga oshirishga harakat qiladi:

1. Ish yuki identifikatori bilan vaqtinchalik hisob ma'lumotlarini olish. `AZURE_TENANT_ID`, `AZURE_CLIENT_ID` va `AZURE_FEDERATED_TOKEN_FILE` muhitda bo'lishi kerak.
2. Agar ish yuki identifikatori mavjud bo'lmasa, Azure Instance Metadata Service dan hisob ma'lumotlarini so'raydi.
3. Foydalanuvchi delegatsiya kalitini olish.
4. Saqlash hisobi blobiga kirish uchun ushbu kalit bilan SAS tokenini generatsiya qilish.

Identifikatorga `Storage Blob Data Contributor` roli tayinlangan bo'lishi kerak.

#### Storj Gateway (SJ)
Storj Gateway ko'p oqimli nusxalashni qo'llab-quvvatlamaydi (UploadPartCopy ga qarang). Bu amalga oshirilishi rejalashtirilgan, lekin tugallangunicha ko'p oqimli nusxalashni o'chirib qo'yish kerak.

Storj Network S3-ga mos API shlyuzini taqdim etadi. Quyidagi konfiguratsiya misolidan foydalaning:

```ruby
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AWS',
  'endpoint' => 'https://gateway.storjshare.io',
  'path_style' => true,
  'region' => 'eu1',
  'aws_access_key_id' => 'ACCESS_KEY',
  'aws_secret_access_key' => 'SECRET_KEY',
  'aws_signature_version' => 2,
  'enable_signature_v4_streaming' => false
}
```

Imzo versiyasi 2 bo'lishi kerak. 4-versiyadan foydalanish `HTTP 411 Length Required` xatosiga olib keladi. Qo'shimcha ma'lumot uchun #4419 masalasiga qarang.

#### Hitachi Vantara HCP
HCP ulanishlari `SignatureDoesNotMatch` xatosini qaytarishi mumkin. Bunday hollarda, `endpoint` ni namespace o'rniga tenant URL sifatida o'rnating va chelak yo'llarini `<namespace_name>/<bucket_name>` sifatida sozlang.

HCP S3-ga mos API ni taqdim etadi. Quyidagi konfiguratsiya misolidan foydalaning:

```ruby
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AWS',
  'endpoint' => 'https://<tenant_endpoint>',
  'path_style' => true,
  'region' => 'eu1',
  'aws_access_key_id' => 'ACCESS_KEY',
  'aws_secret_access_key' => 'SECRET_KEY',
  'aws_signature_version' => 4,
  'enable_signature_v4_streaming' => false
}

# <namespace_name/bucket_name> formatlash misoli
gitlab_rails['object_store']['objects']['artifacts']['bucket'] = '<namespace_name>/<bucket_name>'
```

#### Amazon S3 bilan konsolidatsiyalangan shaklning to'liq misoli
Quyidagi misol barcha qo'llab-quvvatlanadigan xizmatlar uchun ob'ekt saqlashni yoqish uchun AWS S3 dan foydalanadi:

```ruby
# Konsolidatsiyalangan ob'ekt saqlash konfiguratsiyasi
gitlab_rails['object_store']['enabled'] = true
gitlab_rails['object_store']['proxy_download'] = false
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AWS',
  'region' => 'eu-central-1',
  'aws_access_key_id' => '<AWS_ACCESS_KEY_ID>',
  'aws_secret_access_key' => '<AWS_SECRET_ACCESS_KEY>'
}
# Ixtiyoriy: Quyidagi qatorlar faqat server tomonidagi shifrlash talab qilinganda kerak
gitlab_rails['object_store']['storage_options'] = {
  'server_side_encryption' => '<AES256 or aws:kms>',
  'server_side_encryption_kms_key_id' => '<arn:aws:kms:xxx>'
}
gitlab_rails['object_store']['objects']['artifacts']['bucket'] = 'gitlab-artifacts'
gitlab_rails['object_store']['objects']['external_diffs']['bucket'] = 'gitlab-mr-diffs'
gitlab_rails['object_store']['objects']['lfs']['bucket'] = 'gitlab-lfs'
gitlab_rails['object_store']['objects']['uploads']['bucket'] = 'gitlab-uploads'
gitlab_rails['object_store']['objects']['packages']['bucket'] = 'gitlab-packages'
gitlab_rails['object_store']['objects']['dependency_proxy']['bucket'] = 'gitlab-dependency-proxy'
gitlab_rails['object_store']['objects']['terraform_state']['bucket'] = 'gitlab-terraform-state'
gitlab_rails['object_store']['objects']['ci_secure_files']['bucket'] = 'gitlab-ci-secure-files'
gitlab_rails['object_store']['objects']['pages']['bucket'] = 'gitlab-pages'
```

Agar AWS IAM profillaridan foydalanayotgan bo'lsangiz, AWS kirish kaliti va maxfiy kalit/qiymat juftliklarini o'tkazib yuboring:

```ruby
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AWS',
  'region' => 'eu-central-1',
  'use_iam_profile' => true
}
```

Faylni saqlang va GitLab-ni qayta sozlang:

```shell
sudo gitlab-ctl reconfigure
```

### Ob'ekt saqlashga o'tish
Mahalliy saqlangan ma'lumotlarni ob'ekt saqlashga o'tkazish uchun quyidagi qo'llanmalarga qarang:

- Ish artefaktlari, shu jumladan arxivlangan ish jurnallari
- LFS ob'ektlari
- Yuklamalar
- Birlashtirish so'rovlari farqlari
- Paketlar (ixtiyoriy funksiya)
- Bog'liqlik proksisi
- Terraform holat fayllari
- Sahifalar mazmuni
- Loyiha darajasidagi xavfsiz fayllar

### Konsolidatsiyalangan shaklga o'tish
Saqlashga xos konfiguratsiyada:

- CI/CD artefaktlari, LFS fayllari va yuklama ilovalari kabi barcha ob'ekt turlari uchun ob'ekt saqlash konfiguratsiyasi mustaqil ravishda sozlanadi.
- Parollar va endpoint URL lari kabi ob'ekt saqlash ulanish parametrlari har bir tur uchun takrorlanadi.

Masalan, Linux paketi o'rnatilishi quyidagi konfiguratsiyaga ega bo'lishi mumkin:

```ruby
# Asl ob'ekt saqlash konfiguratsiyasi
gitlab_rails['artifacts_object_store_enabled'] = true
gitlab_rails['artifacts_object_store_direct_upload'] = true
gitlab_rails['artifacts_object_store_proxy_download'] = false
gitlab_rails['artifacts_object_store_remote_directory'] = 'artifacts'
gitlab_rails['artifacts_object_store_connection'] = { 'provider' => 'AWS', 'aws_access_key_id' => 'access_key', 'aws_secret_access_key' => 'secret' }
gitlab_rails['uploads_object_store_enabled'] = true
gitlab_rails['uploads_object_store_direct_upload'] = true
gitlab_rails['uploads_object_store_proxy_download'] = false
gitlab_rails['uploads_object_store_remote_directory'] = 'uploads'
gitlab_rails['uploads_object_store_connection'] = { 'provider' => 'AWS', 'aws_access_key_id' => 'access_key', 'aws_secret_access_key' => 'secret' }
```

Bu turli xil bulut provayderlarida ob'ektlarni saqlash imkonini beradi, lekin qo'shimcha murakkablik va keraksiz takrorlanishlarni keltirib chiqaradi. GitLab Rails va Workhorse komponentlari ob'ekt saqlashga kirishi kerak bo'lganligi sababli, konsolidatsiyalangan shakl hisob ma'lumotlarining ortiqcha takrorlanishini oldini oladi.

Konsolidatsiyalangan shakl faqat asl shakldagi barcha qatorlar olib tashlansa ishlatiladi. Konsolidatsiyalangan shaklga o'tish uchun asl konfiguratsiyani (masalan, `artifacts_object_store_enabled` yoki `uploads_object_store_connection`) olib tashlang.

### Ob'ektlarni boshqa ob'ekt saqlash provayderiga ko'chirish
GitLab ma'lumotlarini ob'ekt saqlashdan boshqa ob'ekt saqlash provayderiga ko'chirish kerak bo'lishi mumkin. Quyidagi qadamlar Rclone yordamida buni qanday amalga oshirishni ko'rsatadi.

Quyidagi qadamlar `uploads` chelakni ko'chirishni nazarda tutadi, lekin boshqa chelaklar uchun ham xuddi shunday jarayon ishlaydi.

**Old shartlar:**

- Rclone ni ishga tushirish uchun kompyuterni tanlang. Ko'chiriladigan ma'lumotlar miqdoriga qarab, Rclone uzoq vaqt ishlashi mumkin, shuning uchun quvvat tejash rejimiga o'tishi mumkin bo'lgan noutbuk yoki ish stolidan foydalanmang. GitLab serveridan foydalanishingiz mumkin.
- Rclone ni o'rnating.

**Rclone ni sozlash:**

```shell
rclone config
```

Sozlash jarayoni interaktivdir. Kamida ikkita "remotes" qo'shing: biri hozirgi ma'lumotlaringiz joylashgan ob'ekt saqlash provayderi uchun (eski), ikkinchisi ko'chmoqchi bo'lgan provayder uchun (yangi).

Eski ma'lumotlarni o'qiy olishingizni tasdiqlang. Quyidagi misol `uploads` chelakka ishora qiladi, lekin chelakingiz boshqa nomga ega bo'lishi mumkin:

```shell
rclone ls old:uploads | head
```

Bu sizning `uploads` chelakingizda hozirda saqlangan ob'ektlarning qisman ro'yxatini chiqarishi kerak. Agar xato olsangiz yoki ro'yxat bo'sh bo'lsa, `rclone config` yordamida Rclone konfiguratsiyasini yangilang.

**Dastlabki nusxalashni amalga oshiring.** Bu qadam uchun GitLab serverini o'chirmaslik kerak:

```shell
rclone sync -P old:uploads new:uploads
```

Birinchi sinxronlash tugallangandan so'ng, yangi ob'ekt saqlash provayderining veb-interfeysi yoki buyruq qatori interfeysidan foydalanib, yangi chelakda ob'ektlar borligini tekshiring. Agar ob'ektlar bo'lmasa yoki `rclone sync` ni ishga tushirishda xato yuzaga kelsa, Rclone konfiguratsiyasini tekshiring va qayta urinib ko'ring.

Kamida bitta muvaffaqiyatli Rclone nusxalashni eskidagi joylashuvdan yangi joylashuvga amalga oshirgandan so'ng, texnik xizmatni rejalashtiring va GitLab serverini o'chiring. Texnik xizmat oynasida ikkita narsani bajarishingiz kerak:

1. Foydalanuvchilar yangi ob'ektlar qo'sha olmasligini bilgan holda, eski chelakda hech narsa qoldirmaslik uchun oxirgi `rclone sync` ni ishga tushiring.
2. GitLab serverining ob'ekt saqlash konfiguratsiyasini yangi provayderdan foydalanish uchun yangilang.

### Fayl tizimi saqlashiga alternativlar
Agar GitLab implementatsiyasini kengaytirish yoki xatolarga chidamlilik va ortiqchalik qo'shish ustida ishlayotgan bo'lsangiz, blok yoki tarmoq fayl tizimlariga bog'liqlikni olib tashlashni ko'rib chiqishingiz mumkin. Quyidagi qo'shimcha qo'llanmalarni ko'ring:

- `git` foydalanuvchi uy katalogi mahalliy diskda ekanligiga ishonch hosil qiling.
- Umumiy `authorized_keys` fayliga ehtiyojni yo'qotish uchun SSH kalitlarini ma'lumotlar bazasida qidirishni sozlang.
- Ish jurnallari uchun mahalliy disk ishlatilishini oldini oling.
- Sahifalar mahalliy saqlashni o'chirib qo'ying.

### Nosozliklarni bartaraf etish

#### Ob'ektlar GitLab zaxiralariga kiritilmagan
Zaxira hujjatlarida ta'kidlanganidek, ob'ektlar GitLab zaxiralariga kiritilmaydi. Buning o'rniga ob'ekt saqlash provayderingizda zaxiralarni yoqishingiz mumkin.

#### Alohida chelaklardan foydalanish
GitLab uchun har bir ma'lumot turi uchun alohida chelaklardan foydalanish tavsiya etiladi. Bu GitLab saqlaydigan turli xil ma'lumot turlari o'rtasida to'qnashuvlar bo'lmasligini ta'minlaydi. #292958 masalasi yagona chelakdan foydalanishni yoqishni taklif qiladi.

Linux paketi va o'z-o'zini kompilyatsiya qilingan o'rnatishlarda bitta haqiqiy chelakni bir nechta virtual chelaklarga bo'lish mumkin. Agar ob'ekt saqlash chelakingiz `my-gitlab-objects` deb atalsa, yuklamalarni `my-gitlab-objects/uploads` ga, artefaktlarni `my-gitlab-objects/artifacts` ga va hokazo sozlashingiz mumkin. Ilova bularni alohida chelaklar sifatida qabul qiladi. Chelak prefikslaridan foydalanish Helm zaxiralari bilan to'g'ri ishlamasligi mumkin.

Helm asosidagi o'rnatishlar zaxiralarni tiklash uchun alohida chelaklarni talab qiladi.

#### S3 API moslik muammolari
Barcha S3 provayderlari GitLab ishlatadigan Fog kutubxonasi bilan to'liq mos kelmaydi. Belgilar `production.log` da quyidagi xato bilan ko'rinadi:

```
411 Length Required
```

#### Artefaktlar doimo "download" nomi bilan yuklanadi
Yuklangan artefakt fayllarining nomlari `response-content-disposition` sarlavhasi bilan `GetObject` so'rovida o'rnatiladi. Agar S3 provayderi bu sarlavhani qo'llab-quvvatlamasa, yuklangan fayl doimo `download` sifatida saqlanadi.

#### Proksi yuklab olish
Mijozlar ob'ekt saqlashdagi fayllarni vaqtincha imzolangan URL orqali yoki GitLab ob'ekt saqlashdan ma'lumotlarni proksi qilish orqali yuklab olishlari mumkin. Ob'ekt saqlashdan to'g'ridan-to'g'ri fayl yuklab olish GitLab ishlov berishi kerak bo'lgan chiqish trafigini kamaytirishga yordam beradi.

Fayllar mahalliy blok saqlash yoki NFS da saqlansa, GitLab proksi sifatida harakat qilishi kerak. Bu ob'ekt saqlash bilan standart xatti-harakat emas.

`proxy_download` sozlamasi bu xatti-harakatni boshqaradi: standart qiymat `false`. Buni har bir foydalanish holati uchun hujjatlarda tekshiring.

Agar `proxy_download` ni `true` ga o'rnatsangiz, GitLab fayllarni proksi qiladi. Bu GitLab serveriga katta ishlash ta'sirini keltirib chiqarishi mumkin. GitLab server o'rnatishlarida `proxy_download` `false` ga o'rnatilgan.

Agar `proxy_download` `false` bo'lsa, GitLab vaqtincha imzolangan ob'ekt saqlash URL bilan HTTP 302 qayta yo'naltirishni qaytaradi. Bu quyidagi muammolarga olib kelishi mumkin:

- Agar GitLab ob'ekt saqlashga kirish uchun xavfsiz bo'lmagan HTTP dan foydalansa, mijozlar https->http pasaytirish xatolarini keltirib chiqarishi va qayta yo'naltirishni qayta ishlashdan bosh tortishi mumkin. Yechim sifatida GitLab HTTPS dan foydalanishi kerak. Masalan, LFS quyidagi xatoni keltirib chiqaradi:

```
LFS: lfsapi/client: xavfsiz bo'lmagan qayta yo'naltirishni rad etish, https->http
```

- Mijozlar ob'ekt saqlash sertifikatini chiqargan sertifikat organiga ishonishi kerak, aks holda umumiy TLS xatolari qaytishi mumkin, masalan:

```
x509: noma'lum organ tomonidan imzolangan sertifikat
```

- Mijozlar ob'ekt saqlashga tarmoq kirishiga ega bo'lishi kerak. Tarmoq devorlari kirishni bloklashi mumkin. Agar bu kirish mavjud bo'lmasa, quyidagi xatolar paydo bo'lishi mumkin:

```
Serverdan 403 holat kodi qabul qilindi: Taqiqlangan
```

- Ob'ekt saqlash chelaklari GitLab instansining URL dan Cross-Origin Resource Sharing (CORS) kirishini ruxsat berishi kerak. Repozitoriy sahifasida PDF ni yuklashga urinish quyidagi xatoni ko'rsatishi mumkin:

```
Faylni yuklashda xato yuz berdi. Iltimos, keyinroq qayta urinib ko'ring.
```

Qo'shimcha ma'lumot uchun LFS hujjatlariga qarang.

Bundan tashqari, qisqa vaqt davomida foydalanuvchilar vaqtincha imzolangan ob'ekt saqlash URL larini autentifikatsiyasiz boshqalar bilan baham ko'rishi mumkin. Shuningdek, ob'ekt saqlash provayderi va mijoz o'rtasida tarmoq kengligi xarajatlari yuzaga kelishi mumkin.

#### ETag mos kelmasligi
GitLab standart sozlamalaridan foydalanganda, MinIO va Alibaba kabi ba'zi ob'ekt saqlash backend lari ETag mos kelmaslik xatolarini keltirib chiqarishi mumkin.

**Amazon S3 shifrlash**
Agar Amazon Web Services S3 bilan ETag mos kelmaslik xatosini ko'rsangiz, bu chelakingizdagi shifrlash sozlamalari bilan bog'liq bo'lishi mumkin. Bu muammoni hal qilish uchun ikkita variant mavjud:

1. Konsolidatsiyalangan shakldan foydalaning.
2. Amazon instans profillaridan foydalaning.

MinIO uchun birinchi variant tavsiya etiladi. Aks holda, MinIO uchun yechim sifatida serverda `--compat` parametridan foydalanish mumkin.

Konsolidatsiyalangan shakl yoki instans profillari yoqilmagan bo'lsa, GitLab Workhorse fayllarni Content-MD5 HTTP sarlavhasi hisoblanmagan oldindan imzolangan URL lar yordamida S3 ga yuklaydi. Ma'lumotlarning buzilmasligini ta'minlash uchun Workhorse yuborilgan ma'lumotlarning MD5 xeshini S3 serveridan qaytarilgan ETag sarlavhasi bilan solishtiradi. Shifrlash yoqilganda bu holat sodir bo'lmaydi, bu Workhorse ga yuklash paytida ETag mos kelmaslik xatosini xabar qilishiga olib keladi.

Konsolidatsiyalangan shakl:

- S3-ga mos ob'ekt saqlash yoki instans profili bilan ishlatilganda, Workhorse S3 hisob ma'lumotlariga ega bo'lgan ichki S3 mijozini ishlatadi, shuning uchun Content-MD5 sarlavhasini hisoblashi mumkin. Bu S3 serveridan qaytarilgan ETag sarlavhalarini solishtirish zaruratini yo'qotadi.
- S3-ga mos ob'ekt saqlash bilan ishlatilmaganda, Workhorse oldindan imzolangan URL larga qaytadi.

**Google Cloud Storage shifrlash**
Mijoz tomonidan boshqariladigan shifrlash kalitlari (CMEK) bilan ma'lumot shifrlash yoqilganda Google Cloud Storage (GCS) da ETag mos kelmaslik xatolari yuzaga keladi.

CMEK dan foydalanish uchun konsolidatsiyalangan shakldan foydalaning.

#### Ko'p oqimli nusxalash
GitLab chelak ichida fayllarni nusxalashni tezlashtirish uchun S3 Upload Part Copy API dan foydalanadi. Kraken 11.0.2 gacha bo'lgan Ceph S3 buni qo'llab-quvvatlamaydi va fayllar yuklash jarayonida nusxalanganda 404 xatosini qaytaradi.

Bu funksiyani `:s3_multithreaded_uploads` funksiya bayrog'i yordamida o'chirib qo'yish mumkin. Funksiyani o'chirish uchun GitLab administratoridan Rails konsoliga kirish huquqiga ega bo'lgan kishidan quyidagi buyruqni ishga tushirishni so'rang:

```ruby
Feature.disable(:s3_multithreaded_uploads)
```

#### Rails konsolida qo'lda sinov
Ba'zi hollarda ob'ekt saqlash sozlamalarini Rails konsolida sinab ko'rish foydali bo'lishi mumkin. Quyidagi misol berilgan ulanish sozlamalarini sinab ko'radi, sinov ob'ektini yozadi va nihoyat uni o'qiydi.

**Rails konsolini boshlang.**

Ob'ekt saqlash ulanishini sozlang, `/etc/gitlab/gitlab.rb` da o'rnatgan parametrlar bilan quyidagi misol formatida:

**Mavjud `uploads` konfiguratsiyasidan foydalangan holda ulanish misoli:**

```ruby
settings = Gitlab.config.uploads.object_store.connection.deep_symbolize_keys
connection = Fog::Storage.new(settings)
```

**Kirish kalitlaridan foydalangan holda ulanish misoli:**

```ruby
connection = Fog::Storage.new(
  {
    provider: 'AWS',
    region: 'eu-central-1',
    aws_access_key_id: '<AWS_ACCESS_KEY_ID>',
    aws_secret_access_key: '<AWS_SECRET_ACCESS_KEY>'
  }
)
```

**AWS IAM profillaridan foydalangan holda ulanish misoli:**

```ruby
connection = Fog::Storage.new(
  {
    provider: 'AWS',
    use_iam_profile: true,
    region: 'us-east-1'
  }
)
```

Sinov uchun chelak nomini belgilang, sinov faylini yozing va nihoyat o'qing:

```ruby
dir = connection.directories.new(key: '<bucket-name-here>')
f = dir.files.create(key: 'test.txt', body: 'test')
pp f
pp dir.files.head('test.txt')
```

#### Qo'shimcha nosozliklarni aniqlash
HTTP so'rovlarini ko'rish uchun qo'shimcha nosozliklarni aniqlashni yoqishingiz mumkin. Buni log fayllarida hisob ma'lumotlari sizib chiqmasligi uchun Rails konsolida qilish kerak. Quyidagi turli provayderlar uchun so'rov nosozliklarini aniqlashni yoqishni ko'rsatadi:

**Amazon S3, Google Cloud Storage, Azure Blob Storage**

`EXCON_DEBUG` muhit o'zgaruvchisini o'rnating:

```ruby
ENV['EXCON_DEBUG'] = "1"
```

#### Geo kuzatuv ma'lumotlar bazasini to'liq ob'ektlar muvofiqligini ta'minlash uchun qayta o'rnatish
Quyidagi Geo stsenariysini tasavvur qiling:

- Mu hit bir Geo asosiy va ikkinchi darajali tugundan iborat.
- Asosiyda ob'ekt saqlashga o'tdingiz.
- Ikkinchi darajali alohida ob'ekt saqlash chelaklaridan foydalanadi.
- “Ushbu ikkinchi darajali saytga ob'ekt saqlashda mazmunni replikatsiya qilishga ruxsat berish” opsiyasi yoqilgan.

Bunday migratsiyalar ob'ektlar kuzatuv ma'lumotlar bazasida sinxronlangan deb belgilanishi mumkin, lekin ob'ekt saqlashda jismoniy ravishda yo'q bo'lishi mumkin. Bunday holda, migratsiyadan keyin ob'ektlar holati muvofiq bo'lib qolishi uchun Geo ikkinchi darajali sayt replikatsiyasini qayta o'rnating.

#### Ob'ekt saqlashga o'tishdan keyin ma'lumotlar nomuvofiqligi
Mahalliy saqlashdan ob'ekt saqlashga o'tishda ma'lumotlar nomuvofiqligi yuzaga kelishi mumkin. Ayniqsa, Geo bilan birgalikda, migratsiyadan oldin fayllar qo'lda o'chirilgan bo'lsa.

Masalan, instans administratori mahalliy fayl tizimida bir nechta artefaktlarni qo'lda o'chiradi. Bunday o'zgarishlar ma'lumotlar bazasiga to'g'ri tarqatilmaydi va nomuvofiqliklarga olib keladi. Ob'ekt saqlashga migratsiyadan so'ng, bu nomuvofiqliklar saqlanib qoladi va muammolarni keltirib chiqarishi mumkin. Geo ikkinchi darajalari ma'lumotlar bazasida hali ham mavjud bo'lgan, lekin endi mavjud bo'lmagan fayllarni replikatsiya qilishga urinishda davom etishi mumkin.

#### Geo bilan nomuvofiqliklarni aniqlash
Quyidagi Geo stsenariysini tasavvur qiling:

- Mu hit bir Geo asosiy va ikkinchi darajali tugundan iborat.
- Ikkala tizim ob'ekt saqlashga o'tgan.
- Ikkinchi darajali asosiy bilan bir xil ob'ekt saqlashni ishlatadi.
- “Ushbu ikkinchi darajali saytga ob'ekt saqlashda mazmunni replikatsiya qilishga ruxsat berish” opsiyasi o'chirilgan.
- Ob'ekt saqlash migratsiyasidan oldin bir nechta yuklamalar qo'lda o'chirilgan.
- Bu misolda, masalaga yuklangan ikkita rasm.

Bunday stsenariyda, ikkinchi darajali asosiy bilan bir xil ob'ekt saqlashni ishlatganligi sababli hech qanday ma'lumotni replikatsiya qilishi shart emas. Nomuvofiqliklar tufayli, administratorlar ikkinchi darajalining hali ham ma'lumotlarni replikatsiya qilishga urinayotganini kuzatishi mumkin:

**Asosiy saytda:**

1. Chap yon panelda, pastda, **Admin** ni tanlang.
2. **Geo > Saytlar** ni tanlang.
3. Asosiy saytni ko'ring va tekshirish ma'lumotlarini tekshiring. Barcha yuklamalar tasdiqlangan: Geo Saytlar paneli asosiyning muvaffaqiyatli tekshirilishini ko'rsatadi.
4. Ikkinchi darajali saytni ko'ring va tekshirish ma'lumotlarini tekshiring. Ikkinchi darajali bir xil ob'ekt saqlashni ishlatishi kerak bo'lsa ham, ikkita yuklama hali sinxronlanmoqda ekanligini sezing: Geo Saytlar paneli ikkinchi darajalining nomuvofiqliklarini ko'rsatadi.

#### Nomuvofiqliklarni tozalash
Har qanday o'chirish buyruqlarini berishdan oldin yaqinda ishlaydigan zaxira nusxasiga ega bo'lishingizga ishonch hosil qiling.

Oldingi stsenariyga asoslanib, bir nechta yuklamalar nomuvofiqliklarni keltirib chiqarmoqda, ular quyida misol sifatida ishlatiladi.

Potentsial qoldiqlarni to'g'ri o'chirish uchun quyidagicha davom eting:

1. Aniqlangan nomuvofiqliklarni ularning mos model nomlariga moslashtiring. Model nomi keyingi qadamlarda kerak bo'ladi.

| Ob'ekt saqlash turi        | Model nomi                     |
|----------------------------|--------------------------------|
| Zaxira                    | Qo'llanilmaydi                |
| Konteyner reestri         | Qo'llanilmaydi                |
| Mattermost                | Qo'llanilmaydi                |
| Avtomatik masshtablash kesh| Qo'llanilmaydi                |
| Xavfsiz fayllar           | Ci::SecureFile                |
| Ish artefaktlari          | Ci::JobArtifact va Ci::PipelineArtifact |
| LFS ob'ektlari           | LfsObject                     |
| Yuklamalar               | Upload                        |
| Birlashtirish so'rovlari farqlari | MergeRequestDiff             |
| Paketlar                 | Packages::PackageFile         |
| Bog'liqlik proksisi      | DependencyProxy::Blob va DependencyProxy::Manifest |
| Terraform holat fayllari | Terraform::StateVersion       |
| Sahifalar mazmuni        | PagesDeployment               |

2. Rails konsolini boshlang.

3. Model nomiga asoslanib, hali mahalliy saqlanayotgan barcha “fayllar” ni so'rang (ob'ekt saqlash o'rniga). Bu holda, yuklamalar ta'sirlanganligi sababli, `Upload` model nomi ishlatiladi. `openbao.png` hali mahalliy saqlanayotganini kuzating:

```ruby
Upload.with_files_stored_locally
```

```ruby
#<Upload:0x00007d35b69def68
  id: 108,
  size: 13346,
  path: "c95c1c9bf91a34f7d97346fd3fa6a7be/openbao.png",
  checksum: "db29d233de49b25d2085dcd8610bac787070e721baa8dcedba528a292b6e816b",
  model_id: 2,
  model_type: "Project",
  uploader: "FileUploader",
  created_at: Wed, 02 Apr 2025 05:56:47.941319000 UTC +00:00,
  store: 1,
  mount_point: nil,
  secret: "[FILTERED]",
  version: 2,
  uploaded_by_user_id: 1,
  organization_id: nil,
  namespace_id: nil,
  project_id: 2,
  verification_checksum: nil>]
```

4. Aniqlangan resurslarning `id` si yordamida ularni to'g'ri o'chirish. Avval `find` yordamida to'g'ri resurs ekanligini tasdiqlang, so'ngra `destroy` ni ishga tushiring:

```ruby
Upload.find(108)
Upload.find(108).destroy
```

5. Ixtiyoriy ravishda, resurs to'g'ri o'chirilganligini `find` ni qayta ishga tushirib tasdiqlang, bu endi uni topa olmasligi kerak:

```ruby
Upload.find(108)
```

```ruby
ActiveRecord::RecordNotFound: 'id'=108 bo'lgan Upload topilmadi
```

6. Barcha ta'sirlangan ob'ekt saqlash turlari uchun qadamlarni takrorlang.

#### Ko'p tugunli GitLab instansida ish jurnallari yo'q
Bir nechta Rails tugunlari (veb-xizmatlar yoki Sidekiq ni ishga tushiradigan serverlar) bo'lgan GitLab instanslarida, ish jurnallarini Runner dan yuborilgandan so'ng barcha tugunlar uchun mavjud qilish mexanizmi bo'lishi kerak. Ish jurnallari mahalliy diskda yoki ob'ekt saqlashda saqlanishi mumkin.

Agar NFS ishlatilmayotgan bo'lsa va inkremental jurnal funksiyasi yoqilmagan bo'lsa, ish jurnallari yo'qolishi mumkin:

- Runner dan jurnalni olgan tugun jurnalni mahalliy diskka yozadi.
- GitLab jurnalni arxivlashga urinayotganda, ko'pincha ish boshqa serverda ishlaydi, bu jurnalga kira olmaydi.
- Ob'ekt saqlashga yuklash muvaffaqiyatsiz bo'ladi.

Quyidagi xato `/var/log/gitlab/gitlab-rails/exceptions_json.log` ga yozilishi mumkin:

```yaml
{
  "severity": "ERROR",
  "exception.class": "Ci::AppendBuildTraceService::TraceRangeError",
  "extra.build_id": 425187,
  "extra.body_end": 12955,
  "extra.stream_size": 720,
  "extra.stream_class": {},
  "extra.stream_range": "0-12954"
}
```

Agar CI artefaktlari ko'p tugunli muhitda ob'ekt saqlashga yozilsa, inkremental jurnal funksiyasini yoqish kerak.