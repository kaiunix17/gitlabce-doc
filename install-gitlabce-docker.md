Gitlab Community editionni pull qilamiz imageni:
$ docker pull gitlab/gitlab-ce:latest

Gitlab config folderlari, loglari, datalari saqlanishi uchun folderlar yaratamiz:
mkdir -p /code/gitlab/config /code/gitlab/logs /code/gitlab/data

Containerni start qilamiz pull qilib olgan imagedan foydalanib:
docker run --detach
--hostname localhost
--publish 8443:443
--publish 8080:80
--publish 6022:22
--name gitlab
--restart always
--volume ./config:/etc/gitlab
--volume ./logs:/var/log/gitlab
--volume ./data:/var/opt/gitlab
gitlab/gitlab-ce

Initial Passwordni olamiz u 24 soatdan kn o'chirilib tashaladi
$ docker exec -it gitlab cat /etc/gitlab/initial_root_password


Gitlab Community start with docker-compose.yml:

services:
gitlab:
image: gitlab/gitlab-ce:latest # image dockerhubni gitlab-ceni olamiza
container_name: GitLab # container nomi
restart: always #
hostname: 'gitlab.kyiv.uz' # log uchun yozamiz agar yozmasak container_id ni olib ketadi default
ports:
- '80:80' # http uchun shu port
- '443:443' # https uchun shu port
- '22:22' # git commandlani bajarish uchun 22 port kerak bizgaa.
volumes:
- '/srv/gitlab/config:/etc/gitlab' volumelarni biriktiramiz doim saqlash uchun filelarini GitLab config filelarini, GitLab logs volumelarini saqlaymiz, GitLab datalarini olamiza volumega biriktiramiz.
- '/srv/gitlab/logs:/var/log/gitlab'
- '/srv/gitlab/data:/var/opt/gitlab'
shm_size: '256m' # GitLabga bogliq applicationlar urtasida shared memory orqali malumot almashadi


Bularni belgilangandan sung

containerni ichiga kiramiz. docker exec -it container_id sh.

cd /opt/GitLab
vi GitLab.rb
external_url = 'https://gitlab.kyiv.uz'
letsencrypt['enable'] = true
qilamiza a undan buldi bitta restart beraman containerga ishlab ketadi.

docker exec -it gitlab gitlab-ctl reconfigure
docker exec -it gitlab ls /etc/gitlab/initial_root_password
