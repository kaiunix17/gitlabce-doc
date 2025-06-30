# Gitlab runner caching

Kesh - `job` yuklab oladigan va saqlaydigan bir yoki bir nechta filelar tuplami. Agar kesh qilib fileni yuklab olsa keyingi safar uni yana qayta yuklab olmaydi. Ishni tezlashtirish uchun kerak
Kesh har bir job uchun alohida yaratiladi. Kesh agar yozilgan bulsa keyingi pipelineda ishlatish mumkin agar key tugri kelsa.

```yaml
test-job:
  stage: build
  cache:
    - key:
        files:
          - Gemfile.lock
      paths:
        - vendor/ruby
    - key:
        files:
          - yarn.lock
      paths:
        - .yarn-cache/
  script:
    - bundle config set --local path 'vendor/ruby'
    - bundle install
    - yarn install --cache-folder .yarn-cache
    - echo Run tests...
```

bu joyda 2ta cache bor bittasi 

## Keshdan foydalanish uchun mana buni maximal qilish kerak:
1. Runnerlar teglash kerak. Bu keshni aynan bitta runnerda saqlaydi. (Keshlar runner serverni ichida saqlanadi)
2. Har bitta project uchun runner alohida yaratish kerak sababi keshlar boshqa loyiha bilan almashib ketishini fix qilish uchun kk.
3. Keshlarni S3 yoki shunga uxshash joylarda saqlansa ularni tozalab turish mumkin. Bulmasa manual tozalashga tugri keladi.

## Bitta jobda bir nechta keshdan foydalanish
Har bir job uchun maximal 4ta cache saqlash mumkin. 
```yaml
test-job:
  stage: build
  cache:
    - key:
        files:
          - Gemfile.lock
      paths:
        - vendor/ruby
    - key:
        files:
          - yarn.lock
      paths:
        - .yarn-cache/
  script:
    - bundle config set --local path 'vendor/ruby'
    - bundle install
    - yarn install --cache-folder .yarn-cache
    - echo Run tests...
```

## Fallback cache key 
Agar kesh key topolmasa fallback keys ishlatiladi.
```yaml
test-job:
  stage: build
  cache:
    - key: cache-$CI_COMMIT_REF_SLUG
      fallback_keys:
        - cache-$CI_DEFAULT_BRANCH
        - cache-default
      paths:
        - vendor/ruby
  script:
    - bundle config set --local path 'vendor/ruby'
    - bundle install
    - echo Run tests...
```

Birinchi navbatda keyni qidiradi yani cache-$CI_COMMIT_REF_SLUG. agar bu topilmasa. fallback_keysdan cache-$CI_DEFAULT_BRANCH ni qidiradi agar uni ham topolmasa cache-default ni qidiradi agar topolmasa umuman shunchaki davom etadi cachesiz ishlaydi. 
Cache shunchaki jobni tezlash uchun kk u ishlamasa ham davom etadi.

`$CI_COMMIT_REF_SLUG` - CI_COMMIT_REF_NAME lowercasega almashtirilgani va 63 baytga tushirilgan xullas tahrirlab tashalgani.

## Global key

CACHE_FALLBACK_KEY - shu o'zgaruvchi (variable) yordamida global key yaratiladi.

```yaml
variables:
  CACHE_FALLBACK_KEY: fallback-key

job1:
  script:
    - echo
  cache:
    key: "$CI_COMMIT_REF_SLUG"
    paths:
      - binaries/
```

Agar keyni topolmasa variablesdan oladi har qanaqa jobda agar fail bulsa keyni topish globaldan izlab kuradi hashi tugri kelsa ishlaydi bulmasa cache ishlamaydi.

## Global cache

Bir nechta joblarga bitta template yozishday gap. Masalan

```yaml
default:
  cache: &global_cache
    key: $CI_COMMIT_REF_SLUG
    paths:
      - node_modules/
      - public/
      - vendor/
    policy: pull-push

job1:
  cache:
    <<: *global_cache
    policy: pull  # policyni o'zgartiramiz 

job2:
  cache:
    <<: *global_cache
    key: custom-key-123  # keyni o'zgartiramiz
    paths:               # pathni o'zgartirish
      - build/
      - dist/
```

## Keylarga nom berish

Har bir branch uchun alohida kesh yaratish

```yaml
cache:
  key: $CI_COMMIT_REF_SLUG
```

Barcha branch uchun kesh yaratish

```yaml
cache:
  key: one-key-to-rule-them-all
```

Job name ga qarab kesh yaratish

```yaml
cache:
  key: $CI_JOB_NAME
```

## Nodejs uchun kesh yaratish

```yaml
default:
  image: node:latest
  cache:
    key: $CI_COMMIT_REF_SLUG
    paths:
      - .npm/
  before_script:
    - npm ci --cache .npm --prefer-offline
```

Har bir branch uchun alohida kesh yaratiladi .npm ga saqlanadi kesh va script bajariladi

## Kesh qayerda saqlanadi?

Gitlab Runner o'rnatilgan serverda kursatiladi yoki Docker uchun `/var/lib/docker/volumes/` ichida saqlanadi

Cacheni tozalash uchun `Build > Pipelines > Clear runner caches`
