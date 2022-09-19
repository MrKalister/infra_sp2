## Проект API для Yamdb

## Описание:

Демонстрация работы Docker. Производится сборка образа на основе Dockerfile и запуск контейнеров при помощи Docker-compose.

## Технологический стек:
* python 3.7
* Django
* Django Rest Framework
* Redoc
* Docker
* PostgreSQL

## Установка пректа:
Все команды выполняются в командной строке.
#### Клонировать репозиторий:
* Вариант 1. По SSH:
```
git clone git@github.com:MrKalister/infra_sp2.git
```
* Вариант 2. По HTTPS:
```
git clone https://github.com/MrKalister/infra_sp2.git
```
#### Перейти в проект:
```
cd infra
```
#### Добавить пользователя в группу docker:
```
sudo usermod -aG docker username
```
#### Убедитесь что пользователь добавлен в группу:
```
groups
```
#### Заполнить шаблон .env_example:
Лучше всего воспользоваться встроенным редактором nano или любым аналогичным.
```
nano .env_example
```
#### Переименовать env-файл:
```
cp .env_example .env
```
#### Создать и запустить контейнеры:
```
docker-compose up
```
#### Выполнить миграции:
```
docker-compose exec web python manage.py migrate
```
#### Создать администратора:
```
docker-compose exec web python manage.py createsuperuser
```
#### Загрузить тестовую базу данных:
```
docker-compose exec web python manage.py loaddata fixtures.json
```
#### Проверить, что проект запущен:
1. Ввести в браузере:
```
localhost/admin
```
2. Войти в учетую запись администратора
3. Нажмите Titles и удостоверьтесь в наличие записей.

## Использование пректа:
Авторизация на проекте осуществляется при помощи JWT-токенов.

#### Регистрация
1. Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
2. Бэкенд отправляет письмо с кодом подтверждения на указанный адрес email.
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code с этим кодом на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен). В ответе приходит только access token (без refresh token'a) c временем жизни в сутки, по истечении которого, пользователю необходимо снова отправить запрс на эндпоинт /api/v1/auth/token/ и получить новый токен.

Так же пользователя может создать и админ, в этом случае, поле confirmation_code у пользователя будет пустое, при отправке POST-запроса с параметрами email и username этого пользователя на эндпоинт /api/v1/auth/signup/, бэкенд создаст код потверждения (confirmation_code) и также отправит письмо с кодом подтверждения (confirmation_code) на указанный адрес email.

#### Примеры запросов:
Получение списка всех произведений:
GET http://127.0.0.1:8000/api/v1/titles/

Получение произведения c id 1 в базе данных:
GET http://127.0.0.1:8000/api/v1/titles/1/

Получение списка всех отзывов о произведении c id 1 в базе данных:
GET http://127.0.0.1:8000/api/v1/titles/1/reviews/

Создание отзыва о произведении c id 1 в базе данных:
POST http://127.0.0.1:8000/api/v1/titles/1/reviews/
с телом запроса 
```
{
"text": "string",
"score": 1
}
```

Частичное обновление отзыва c id 1 о произведении c id 1 в базе данных:
PATCH http://127.0.0.1:8000/api/v1/titles/1/reviews/1/
с телом запроса 
```
{
"score": 2
}
```

Удаление отзыва c id 1 о произведении c id 1 в базе данных:
DELETE http://127.0.0.1:8000/api/v1/titles/1/reviews/1/

Получение списка всех комментариев к отзыву c id 1 о произведении c id 1 в базе данных:
GET http://127.0.0.1:8000/api/v1/titles/1/reviews/comments/

Создание комментария к отзыву c id 1 о произведении c id 1 в базе данных:
POST http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/
с телом запроса 
```
{
"text": "string"
}
```

Частичное обновление комментария c id 1 к отзыву c id 1 о произведении c id 1 в базе данных:
PATCH http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/1/
с телом запроса 
```
{
"text": "string"
}
```

Удаление комментария c id 1 к отзыву c id 1 о произведении c id 1 в базе данных:
DELETE http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/1/

#### Остальные примеры запросов и подробное описание доступны в документации по пути
http://127.0.0.1:8000/redoc/

## Контакты автора:
email: 

maxon.nowik@yandex.ru

github: 

https://github.com/suslyak/
