# Проект API_YAMDB
### Описание
Проект API_YAMDB позволяет создать отзывы (Review) пользователей (User) на произведения (Title). Произведениям при создании присваиваются категории (Category) и жанры (Genre).
Регистрировать произведения, создавать новые категории и жанры может только администратор.
Пользователи могут выставить произведению свою оценку по шкале от 1 до 10, из всех оценок автоматически высчитывается средний рейтинг произведения.

Разработка:
- :white_check_mark: [almazpractice](https://github.com/almazpractice)
- :white_check_mark: [Andrey-Vyshegorodskiy](https://github.com/Andrey-Vyshegorodskiy)
- :white_check_mark: [AlxPaw](https://github.com/AlxPaw)


Полная документация к API находится в /redoc
### Технологии
Python 3.7, Django 2.2, DRF, JWT
### Как запустить проект
- Клонировать репозиторий и перейти в него в командной строке.
```Bash
git clone <url-адрес репозитория на GitHub>

cd api_yamdb
```
- Создать и активировать виртуальное окружение:
```Bash
python -m venv venv

source venv/Scripts/activate

python -m pip install --upgrade pip
```
- Установить зависимости из файла requirements.txt:
```Bash
pip install -r requirements.txt
```
- Выполнить миграции:
```Bash
python manage.py migrate
```
- Заполнить БД тестовыми записями:
```Bash
$ python manage.py upload_data
```
- Запустить проект:
```Bash
$ python manage.py runserver
```
### Примеры работы с API для всех пользователей
Подробная документация доступна по адресу /redoc/
Для неавторизованных пользователей работа с API доступна в режиме чтения,
что-либо изменить или создать не получится. 

Права доступа: Доступно без токена.
```
GET /api/v1/categories/ - Получение списка всех категорий
GET /api/v1/genres/ - Получение списка всех жанров
GET /api/v1/titles/ - Получение списка всех произведений
GET /api/v1/titles/{title_id}/reviews/ - Получение списка всех отзывов
GET /api/v1/titles/{title_id}/reviews/{review_id}/comments/ - Получение списка всех комментариев к отзыву
```
Права доступа: Администратор
```
GET /api/v1/users/ - Получение списка всех пользователей
GET /api/v1/users/{username}/  - Получение пользователя по username
```

### Виды пользователей
- Анонимный пользователь — может просматривать описания произведений, читать отзывы и комментарии.
- Аутентифицированный пользователь (user) — помимо прав доступа Анонимного пользователя, дополнительно может публиковать отзывы и ставить оценку произведениям, комментировать чужие отзывы; кроме того может редактировать и удалять свои отзывы и комментарии. Присваивается автоматически каждому новому пользователю.
- Модератор (moderator) — помимо прав доступа Аутентифицированного пользователя имеет право удалять любые отзывы и комментарии.
- Администратор (admin) — полные права на управление всем контентом. Может создавать и удалять произведения, категории и жанры, назначать роли пользователям.

### Регистрация нового пользователя

Регистрация нового пользователя:
```
POST /api/v1/auth/signup/
{
  "email": "string",
  "username": "string"
}
```
Поля email и username должны быть уникальными.

Получение JWT-токена:
```
POST /api/v1/auth/token/
{
  "username": "string",
  "confirmation_code": "string"
}
```

### Примеры работы с API для авторизованных пользователей
- Права доступа: admin.

Добавление категории:
```
POST /api/v1/categories/
{
  "name": "string",
  "slug": "string"
}
```

Удаление категории:
```
DELETE /api/v1/categories/{slug}/
```

Добавление жанра:
```
POST /api/v1/genres/
{
  "name": "string",
  "slug": "string"
}
```

Удаление жанра:
```
DELETE /api/v1/genres/{slug}/
```

Добавление произведения:
```
POST /api/v1/titles/
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```
Год выпуска произведения не может быть больше текущего.

Частичное обновление информации о произведении:
```
PATCH /api/v1/titles/{titles_id}/
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```

Удаление произведения:
```
DEL /api/v1/titles/{titles_id}/
```

- Права доступа: user.

Создание отзыва на произведение:
```
POST /api/v1/titles/{title_id}/reviews/
{
  "text": "string",
  "score": 1
}
```

Частичное обновление отзыва:
```
PATCH /api/v1/titles/{title_id}/reviews/{review_id}/
{
  "text": "string",
  "score": 1
}
```

Удаление отзыва:
```
DEL /api/v1/titles/{title_id}/reviews/{review_id}/
```