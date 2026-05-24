# CLAUDE.md — Task Management REST API

This file configures the behavior of Claude Code in this project. Read carefully before taking any action.

---

## Project Context

This project is a **REST API** for task management (to-do list) built with **Python 3.11** and **FastAPI**. The API allows you to create, list, update and delete tasks, with JWT authentication, PostgreSQL database via SQLAlchemy and automatic documentation via Swagger.

**Main stack:**
-Python 3.11
- FastAPI 0.111
- SQLAlchemy 2.0 (ORM)
- PostgreSQL 15
- Alembic (migrations)
- Pydantic v2 (data validation)
- pytest (tests)
- Docker/Docker Compose

**Repository:**`github.com/nmaldaner/task-api`**Main branch:**`main`**Development branch:**`develop`---

## Development Commands

### Installation```bash
# Clonar e entrar no projeto
git clone https://github.com/nmaldaner/task-api.git
cd task-api

# Criar e ativar ambiente virtual
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows

# Instalar dependências
pip install -r requirements.txt
pip install -r requirements-dev.txt
```### Run the Project```bash
# Subir banco de dados com Docker
docker compose up -d postgres

# Rodar migrations
alembic upgrade head

# Iniciar servidor de desenvolvimento
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# Iniciar com configurações de produção
uvicorn app.main:app --workers 4 --host 0.0.0.0 --port 8000
```### Tests```bash
# Rodar todos os testes
pytest

# Rodar com cobertura
pytest --cov=app --cov-report=html

# Rodar apenas testes de um módulo específico
pytest tests/test_tasks.py -v

# Rodar testes marcados como unitários
pytest -m unit

# Rodar testes e parar no primeiro erro
pytest -x
```### Migrations```bash
# Criar nova migration
alembic revision --autogenerate -m "descricao_da_mudanca"

# Aplicar todas as migrations
alembic upgrade head

# Reverter última migration
alembic downgrade -1

# Ver histórico de migrations
alembic history
```### Other Useful Commands```bash
# Lint e formatação
ruff check app/
ruff format app/
mypy app/

# Verificar tipos
mypy app/ --strict

# Gerar requirements.txt atualizado
pip freeze > requirements.txt
```---

## Code Conventions

### Nomenclature

- **Variables and functions:**`snake_case`— e.g.:`get_user_tasks`, `task_id`- **Classes:**`PascalCase`— e.g.:`TaskCreate`, `UserResponse`- **Constants:**`UPPER_SNAKE_CASE`— e.g.:`MAX_TASKS_PER_PAGE`, `DEFAULT_TIMEOUT`- **Files:**`snake_case.py`— e.g.:`task_router.py`, `auth_service.py`- **Endpoints:** kebab-case in the URL — e.g.`/api/v1/user-tasks`### Type Hints

All code MUST use type hints. Use the module`typing`when necessary.```python
# CORRETO
def get_task(task_id: int, db: Session) -> TaskResponse | None:
    ...

# ERRADO — sem type hints
def get_task(task_id, db):
    ...
```###Docstrings

Use Google Style format for docstrings in all public functions:```python
def create_task(task_data: TaskCreate, db: Session) -> TaskResponse:
    """Cria uma nova tarefa no banco de dados.

    Args:
        task_data: Dados da tarefa a ser criada, validados pelo Pydantic.
        db: Sessão do banco de dados injetada pelo FastAPI.

    Returns:
        TaskResponse com os dados da tarefa criada, incluindo o ID gerado.

    Raises:
        HTTPException: Se o usuário não tiver permissão ou os dados forem inválidos.
    """
    ...
```### Import Organization

Follow the order: stdlib > third-party > local, separated by a blank line:```python
# 1. Stdlib
from datetime import datetime
from typing import Optional

# 2. Third-party
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session

# 3. Local
from app.database import get_db
from app.models.task import Task
from app.schemas.task import TaskCreate, TaskResponse
```### Error Handling

Always use`HTTPException`from FastAPI with semantic status codes:```python
raise HTTPException(
    status_code=404,
    detail="Tarefa não encontrada"
)
```---

## Claude Code's Rules of Behavior

### Git and Versioning

- **NEVER commit directly to`main` ou `develop`**
- Always create a descriptive branch before making any changes:`feat/descricao`, `fix/descricao`, `refactor/descricao`- Commit messages in Portuguese, in the imperative: "Add listing endpoint", "Fix bug in date filter"
- Always run tests before committing:`pytest`- Never use`git push --force`— if necessary, ask the user

### Branch Creation```bash
# Para novas features
git checkout -b feat/nome-da-feature

