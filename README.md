# ✅ Task Management API

A JWT-authenticated REST API for managing personal tasks, built with **FastAPI** and **SQLAlchemy**. Each user only sees and manages their own tasks.

![Status](https://img.shields.io/badge/status-active-success)
![Python](https://img.shields.io/badge/python-3.11%2B-blue)
![License](https://img.shields.io/badge/license-MIT-blue)

## ✨ Features

- 🔐 User registration and login with JWT access tokens
- 🔑 Passwords hashed with bcrypt (never stored in plain text)
- ✅ Full CRUD for tasks: title, description, priority, due date, completed status
- 👤 Tasks are scoped per user — you can only see and edit your own
- 🧪 Test suite with pytest covering auth and task endpoints
- 📚 Auto-generated interactive API docs (Swagger UI + ReDoc)
- 🗄️ SQLite by default (zero setup), swappable for PostgreSQL via one env variable

## 🛠️ Tech Stack

- **Framework:** FastAPI
- **ORM:** SQLAlchemy 2.0
- **Auth:** python-jose (JWT) + passlib (bcrypt)
- **Validation:** Pydantic v2
- **Testing:** pytest + httpx TestClient

## 📁 Folder Structure

```
task-api/
├── app/
│   ├── core/
│   │   ├── config.py       # Settings (env-based)
│   │   ├── security.py     # Password hashing + JWT helpers
│   │   └── deps.py         # get_current_user dependency
│   ├── models/
│   │   ├── user.py         # User SQLAlchemy model
│   │   └── task.py         # Task SQLAlchemy model
│   ├── schemas/
│   │   ├── user.py         # Pydantic schemas for auth
│   │   └── task.py         # Pydantic schemas for tasks
│   ├── crud/
│   │   ├── user.py         # DB operations for users
│   │   └── task.py         # DB operations for tasks
│   ├── routers/
│   │   ├── auth.py         # /api/auth/register, /api/auth/login
│   │   └── tasks.py        # /api/tasks CRUD
│   ├── database.py         # Engine, session, Base
│   └── main.py             # FastAPI app entry point
├── tests/
│   ├── conftest.py         # Test DB fixtures
│   ├── test_auth.py
│   └── test_tasks.py
├── requirements.txt
├── .env.example
└── .gitignore
```

## 🚀 Getting Started

### Prerequisites

- Python 3.11+

### 1. Clone and install

```bash
git clone https://github.com/your-username/task-api.git
cd task-api
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Configure environment

```bash
cp .env.example .env
```

Generate a real secret key instead of the placeholder:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

### 3. Run the server

```bash
uvicorn app.main:app --reload
```

- API: `http://localhost:8000`
- Interactive docs: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

Tables are created automatically on first run (SQLite file `tasks.db`).

### 4. Run tests

```bash
pytest -v
```

## 📡 API Endpoints

### Auth

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Create a new user |
| POST | `/api/auth/login` | Get a JWT access token (OAuth2 password flow) |

### Tasks (require `Authorization: Bearer <token>`)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/tasks` | List your tasks (optional `?completed=true/false`) |
| GET | `/api/tasks/{id}` | Get a single task |
| POST | `/api/tasks` | Create a task |
| PUT | `/api/tasks/{id}` | Update a task |
| DELETE | `/api/tasks/{id}` | Delete a task |

## 🔍 Example Usage

```bash
# Register
curl -X POST http://localhost:8000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "alice", "email": "alice@example.com", "password": "secret123"}'

# Login
curl -X POST http://localhost:8000/api/auth/login \
  -d "username=alice&password=secret123"

# Create a task (replace TOKEN with the access_token from login)
curl -X POST http://localhost:8000/api/tasks \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title": "Finish README", "priority": "high"}'
```

## 🔑 Environment Variables

| Variable | Description |
|---|---|
| `DATABASE_URL` | SQLAlchemy connection string (default: local SQLite file) |
| `SECRET_KEY` | Secret used to sign JWTs — must be kept private |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | Token lifetime in minutes |

To switch to PostgreSQL, just change `DATABASE_URL`, e.g.:
```
DATABASE_URL=postgresql://user:password@localhost:5432/taskdb
```
(and add `psycopg2-binary` to `requirements.txt`)

## 🧪 Possible Improvements

- Task categories/tags and filtering
- Pagination on the task list
- Refresh tokens
- Alembic migrations instead of `create_all`
- Rate limiting on login/register

## 📄 License

MIT — free to use and modify.
