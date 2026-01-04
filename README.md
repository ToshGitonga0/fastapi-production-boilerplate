# 🚀 FastAPI Production Boilerplate

<div align="center">

[![Python Version](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-00a393.svg)](https://fastapi.tiangolo.com)
[![Code style: ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

*A production-ready FastAPI boilerplate following SOLID principles, clean architecture, and industry best practices.*


[Features](#-features) •
[Quick Start](#-quick-start) •
[Architecture](#-architecture) •
[Documentation](#-documentation) •
[Contributing](#-contributing)

</div>

---

## ✨ Features

### 🏗️ **Architecture & Design**
- ✅ **Clean Architecture** - Repository → Service → Controller pattern
- ✅ **SOLID Principles** - Single Responsibility, Dependency Inversion, etc.
- ✅ **Dependency Injection** - Proper DI with FastAPI dependencies
- ✅ **Type Safety** - Full type hints with Pydantic v2

### 🛠️ **Tech Stack**
- ⚡ **FastAPI** - Modern, fast web framework
- 🗄️ **SQLModel** - SQL databases with Python objects (SQLAlchemy 2.0)
- 🔄 **Alembic** - Database migrations
- 🔐 **JWT Authentication** - Secure token-based auth
- 📦 **uv** - Ultra-fast Python package manager
- 🧪 **Pytest** - Comprehensive testing suite

### 🚀 **Developer Experience**
- 📝 **Beautiful CLI** - Rich console interface with `manage.py`
- 🔍 **Auto-generated Docs** - Interactive API documentation
- 🎨 **Code Quality** - Ruff for linting & formatting
- 🔄 **Hot Reload** - Instant development feedback
- 📊 **Database Seeding** - Pre-populated test data

### 🔒 **Production Ready**
- 🛡️ **Security Best Practices** - Password hashing, CORS, rate limiting ready
- 📈 **Scalable Design** - Async/await, connection pooling
- 🐛 **Error Handling** - Comprehensive exception handling
- 📝 **Logging** - Structured logging with loguru
- 🔍 **Monitoring Ready** - Sentry integration support

---

## 📁 Project Structure

```
fastapi-producion-boilerplate/
    │README.md
    │backend/
    ├── app/
    │   ├── api/
    │   │   ├── deps.py              # Dependency injection
    │   │   ├── main.py              # API router
    │   │   └── routes/
    │   │       ├── auth.py          # Authentication endpoints
    │   │       ├── users.py         # User management
    │   │       ├── address.py       # Address CRUD
    │   │       └── healthz.py       # Health checks
    │   ├── core/
    │   │   ├── config.py            # Settings & configuration
    │   │   ├── db.py                # Database session
    │   │   ├── security.py          # Password hashing, JWT
    │   │   └── logger.py            # Logging setup
    │   ├── models/
    │   │   └── models.py            # SQLModel database models
    │   ├── repositories/
    │   │   ├── base.py              # Generic CRUD operations
    │   │   ├── user.py              # User repository
    │   │   └── address.py           # Address repository
    │   ├── services/
    │   │   ├── users.py             # User business logic
    │   │   └── address.py           # Address business logic
    │   ├── alembic/                 # Database migrations
    │   └── main.py                  # FastAPI application
    ├── tests/                       # Comprehensive test suite
    ├── manage.py                    # CLI management tool
    ├── pyproject.toml               # Dependencies & config
    ├── ruff.toml                    # Linting rules
    └── .env.example                 # Environment template
```

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.11+**
- **PostgreSQL 14+**
- **uv** (Python package manager)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/ToshGitonga0/fastapi-production-boilerplate.git
   cd fastapi-production-boilerplate/backend
   ```

2. **Install uv** (if not already installed)
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

3. **Create virtual environment and install dependencies**
   ```bash
   uv venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   uv sync
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

5. **Set up the database**
   ```bash
   # Make sure PostgreSQL is running
   python manage.py db:migrate
   python manage.py db:seed
   ```

6. **Run the development server**
   ```bash
   python manage.py dev
   ```

7. **Visit the API documentation**
   - Interactive docs: http://localhost:8000/api/v1/docs
   - ReDoc: http://localhost:8000/api/v1/redoc


## 🎯 Management CLI

This project includes a powerful CLI for common tasks:

```bash
# Testing
python manage.py test              # Run all tests with coverage
python manage.py test:unit         # Run unit tests only
python manage.py test:integration  # Run integration tests only
python manage.py test:watch        # Run tests in watch mode

# Database
python manage.py db:migrate        # Create & apply migrations
python manage.py db:seed           # Seed test data
python manage.py db:reset          # Reset database (CAUTION!)

# Code Quality
python manage.py lint              # Run linters
python manage.py lint --fix        # Auto-fix issues
python manage.py format            # Format code

# Development
python manage.py dev               # Start dev server
python manage.py shell             # Interactive Python shell
python manage.py info              # Project statistics
python manage.py clean             # Clean generated files
```

---

## 🏛️ Architecture

### Repository Pattern

Separates data access logic from business logic:

```python
# Repository: Data access
class UserRepo(BaseRepo[User, UserCreate, UserUpdate]):
    async def get_user_by_email(self, email: str) -> User | None:
        # Database query logic
        ...

# Service: Business logic
class UserService:
    def __init__(self, repo: UserRepo):
        self.repo = repo
    
    async def create_user(self, user_in: UserCreate) -> UserPublic:
        # Validation, business rules
        if await self.repo.get_user_by_email(user_in.email):
            raise HTTPException(...)
        return await self.repo.create_user(user_in)

# Route: HTTP handling
@router.post("/")
async def create_user(user_in: UserCreate, service: UserServiceDep):
    return await service.create_user(user_in)
```

---

## 🧪 Testing

```bash
# Run all tests with coverage
python manage.py test

# Run specific test file
pytest tests/routes/test_users.py

# Run specific test
pytest tests/routes/test_users.py::test_create_user

# Stop on first failure
pytest -x

# Run with verbose output
pytest -v
```

---

## 📚 API Documentation

Once the server is running, visit:
- **Swagger UI**: http://localhost:8000/api/v1/docs
- **ReDoc**: http://localhost:8000/api/v1/redoc

---

## 🔐 Authentication

JWT-based authentication with role-based access control (RBAC):

```python
# Protected endpoint (any authenticated user)
@router.get("/me")
async def get_me(current_user: CurrentUser):
    return current_user

# Admin-only endpoint
@router.get("/users")
async def list_users(admin: CurrentAdmin):
    return await service.list_users()

# Multiple roles
@router.get("/reports")
async def get_reports(user: CurrentAdminOrManager):
    return await service.get_reports()
```

---

## 🌍 Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PROJECT_NAME` | Application name | FastAPI App |
| `ENVIRONMENT` | `local`, `staging`, `production` | local |
| `SECRET_KEY` | JWT secret key | changethis |
| `POSTGRES_SERVER` | Database host | localhost |
| `POSTGRES_USER` | Database user | postgres |
| `POSTGRES_PASSWORD` | Database password | changethis |
| `POSTGRES_DB` | Database name | fastapi_db |
| `ADMIN` | Admin email | admin@example.com |
| `ADMIN_PASSWORD` | Admin password | changethis |

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style

- Use `ruff` for linting and formatting
- Follow SOLID principles
- Write tests for new features
- Update documentation

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Sebastián Ramírez** - Creator of FastAPI, whose work inspired this boilerplate
- The FastAPI community for excellent documentation and support
- All contributors who help improve this project

---

## 🌟 Star History

If you find this project helpful, please consider giving it a star! ⭐

---

<div align="center">

**[⬆ Back to Top](#-fastapi-production-boilerplate)**

Made with ❤️ by [Tosh Gitonga]

</div>