# Para correções de bug
git checkout -b fix/descricao-do-bug

# Para refatorações
git checkout -b refactor/descricao
```### When Modifying Code

1. Always read the complete file before editing
2. Do not remove existing comments without an explicit reason
3. Maintain the code style already present in the file
4. After editing any Python file, run:`ruff check app/`to check lint
5. After modifying models or schemas, check if migrations need to be updated

### When Creating New Files

- New routers must be registered at`app/main.py`- New models must inherit from`Base` em `app/database.py`- New schemas must follow the standard`NomeCreate`, `NomeUpdate`, `NomeResponse`- Every new file must have corresponding tests in`tests/`### Never Do

- Never expose`SECRET_KEY` ou `DATABASE_URL`in code
- Never remove already applied migration files
- Never do`DROP TABLE` ou `DELETE FROM`without explicit user confirmation
- Never install dependencies without adding to`requirements.txt`- Never change production configuration files without notifying the user

---

## Project Architecture```
task-api/
├── app/
│   ├── __init__.py
│   ├── main.py               # Entry point, registro de routers
│   ├── database.py           # Configuração do SQLAlchemy, get_db()
│   ├── config.py             # Settings via pydantic-settings
│   ├── dependencies.py       # Dependências compartilhadas (auth, etc.)
│   │
│   ├── models/               # Modelos SQLAlchemy (tabelas do banco)
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── task.py
│   │
│   ├── schemas/              # Schemas Pydantic (request/response)
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── task.py
│   │
│   ├── routers/              # Endpoints organizados por recurso
│   │   ├── __init__.py
│   │   ├── auth.py           # /api/v1/auth/*
│   │   ├── users.py          # /api/v1/users/*
│   │   └── tasks.py          # /api/v1/tasks/*
│   │
│   └── services/             # Lógica de negócio
│       ├── __init__.py
│       ├── auth_service.py
│       └── task_service.py
│
├── tests/
│   ├── conftest.py           # Fixtures compartilhadas
│   ├── test_auth.py
│   ├── test_users.py
│   └── test_tasks.py
│
├── migrations/               # Alembic migrations
│   ├── env.py
│   ├── script.py.mako
│   └── versions/
│
├── .env.example              # Template de variáveis de ambiente
├── .env                      # Variáveis locais (NÃO commitar)
├── alembic.ini
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── requirements-dev.txt
└── CLAUDE.md                 # Este arquivo
```---

## Environment Variables

Copy`.env.example`to`.env`before running the project. The file`.env`should never be committed.```bash
# Banco de dados
DATABASE_URL=postgresql://user:password@localhost:5432/taskdb
DATABASE_URL_TEST=postgresql://user:password@localhost:5432/taskdb_test

# Segurança
SECRET_KEY=your-super-secret-key-change-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Aplicação
ENVIRONMENT=development
DEBUG=true
LOG_LEVEL=info
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:5173

# Opcionais
SENTRY_DSN=
REDIS_URL=redis://localhost:6379/0
```---

## Main URLs and Endpoints

**Interactive documentation (Swagger):** http://localhost:8000/docs
**Alternative documentation (ReDoc):** http://localhost:8000/redoc
**Health check:** http://localhost:8000/health

### API endpoints

| Method | Endpoint | Description |
|--------|-----------------------------|-----------------------------------|
| POST | /api/v1/auth/register | Register new user |
| POST | /api/v1/auth/login | Authenticate and obtain JWT |
| GET | /api/v1/users/me | Authenticated user data |
| GET | /api/v1/tasks | List user tasks |
| POST | /api/v1/tasks | Create new task |
| GET | /api/v1/tasks/{id} | Search task by ID |
| PATCH | /api/v1/tasks/{id} | Partially update task |
| DELETE | /api/v1/tasks/{id} | Delete task |
| PATCH | /api/v1/tasks/{id}/complete | Mark task as completed |

### Authentication

All endpoints (except`/auth/*` e `/health`) require the header:```
Authorization: Bearer <seu-jwt-token>
```
