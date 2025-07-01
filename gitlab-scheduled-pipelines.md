# GitLab CI/CD Rejalashtirilgan Pipeline-lar

## Kirish
Gitlab scheduled pipelines aynan bir vaqtda build bulishini rejalashtirish uchun kerak.

## Yangi pipeline jadvalini qo'shish
Yangi jadval qo'shish uchun quyidagi qadamlarni bajaring:
1. projectga kiramiz.
2. build > scheduled pipelines ga kiramiz.
3. **Yangi jadval** tugmasini bosing va shaklni to'ldiring:
    - **Interval**: Pipeline qanchalik tez-tez ishga tushishini belgilang:
        - Oldindan tayyor intervallarni tanlash (masalan, har kuni, har soatda).
        - Maxsus cron sintaksisi (masalan, `0 9 * * *` — har kuni soat 9:00 da).
    - **Maqsadli shox yoki teg**: Pipeline qaysi shox yoki teg uchun ishlasin, shuni tanlang.
    - **Kiritishlar (Inputs)**: `spec:inputs` bo'limida belgilangan qiymatlarni kiriting (maksimal 20 ta).
    - **O'zgaruvchilar (Variables)**: Faqat rejalashtirilgan pipeline uchun ishlatiladigan CI/CD o'zgaruvchilarini qo'shing. Kiritishlar xavfsizroq va moslashuvchan.

**Eslatma**: Agar loyihada jadvallar soni maksimumga yetgan bo'lsa, yangi jadval qo'shish uchun eski jadvallarni o'chirish kerak.

## Jadvalni tahrirlash
Jadvalni faqat uning egasi tahrirlay oladi:
1. **Qurish > Pipeline jadvallari** bo'limiga o'ting.
2. Jadval yonidagi **Tahrirlash** (✏️) belgisini bosing.
3. Kerakli o'zgarishlarni kiriting.

## Qo'lda ishga tushirish
Pipeline-ni rejalashtirilgan vaqtdan oldin ishga tushirish uchun:
1. **Qurish > Pipeline jadvallari** bo'limiga o'ting.
2. Jadval yonidagi **Ishga tushirish** (▶️) tugmasini bosing.

**Eslatma**:
- Qo'lda ishga tushirishda pipeline foydalanuvchining ruxsatlari bilan ishlaydi, jadval egasinikilar bilan emas.
- Qo'lda ishga tushirish daqiqada 3 marta bosish mumkin.

## Egalilikni olish
Boshqa foydalanuvchi yaratgan jadvalni boshqarish uchun:
1. **Qurish > Pipeline jadvallari** bo'limiga o'ting.
2. Jadval yonidagi **Egalilikni olish** tugmasini bosing.

Buning uchun kamida *Maintainer* roli kerak. Jadval egasi pipeline-ning ruxsatlariga, shu jumladan himoyalangan muhitlarga va CI/CD tokenlariga kirish huquqiga ega.