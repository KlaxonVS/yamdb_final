## Проект «api_YaMDb»
***
[![yamdb_workflow](https://github.com/VorVorsky/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/VorVorsky/yamdb_final/actions/workflows/yamdb_workflow.yml)<br/>
***
[Ссылка на API](https://vorvorsky.sytes.net/api/v1/)
***
### Технологии:
``[Django, djangorestframework, simplejwt]``
### Соавторы:
* [Михаил Манузин](https://github.com/klinf1)
* [Олег Смагин](https://github.com/OrdinaryWorker)
***
### Описание:
Проект YaMDb собирает отзывы пользователей на различные произведения.
***
### Функционал:
* Написание отзывов к произведениям;
* Комментирование отзывов;
* Просмотр информации о произведениях:
  * название,
  * год выхода,
  * рейтинг,
  * описание,
  * жанр.
* Возможности пользователя зависят от роли:
    * Аноним — может просматривать описания произведений, читать отзывы<br/>
      и комментарии;
    * Аутентифицированный пользователь (``user``) — может, как и Аноним,<br/>
      читать всё, дополнительно он может публиковать отзывы<br/>
      и ставить оценку произведениям (фильмам/книгам/песенкам),<br/>
      может комментировать чужие отзывы; может редактировать<br/>
      и удалять свои отзывы и комментарии.<br/>
      Эта роль присваивается по умолчанию каждому новому пользователю;
    * Модератор (``moderator``) — те же права, что и у Аутентифицированного<br/>
      пользователя плюс право удалять любые отзывы и комментарии;
    * Администратор (``admin``) — полные права на управление всем контентом проекта.<br/>
      Может создавать и удалять произведения, категории и жанры.<br/>
      Может назначать роли пользователям.
***
### Самостоятельная регистрация новых пользователей:
Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт<br/>
`/api/v1/auth/signup/`. Сервис YaMDB отправляет письмо с кодом подтверждения<br/>
(confirmation_code) на указанный адрес email. Пользователь отправляет POST-запрос<br/>
с параметрами username и confirmation_code на эндпоинт `/api/v1/auth/token/`,<br/>
в ответе на запрос ему приходит token (JWT-токен).<br/>
В результате пользователь получает токен и может работать с API проекта,<br/>
отправляя этот токен с каждым запросом.

### Создание пользователя администратором:
Пользователя может создать администратор — через админ-зону сайта или через<br/>
POST-запрос на специальный эндпоинт `api/v1/users/`<br/>
(описание полей запроса для этого случая — в redoc).<br/>
В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой `email`<br/>
и username на эндпоинт `/api/v1/auth/signup/` , в ответ ему должно прийти письмо<br/>
с кодом подтверждения. Далее пользователь отправляет POST-запрос с параметрами<br/>
`username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`,<br/>
в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.
***
### Ресурсы API YaMDb
[Для перехода в redoc после запуска веб-сервера](http://localhost/redoc/)
* Ресурс `auth`: аутентификация:
    * `v1/auth/signup/` для регистрации пользователей и получения кода подтверждения;
    * `v1/auth/token/` получение токена для доступа к ресурсу.
<br/><br/> 
* Ресурс `users`: пользователи:
    * `users/me/` доступ пользователя к информации о себе;
    * `users/{username}` доступ админа к списку пользователей<br/>
       и при указании `username` к конкретному пользователю;
<br/><br/> 
* Ресурс `title`: произведения, к которым пишут отзывы (определённый фильм, книга или песенка):
    * `titles/{titles_id}` доступ пользователя к информации о произведениях<br/>
      и при указании `titles_id` к конкретному произведению.
<br/><br/> 
* Ресурс `categories`: категории (типы) произведений («Фильмы», «Книги», «Музыка»):
    * `categories/` доступ пользователя к списку категорий.
<br/><br/> 
* Ресурс `genres`: жанры произведений. Одно произведение может быть привязано к нескольким жанрам:
    * `genres/` доступ пользователя к списку жанров.
<br/><br/> 
* Ресурс `reviews`: отзывы на произведения. Отзыв привязан к определённому произведению:
    * `titles/{title_id}/reviews/` доступ пользователя к списку отзывов к произведению `{title_id}`;
    * `titles/{title_id}/reviews/{review_id}/` доступ пользователя к конкретному отзыву;
<br/><br/> 
* Ресурс `comments`: комментарии к отзывам. Комментарий привязан к определённому отзыву:
    * `titles/{title_id}/reviews/{review_id}/comments/`<br/>
        доступ к комментариям к отзыву;
    * `titles/{title_id}/reviews/{review_id}/comments/{comment_id}/` <br/>
        доступ к конкретному коментарию.

## Запуск проекта через Doker:

1. Клонировать репозиторий: <br/>``git clone git@github.com:VorVorsky/yamdb_final.git``
2. Создать файл **.env**

### Образец .env файла -- секретные переменные для проекта расположить в папке<br/>
``infra/``:
* SECRET_KEY (ключ Django-проекта)
* ALLOWED_HOSTS (хосты разделённые "|")
* LOCAL (для локального запуска на компьютере)
* DEBUG (режим отладки)
* DB_ENGINE (используемая база, по умолчанию django.db.backends.postgresql)
* DB_NAME (имя базы)
* POSTGRES_USER (пользователь базы)
* POSTGRES_PASSWORD (пароль пользователя)
* DB_HOST (хост)
* DB_PORT (порт)
* SQLITE (sqlite как бд)
* EMAIL (для сертификации ssl)
* DOMAIN (ваш домен)

3. В консоли из папки `infra/` соберите контейнер: `sudo docker compose up -d --build`
4. Выполнить миграции:
   * `sudo docker compose exec web python manage.py makemigrations`
   * `sudo docker compose exec web python manage.py migrate`
5. Соберите статику:<br/>
`sudo docker compose exec web python manage.py collectstatic --no-input`

### Если необходима выгрузка данных в БД из csv файлов:
1. Выполнить выгрузку коммандой:<br/>`sudo docker compose exec web python manage.py load_data`
2. В PostgreSQL В связи с загрузкой собьются данные о последнем индексе, необходимо<br/>
получить SQL команды для их обновления:<br/>`sudo docker compose exec web python manage.py sqlsequencereset reviews`
3. Войти в оболочку psql: `sudo docker compose exec db psql -U postgres`
4. Выполните полученные SQL запросы:<br/>
`BEGIN;`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_user_groups"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_user_groups";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_user_user_permissions"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_user_user_permissions";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_user"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_user";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_category"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_category";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_genre"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_genre";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_title_genre"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_title_genre";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_title"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_title";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_review"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_review";`<br/>
`SELECT setval(pg_get_serial_sequence('"reviews_comments"','id'), coalesce(max("id"), 1), max("id") IS NOT null) FROM "reviews_comments";`<br/>
`COMMIT;`<br/>

### Создание суперпользователя:
* Используйте команду: `sudo docker compose exec web python manage.py createsuperuser`
#### Если возникает ошибка:
`Superuser creation skipped due to not running in a TTY.`<br/>
`You can run manage.py createsuperuser in your project to create one manually.`<br/>
* Используйте команду: `winpty docker-compose exec web python manage.py createsuperuser`