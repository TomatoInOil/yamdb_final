# API YAMDB
![Yamdb workflow](https://github.com/tomatoinoil/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
## Оглавление
1. [Описание проекта](https://github.com/TomatoInOil/api_yamdb#описание-проекта)
2. [Как развернуть](https://github.com/TomatoInOil/api_yamdb#как-развернуть)
    1. [Запуск с помощью сервера разработки](https://github.com/TomatoInOil/api_yamdb#запуск-с-помощью-сервера-разработки)
    2. [Запуск проекта через докер](https://github.com/TomatoInOil/api_yamdb#запуск-проекта-через-докер)
    3. [Уже развёрнутый проект](https://github.com/TomatoInOil/api_yamdb#уже-развёрнутый-проект)
3. [Работа с API](https://github.com/TomatoInOil/api_yamdb#работа-с-api)
    1. [Доступные запросы](https://github.com/TomatoInOil/api_yamdb#доступные-запросы)
    2. [Аутентификация](https://github.com/TomatoInOil/api_yamdb#аутентификация) 
    3. [Примеры запросов и ответов](https://github.com/TomatoInOil/api_yamdb#примеры-запросов-и-ответов) 
4. [Об авторах](https://github.com/TomatoInOil/api_yamdb#об-авторах)
## Описание проекта
Проект _**YaMDb**_ собирает **отзывы** пользователей на **произведения**. Сами произведения в _**YaMDb**_ не хранятся, здесь нельзя посмотреть фильм или послушать музыку.  
  
**Произведения** делятся на **категории**: «Книги», «Фильмы», «Музыка». Список **категорий** может быть расширен администратором.  
Произведению может быть присвоен **жанр** из списка предустановленных. Новые **жанры** может создавать только администратор.  
Благодарные или возмущённые пользователи оставляют к произведениям текстовые **отзывы** и ставят произведению **оценку** в диапазоне от **1** до **10** (целое число); из пользовательских **оценок** формируется усреднённая оценка произведения — **рейтинг** (целое число). На одно произведение пользователь может оставить только один отзыв.  
  
В проекте реализован _**API**_-**сервис** для аутентификации пользователей и работы со всеми вышеперечисленными ресурсами. Подробнее в разделе [примеров запросов и ответов](https://github.com/TomatoInOil/api_yamdb#примеры-запросов-и-ответов). Аутентификация пользователей реализована по стандарту _**JWT**_.  
## Как развернуть
### Запуск с помощью сервера разработки
```BASH
git clone https://github.com/TomatoInOil/api_yamdb.git
```
```BASH
cd api_yamdb/
```
Cоздать и активировать виртуальное окружение:
```BASH
python3 -m venv env
```
```BASH
source env/bin/activate
```
Обновить менеджер пакетов
```BASH
python3 -m pip install --upgrade pip
```
Установить зависимости из файла `requirements.txt`:
```BASH
pip install -r requirements.txt
```
Перейти в директорию с `manage.py`:
```BASH
cd api_yamdb/
```
Выполнить миграции:
```BASH
python3 manage.py migrate
```
Запустить проект:
```BASH
python3 manage.py runserver
```
ДОП. Наполнить БД тестовыми данными
```
python3 manage.py csvtodb
```
### Запуск проекта через докер
Перейти в директорию с файлом docker-compose
```BASH
cd infra/
```
Создать и заполнить `.env` в соотвествии с шаблоном `example.env` в директории `infra/`  
  
Запустить docker-compose
```BASH
docker-compose up -d
```
Выполнить миграции
```BASH
docker-compose exec web python manage.py migrate --noinput
```
Собрать статику 
```BASH
docker-compose exec web python manage.py collectstatic
```
ДОП. Наполнить БД тестовыми данными
```
docker-compose exec web python manage.py csvtodb
```
### Уже развёрнутый проект
Развёрнутый проект доступен по адресу http://158.160.4.15/, а на эндпоинте http://158.160.4.15/api/v1/redoc/ можно найти документацию к API.
## Работа с API
### Доступные запросы
| Запрос | Эндпоинт | Метод |
|--------|:---------|-------|
| Регистрация нового пользователя |`.../api/v1/auth/signup/`| POST |
| Получение JWT-токена |`.../api/v1/auth/token/`| POST |
| Получение списка всех категорий |`.../api/v1/categories/`| GET |
| Добавление новой категории |`.../api/v1/categories/`| POST |
| Удаление категории |`.../api/v1/categories/{slug}/`| DELETE |
| Получение списка всех жанров |`.../api/v1/genres/`| GET |
| Добавление жанра |`.../api/v1/genres/`| POST |
| Удаление жанра |`.../api/v1/genres/{slug}/`| DELETE |
| Получение списка всех произведений |`.../api/v1/titles/`| GET |
| Добавление произведения |`.../api/v1/titles/`| POST |
| Получение информации о произведении |`.../api/v1/titles/{title_id}/`| GET |
| Частичное обновление информации о произведении |`.../api/v1/titles/{title_id}/`| PATCH |
| Удаление произведения |`.../api/v1/titles/{title_id}/`| DELETE |
| Получение списка всех отзывов |`.../api/v1/titles/{title_id}/reviews/`| GET |
| Добавление нового отзыва |`.../api/v1/titles/{title_id}/reviews/`| POST |
| Получение отзыва по id |`.../api/v1/titles/{title_id}/reviews/{review_id}/`| GET |
| Частичное обновление отзыва по id |`.../api/v1/titles/{title_id}/reviews/{review_id}/`| PATCH |
| Удаление отзыва по id |`.../api/v1/titles/{title_id}/reviews/{review_id}/`| DELETE |
| Получение списка всех комментариев к отзыву |`.../api/v1/titles/{title_id}/reviews/{review_id}/comments/`| GET |
| Добавление комментария к отзыву |`.../api/v1/titles/{title_id}/reviews/{review_id}/comments/`| POST |
| Получение комментария к отзыву |`.../api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/`| GET |
| Частичное обновление комментария к отзыву |`.../api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/`| PATCH |
| Удаление комментария к отзыву |`.../api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/`| DELETE |
| Получение списка всех пользователей |`.../api/v1/users/`| GET |
| Добавление пользователя |`.../api/v1/users/`| POST |
| Получение пользователя по username |`.../api/v1/users/{username}/`| GET |
| Изменение данных пользователя по username |`.../api/v1/users/{username}/`| PATCH |
| Удаление пользователя по username |`.../api/v1/users/{username}/`| DELETE |
| Получение данных своей учетной записи |`.../api/v1/users/me/`| GET |
| Изменение данных своей учетной записи |`.../api/v1/users/me/`| PATCH |
### Аутентификация
#### Алгоритм регистрации пользователей
1. Пользователь отправляет _**POST**_-запрос на добавление нового пользователя с параметрами `email` и `username` на эндпоинт `.../api/v1/auth/signup/`.  
> Пример запроса:  
> _**POST .../api/v1/auth/signup/**_  
> ```JSON
> {
>   "email": "string",
>   "username": "string"
> }
> ```
> Пример ответа (200):
> ```JSON
> {
>   "email": "string",
>   "username": "string"
> }
> ```
2. YaMDB отправляет письмо с кодом подтверждения (`confirmation_code`) на адрес `email`.
3. Пользователь отправляет _**POST**_-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (_JWT_-токен).
> Пример запроса:  
> _**POST .../api/v1/auth/token/**_  
> ```JSON
> {
>   "username": "string",
>   "confirmation_code": "string"
> }
> ```
> Пример ответа (200):
> ```JSON
> {
>   "token": "string"
> }
> ```
4. При желании пользователь отправляет _**PATCH**_-запрос на эндпоинт `/api/v1/users/me/` и заполняет поля в своём профайле.
> Пример запроса:  
> _**POST .../api/v1/users/me/**_  
> ```JSON
> {
>   "username": "string",
>   "email": "user@example.com",
>   "first_name": "string",
>   "last_name": "string",
>   "bio": "string"
> }
> ```
> Пример ответа (200):
> ```JSON
> {
>   "username": "string",
>   "email": "user@example.com",
>   "first_name": "string",
>   "last_name": "string",
>   "bio": "string"
>   "role": "user"
> }
> ```
#### Пользовательские роли
- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
- **Аутентифицированный пользователь** (`user`) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
- **Модератор** (`moderator`) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
- **Администратор** (`admin`) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- **Суперюзер Django** — обладет правами администратора (`admin`)  
### Примеры запросов и ответов
После того, как проект [развёрнут и запущен](https://github.com/TomatoInOil/api_yamdb#как-развернуть), документацию по API можно найти на эндпоинте `.../api/v1/redoc/`.
## Об авторах
Над проектом работали [Владимир Коршак](https://github.com/Vladimir-spb), [Антон Шлёнов](https://github.com/CodeDemonTony) и [Даниил Паутов](https://github.com/TomatoInOil), студенты Яндекс.Практикума. Это была замечательная командная работа!
