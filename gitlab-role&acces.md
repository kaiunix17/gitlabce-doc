# Gitlab Role:

* `guest` - internal va public repositorylarni clone qiloladi privatelarni kurolmaydi. Issue yaratoladi merge requestlarni kuroladi. 
Biror ortiqcha action qilolmaydi repolarga ruxsat berilmasa


* `reporter` - o'zi project yarata oladi. Git clone git push qilishi, projectni control qilishi, branch yaratishi, commitlarni ko'rishi, pipelinelarni ko'rishi yozishi build qilishi mumkin bu ishlarni faqat o'zi yaratgan repoda qila oladi.
Boshqa projectlarda branch yarata olmaydi settingslarni o'zgartira olmaydi merge request yarata olamaydi. Guest qila olgan hamma ishlarni qila oladi.


* `developer` - project kodlari bilan ishlay oladi. branch yarata oladi o'chira oladi. Merge requestlar yaratadi, marge request edit qila oladi, merge request approve qila oladi.
Ci/CD ga ruxsati bor pipelineni ishga tushiradi joblarni cancel qilishi mumkin. Qilolmaydigan ishi settingslarni o'zgartirolmaydi; webhookni configure qilolmaydi, member qusholmaydi


* `maintainer` - member qusholadi developerda bor hamma access unda ham bo'ladi. control qiladi memberlarni external userlarga access bera oladi.
webhooklar yarata oladi, ci/cdni setup qila oladi. Qilolmaydigan yagona ishi projectni o'chira olmaydi.


* `Owner` - barcha accessga ega !