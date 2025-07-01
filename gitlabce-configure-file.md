Gitlab selfhost configuration features:

1. GitLab URL: external_url. GitLab’ning tashqi foydalanuvchilar uchun ochiq bo‘lgan URL manzilini belgilaydi (https://gitlab.kyiv.uz).

2. Roles for multi-instance GitLab.
   ##!   application_role - GitLab uchun rolelarni belgilaymiz. projectlarni boshqarish uchun shu rollar userlar
   ##!   redis_sentinel_role, redis_master_role, redis_replica_role - redis buyicha rolelarni boshqarish uchun kerak.
   ##!   monitoring_role - Prometheus monitoring applicationlani ishga tushirish GitLab resurslarini monitoring qilish uchun kerak
   ##!   geo_primary_role, geo_secondary_role - (ee mode uchun ishlaydi). datalarni tashqaridagi userlarga yuborish uchun uqish uchun
   ##!   postgres_role - gitlabni asosiy database hisooblanadi

3. GitLab Rails (gitlab_rails).
   git commandlarini sozlash mumkin, timezona sozlash, default_themelar GitLab default colorlarni sozlash, electron pochtani sozlash mumkin, userlarni uzini nomini uzgartirishga ruxsat berish, google yoki boshqa katta programmalar bilan login qilish,

4. Veb-server (NGINX)
   Asosiy sozlamalar: nginx['enable'], nginx['listen_port'], nginx['ssl_certificate'], nginx['ssl_key']
   Maqsad: GitLab’ni xizmat qilish uchun NGINX veb-serverini sozlash.
   Imkoniyat: NGINX’ni yoqish/o‘chirish, portlarni sozlash, SSL/TLS’ni yoqish yoki sarlavhalar va ishlash sozlamalarini moslashtirish, artifaktlar, paketlar terraform sozlar, backuplarni sozlash, cacheni tozalash, ci artefaktlarni uchirish, proxylani ruyxatdan utkazish, webhooklarni vaqtini sozlash

5. PostgreSQL
   Maqsad: PostgreSQL ma’lumotlar bazasini sozlash, shu jumladan xost, port, foydalanuvchi nomi va parol.

6. Redis
   Maqsad: Kesh, navbatlar va sessiya saqlash uchun Redis’ni sozlash.

7. Monitoring (Prometheus)
   Maqsad: GitLab va tizim metrikalari uchun monitoring vositalarini yoqish va sozlash.

8. Let’s Encrypt
   Maqsad: Let’s Encrypt orqali SSL sertifikatlarini avtomatik yaratish va yangilashni yoqish.

9. Cron ishlari
   Asosiy sozlamalar: Turli gitlab_rails['*_cron'] sozlamalari
   Maqsad: Repozitoriy tekshiruvlari, backuplarni tozalash loglarni tozalash