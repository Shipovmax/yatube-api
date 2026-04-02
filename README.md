# Проект API для Yatube

API для сервиса Yatube. Позволяет управлять постами, комментариями, группами и подписками.

## Технологии
- Python 3.9
- Django 3.2
- Django REST Framework
- SimpleJWT

## Установка

1. Клонируйте репозиторий:
`git clone ...`

2. Создайте и активируйте виртуальное окружение:
`python -m venv venv`
`source venv/bin/activate` (для Linux/macOS) или `venv\Scripts\activate` (для Windows)

3. Установите зависимости:
`pip install -r requirements.txt`

4. Выполните миграции:
`python manage.py migrate`

5. Запустите сервер:
`python manage.py runserver`

## Примеры запросов
`POST /api/v1/jwt/create/` — получение JWT-токена
`GET /api/v1/posts/` — получение списка постов