# GitLab CE da Bitta Repositoryni Backup and Restore 


## 1. Bitta Repositoryni Backup Qilish

GitLab-ning umumiy backup vositasi butun tizimni saqlaydi, lekin bitta repositoryni backup qilish uchun quyidagi usullardan foydalanamiz. 
Bu usullar faqat kod va commit tarixini saqlaydi (issues, merge requests kabi ma'lumotlar kirmaydi).

### Usul 1: Git Clone yordamida Backup

**Repository URL sini toping:**
- GitLab veb-interfeysida projectga o'tamiz.
- Clone tugmasini bosing va HTTPS nusxalab (masalan, `https://gitlab.example.com/group/project.git`).

**Repositoryni klonlash:**
Local kompyuterda quyidagi buyruqni ishlatamiz:

```bash
git clone https://gitlab.example.com/group/project.git project
```

Bu repositoryni `project` folderiga yuklaydi.

**Klonlangan folderni arxivlaymiz:**
Repositoryni zip yoki tar.gz formatida saqlash uchun:

```bash
tar -czf project_backup_$(date +%s).tar.gz project
```

`project_backup_1658765432.tar.gz` shu file hosil buladi

### Usul 3: Dockerdagi Repositoryni backup qilish

Dockerda agar run qilingan bulsa birinchi urinda gitlab_ce start qilingan container_id ni kurvolamiz:

`docker ps`

**Repository faylini topish:**
Repositorylar odatda konteyner ichida `/var/opt/gitlab/git-data/repositories/<group>/<project>.git` jildida saqlanadi.

Jildni tekshirish uchun:

```bash
docker exec -it {container_id} ls /var/opt/gitlab/git-data/repositories
```

**Faylni local kompyuterga olamiz: group/project.git shu pathga**

```bash
docker cp gitlab:/var/opt/gitlab/git-data/repositories/group/project.git /home/user/backup/project.git
```

**Arxivlash:**
Nusxalangan jildni arxivlash:

```bash
tar -czf /home/user/backup/project_backup.tar.gz /home/user/backup/project.git
```

> Bu usul faqat kod va commit tarixini saqlaydi. Issues, merge requests yoki wikilarni backup qilish uchun GitLabga kiramiz va 
> Export Project functiondan foydalanamiz (Settings > General > Export project). 


## 2. Bitta Repositoryni Qayta Tiklash (Restore)

### Usul 1: Docker ichidagi Repository Fayllarini Qayta Tiklash

**Backup faylini konteynerga nusxalash:**
Agar backup `project.git` jildi sifatida bo'lsa, uni konteynerga qaytarib joylashtirish kk:

```bash
docker cp /home/user/backup/project.git gitlab:/var/opt/gitlab/git-data/repositories/group/project.git
```

Konteyner ichida fayl egasi va ruxsatlarini sozlash:

```bash
docker exec -it gitlab chown -R git:git /var/opt/gitlab/git-data/repositories/group/project.git
docker exec -it gitlab chmod -R u+rwX /var/opt/gitlab/git-data/repositories/group/project.git
```

O'zgarishlarni qo'llash uchun:

```bash
docker exec -it gitlab gitlab-ctl reconfigure
docker exec -it gitlab gitlab-ctl restart
```

### Usul 3: GitLab Export Faylidan Qayta Tiklash

Agar export qilib olgan bulsak:

GitLabda yangi loyiha yaratamiz.

**Eksport faylini import qilish:**
- New Project > Import Project > GitLab Export bo'limiga o'tammiz
- `.tar.gz` faylini yuklaymiz
- GitLab avtomatik ravishda repository, issues, merge requests va boshqa ma'lumotlarni qayta tiklaydi.

# Server orqali install qilingan gitlab-ce ni backup qilish 
1. **Backup olish uchun asosiy buyruq**
   GitLab CE da backup olish uchun gitlab-backup buyrug'idan foydalaniladi. 
   Bu buyruq barcha repositorylar, ma'lumotlar bazasi va qo'shimcha fayllarni (masalan, attachments) arxiv fayliga yig'adi.

   `sudo gitlab-backup create`

* Backup db, repositorylar umuman full gitlab-ce ni backup qivoladi.

2. **Backup pathni change qilish**
   Agar backup pathni o'zgartirish kk busa, /etc/gitlab/gitlab.rb faylida quyidagi paramterni change qilamiz:

    /mnt/gitlab_backup/backups
   `gitlab_rails['backup_path'] = "/mnt/gitlab_backup/backups"`
    recongifure qilamiz uzgartirganimizdan kn
   `sudo gitlab-ctl reconfigure`

3. **Avtomatik backup sozlash**
   Backupni avtomatlashtirish uchun crontab yozish mumkin. Masalan, har kuni soat 04:15 da backup olish uchun:
   
   `sudo crontab -e`
   Quyidagi qatorni qo'shing:
   
   `15 04 * * * /usr/bin/gitlab-backup create`
* Bu har kuni soat 04:15 da backup yaratadi.


1. **GitLab konteynerini aniqlash**
   Avval GitLab ishlayotgan Docker konteynerining nomini yoki ID sini aniqlang:
   
   `docker ps`

2. **Backup buyruqlarini konteyner ichida ishlatish**
   GitLab backup olish uchun konteyner ichida gitlab-backup buyrug'ini ishlatasiz.
   
   `docker exec -it {container_id}  gitlab-backup create`

3. **Backup faylini local kompyuterga kuchirib olamiz**
   Konteyner ichidagi backup faylini local kompytuerga ko'chirish uchun docker cp buyrug'idan foydalaning:
   
   `docker cp :/var/opt/gitlab/backups/.tar /path/to/local/backup`

4. **Konfiguratsiya fayllarini backup qilish**
   Configure filelani backup qilish uchun local hostni ichida turgan folderlarda olishimiz mumkin
   docker-compose.yml volume joyida kurishimiz mumkin:
   
   `volumes: - /srv/gitlab/config:/etc/gitlab - /srv/gitlab/data:/var/opt/gitlab - /srv/gitlab/logs:/var/log/gitlab`
   Unda konfiguratsiya fayllari /srv/gitlab/config jildida bo'ladi. Ushbu jildni backup qilish uchun:
   
   `tar -cf gitlab-config-$(date +%s).tar /srv/gitlab/config`
    