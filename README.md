# AI-enabled-business-management-platform

Enterprise FastAPI Microservice

An enterprise-grade FastAPI microservice built with clean architecture, JWT authentication, AI services, analytics, background workers, and Docker support. Designed for scalability, maintainability, and production readiness.

# Project Structure

python-service/
│
├── app/
│   ├── main.py                # FastAPI entry point
│   │
│   ├── core/                  # Core utilities
│   │   ├── config.py          # Environment settings
│   │   ├── security.py        # JWT, password hashing
│   │   └── logging.py         # Log configuration
│   │
│   ├── api/
│   │   └── v1/                # API versioning
│   │       ├── auth.py        # Authentication APIs
│   │       ├── users.py       # User APIs
│   │       ├── analytics.py   # Reports & KPIs
│   │       └── ai.py          # AI prediction APIs
│   │
│   ├── models/                # Database models
│   │   ├── user.py
│   │   └── business.py
│   │
│   ├── schemas/               # Pydantic schemas
│   │   ├── user.py
│   │   └── analytics.py
│   │
│   ├── services/              # Business logic
│   │   ├── ai_service.py
│   │   ├── report_service.py
│   │   └── automation.py
│   │
│   ├── db/                    # Database setup
│   │   ├── base.py
│   │   ├── session.py
│   │   └── init_db.py
│   │
│   ├── workers/               # Background workers
│   │   └── celery_worker.py
│   │
│   └── tests/                 # Test cases
│       ├── test_auth.py
│       ├── test_users.py
│       └── test_ai.py
│
├── alembic/                   # Database migrations
├── docker/                    # Docker configuration
│   ├── Dockerfile
│   └── docker-compose.yml
│
├── .env                       # Environment variables
├── requirements.txt
├── pyproject.toml
└── README.md

# 📦 app/ — Application Root
This is the main application package. Everything related to the backend logic lives here.
It keeps your project modular, testable, and production-ready.

# 🚀 main.py — FastAPI Entry Point

# Purpose:
1) Creates the FastAPI app instance
2) Loads configuration, middleware, routers
3) Starts the application
# Theory:
1) Acts as the composition root
2) No business logic here
3) Only wiring and initialization

# ⚙️ core/ — Core Utilities (Cross-Cutting Concerns)
Contains logic used across the entire application.
# config.py
1) Reads environment variables
2) Manages settings (DB URL, JWT secret, API keys)
# 📌 Theory:
Centralizes configuration to avoid hard-coding and enable 12-factor app principles.
# security.py
1)JWT token creation & validation
2) Password hashing & verification
3) Authentication helpers
# 📌 Theory:
Encapsulates security logic so it’s consistent, reusable, and auditable.

# logging.py
1) Logging format
2) Log levels
3) File/console handlers
# 📌 Theory:
Ensures observability and debugging in production systems.

# 🌐 api/ — API Layer (Presentation Layer)
1) v1/ — API Versioning
2) Supports backward compatibility
3) Enables safe API evolution
# 📌 Theory:
Versioning prevents breaking changes for existing clients.

# auth.py
1) Login / Register / Token refresh APIs
# 📌 Role:
Handles authentication flow, not business logic.
# users.py
1) User CRUD operations
2) Profile management
# 📌 Theory:
Controllers should be thin — request/response handling only.

# analytics.py
1) Reports
2) KPIs
3) Dashboards data
# 📌 Theory:
Separates read-heavy analytical endpoints from core entities.

# ai.py
1) ML inference endpoints
2) Prediction / recommendation APIs
# 📌 Theory:
Keeps AI logic isolated for scalability and model replacement.

# 🧠 models/ — Database Models (Persistence Layer)
Defines how data is stored in the database.

# user.py
1) User table structure
2) ORM mappings

# business.py
1)Business/domain entities
# 📌 Theory:
Models represent the source of truth for persisted data.

# 📄 schemas/ — Pydantic Schemas (Data Contracts)

Defines input/output validation.
# user.py
1) UserCreate, UserResponse, UserUpdate

# analytics.py
1) Report schemas
# 📌 Theory:
Separates data validation from database models → improves security and clarity.

# 🧩 services/ — Business Logic Layer
Where real application logic lives.
# ai_service.py
1) Model loading
2) Prediction logic

# report_service.py
1) Aggregations
2) Data transformations

# automation.py
1) Scheduled jobs
2) Workflow logic

# 📌 Theory:
Services:
1) Are reusable
2) Are testable
3) Do not depend on FastAPI

# 🗄️ db/ — Database Infrastructure
# base.py
1) SQLAlchemy Base class

# session.py
1) DB session creation
2) Dependency injection

# init_db.py
1) Initial data seeding
2) Migration helpers

# 📌 Theory:
Isolates DB plumbing from business logic.

# 🔄 workers/ — Background Processing
Handles async or long-running tasks.

# celery_worker.py
1) Email sending
2) Report generation
3) AI batch jobs
# 📌 Theory:
Improves performance by offloading heavy tasks from HTTP requests.
# Top-level files: 
Dockerfile, docker-compose.yml, requirements.txt, and a (previously empty) README.md that this file replaces.

# Quick prerequisites
Python 3.9+ (3.10/3.11 recommended)
pip
(optional) Docker & docker-compose for containerized runs
network access for the example external weather/pollution APIs

Create and activate a virtual environment (Windows/cmd example):

python -m venv python-service
python-service\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt

Notes about running: many modules use package-relative imports (e.g. from ..config.settings import ...). To ensure these imports work, run scripts as modules from the project root using python -m <module> or run via uvicorn with the Python module path.

# Configuration / Environment
Configuration lives in app/core/config/settings.py. Several environment variables are read via python-dotenv:

1) DATABASE_URL--database connect string (default
mysql+asyncmy://root:Siva%40219@localhost:3306/python_service
2) REDIS_URL-- Redis helps achieve performance, scalability, and statelessness
   (redis://localhost:6379/0
4) JWT_SECRET-- JWT(JSON Web Token) is a stateless authentication mechanism
   ( super-secret-jwt-key

Create a .env file at the repository root (optional) with custom values, for example:

DATABASE_URL=mysql+asyncmy://root:Siva%40219@localhost:3306/python_service
REDIS_URL=redis://localhost:6379/0
JWT_SECRET=super-secret-jwt-key

# Running the model server (FastAPI)

pip install uvicorn
uvicorn app.ai.model_server:app --host 0.0.0.0 --port 9000 --reload

# /Sending results to backend
[app/api/send_to_backend.py](http://127.0.0.1:8080/docs#/Auth/register_api_v1_auth_register_post) posts prediction results to the backend configured by BACKEND_API_URL. By default it points to 
python -m uvicorn app.main:app --reload --port 8080 — ensure a receiving endpoint is available or change the URL.
