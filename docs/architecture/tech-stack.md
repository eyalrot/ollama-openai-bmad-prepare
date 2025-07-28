# Tech Stack

## Cloud Infrastructure

- **Provider:** On-premises deployment only
- **Key Services:** Docker containers with docker-compose orchestration
- **Deployment Regions:** N/A (on-premises)

## Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|-----------|
| **Language** | Python | 3.12.3 | Primary development language | Specified in PRD, excellent async support |
| **Framework** | FastAPI | 0.109.0 | REST API framework | High performance, automatic OpenAPI docs, async support |
| **HTTP Server** | Uvicorn | 0.27.0 | ASGI server | FastAPI recommended, production-ready |
| **HTTP Client** | httpx | 0.26.0 | Async HTTP client | Modern async support, streaming capabilities |
| **OpenAI SDK** | openai | 1.12.0 | OpenAI API client | Official SDK, well-maintained |
| **Validation** | Pydantic | 2.5.3 | Data validation | FastAPI integration, automatic validation |
| **Logging** | structlog | 24.1.0 | Structured logging | JSON logging, better observability |
| **Testing** | pytest | 8.0.0 | Test framework | Python standard, great async support |
| **Testing** | pytest-asyncio | 0.23.3 | Async test support | Required for async endpoint testing |
| **Code Quality** | black | 24.1.1 | Code formatter | Consistent code style |
| **Code Quality** | flake8 | 7.0.0 | Linter | Code quality checks |
| **Code Quality** | mypy | 1.8.0 | Type checker | Static type checking |
| **Containerization** | Docker | 24.0.7 | Container runtime | Deployment standardization |
| **Orchestration** | docker-compose | 2.23.3 | Container orchestration | Simple multi-container management |
| **Environment** | python-dotenv | 1.0.0 | Environment management | .env file support |

**Important Note**: This table represents the DEFINITIVE technology choices. All implementation must use these exact versions to ensure consistency.
