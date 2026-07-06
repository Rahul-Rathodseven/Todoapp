# TodoApp

A FastAPI-based todo application with JWT authentication, server-rendered pages, SQLite storage, Alembic migrations, and automated tests.

## Features

- User registration and login
- JWT-based authentication
- Create, read, update, and delete personal todos
- Admin-only todo management endpoints
- User profile endpoints for password and phone number updates
- Jinja2 templates for login, registration, and todo pages
- SQLite database with Alembic migration support
- Pytest test suite for core routes and auth flows

## Project Structure

```text
TodoApp/
├── main.py
├── database.py
├── models.py
├── routers/
├── templates/
├── static/
├── alembic/
└── test/
```

## Requirements

- Python 3.10+
- `pip`

## Installation

Create and activate a virtual environment, then install the packages used by the project:

```bash
python -m venv .venv
source .venv/bin/activate
pip install fastapi "uvicorn[standard]" sqlalchemy alembic jinja2 \
  "python-jose[cryptography]" "passlib[bcrypt]" python-multipart pytest
```

## Running the App

This project uses package-relative imports such as `from .models import Base` and template/static paths like `TodoApp/templates`, so start the server from the parent directory of `TodoApp`.

```bash
cd ..
uvicorn TodoApp.main:app --reload
```

Open these URLs in your browser:

- App: `http://127.0.0.1:8000/`
- Swagger UI: `http://127.0.0.1:8000/docs`
- Health check: `http://127.0.0.1:8000/healthy`

## Database

The app uses SQLite by default and creates `todosapp.db` locally.

To apply migrations:

```bash
alembic upgrade head
```

## Main Routes

### Auth

- `POST /auth/` - register a new user
- `POST /auth/token` - get an access token
- `GET /auth/login-page` - login page
- `GET /auth/register-page` - registration page

### Todos

- `GET /todos/` - list the authenticated user's todos
- `GET /todos/todo/{todo_id}` - get one todo
- `POST /todos/todo` - create a todo
- `PUT /todos/todo/{todo_id}` - update a todo
- `DELETE /todos/todo/{todo_id}` - delete a todo
- `GET /todos/todo-page` - todo dashboard page

### User

- `GET /user/` - get current user details
- `PUT /user/password` - change password
- `PUT /user/phonenumber/{phone_number}` - update phone number

### Admin

- `GET /admin/todo` - list all todos
- `DELETE /admin/todo/{todo_id}` - delete any todo

## Running Tests

Run the test suite with:

```bash
pytest
```

## Notes

- The default database URL is defined in `database.py` as `sqlite:///./todosapp.db`.
- The root route redirects to `/todos/todo-page`.
- Authentication tokens are created in `routers/auth.py` and are valid for 20 minutes.
