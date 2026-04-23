# Yatube API

A RESTful API for the Yatube social blogging platform.  
Supports posts, comments, groups, subscriptions, and JWT authentication.

---

## Features

- **Posts** — create, read, update, delete; image upload support; LimitOffset pagination
- **Comments** — nested under posts; author-only edit/delete
- **Groups** — read-only catalog of post communities
- **Follows** — subscribe to authors; search subscriptions by username; no self-follow
- **JWT authentication** — `djangorestframework-simplejwt`; obtain/refresh/verify via `/api/v1/jwt/`
- **Permissions** — authenticated users can write; read-only for anonymous; only the author can modify their own content
- **OpenAPI / ReDoc** — schema served at `/redoc/`

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.9 |
| Framework | Django 3.2, Django REST Framework 3.12 |
| Auth | SimpleJWT 4.7 |
| Filtering | django-filter 2.4, DRF SearchFilter |
| Image processing | Pillow 9.3 |
| Testing | pytest, pytest-django |

---

## Quick Start

```bash
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt

cd yatube_api
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

API available at `http://127.0.0.1:8000/api/v1/`  
ReDoc schema at `http://127.0.0.1:8000/redoc/`

---

## Authentication

Obtain a JWT token pair:

```http
POST /api/v1/jwt/create/
Content-Type: application/json

{"username": "your_user", "password": "your_password"}
```

Use the access token in subsequent requests:

```http
Authorization: Bearer <access_token>
```

Refresh: `POST /api/v1/jwt/refresh/`  
Verify: `POST /api/v1/jwt/verify/`

---

## API Endpoints

| Method | Endpoint | Description | Auth required |
|--------|----------|-------------|---------------|
| `GET` | `/api/v1/posts/` | List posts (paginated) | No |
| `POST` | `/api/v1/posts/` | Create post | Yes |
| `GET` | `/api/v1/posts/{id}/` | Get post | No |
| `PUT/PATCH` | `/api/v1/posts/{id}/` | Update post | Author only |
| `DELETE` | `/api/v1/posts/{id}/` | Delete post | Author only |
| `GET/POST` | `/api/v1/posts/{id}/comments/` | List / create comments | Read: No, Write: Yes |
| `GET/PUT/PATCH/DELETE` | `/api/v1/posts/{id}/comments/{id}/` | Comment detail | Author only for write |
| `GET` | `/api/v1/groups/` | List groups | No |
| `GET` | `/api/v1/groups/{id}/` | Group detail | No |
| `GET/POST` | `/api/v1/follow/` | List subscriptions / subscribe | Yes |

Pagination on posts: `?limit=10&offset=0`  
Search follows: `?search=username`

---

## Running Tests

```bash
cd yatube_api
pytest
```

---

## Project Structure

```
yatube_api/
├── api/
│   ├── models.py       # (empty — models live in posts/)
│   ├── serializers.py  # PostSerializer, CommentSerializer, GroupSerializer, FollowSerializer
│   ├── views.py        # ViewSets for all resources
│   ├── permissions.py  # IsAuthorOrReadOnly
│   └── urls.py         # Router registration
├── posts/
│   └── models.py       # Post, Comment, Group, Follow
└── yatube_api/
    ├── settings.py
    └── urls.py
```

---

## Author

- GitHub: [Shipovmax](https://github.com/Shipovmax)
- Email: shipov.max@icloud.com
