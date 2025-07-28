# 4. Technical Assumptions

## 4.1 Repository Structure: **Monorepo**
All code, tests, documentation, and deployment configurations will exist in a single repository to simplify development and deployment.

## 4.2 Service Architecture: **Monolith**
Phase 1 implements a single service handling all proxy functionality. This follows KISS principle and avoids unnecessary complexity.

## 4.3 Testing Requirements
- **Unit Testing**: Required for all components (translators, handlers, utilities)
- **Integration Testing**: Required using Ollama SDK as client
- **End-to-End Testing**: Required with real OpenAI API (credentials via .env file)
- **Load Testing**: Basic load testing in Epic 3 to validate concurrent request handling
- **Performance Testing**: Not required in Phase 1 (LLM response time >> proxy overhead)

## 4.4 Additional Technical Assumptions
- **Language**: Python 3.12 (exact version required)
- **Framework**: FastAPI for REST API implementation
- **Deployment**: Docker containers with docker-compose for on-premises deployment
- **Configuration**: Environment variables via .env file (not committed to git)
- **Development Environment**: Linux (Ubuntu 22.04 LTS recommended)
- **Package Management**: venv (virtual environment)
- **OpenAI SDK**: Latest version for backend communication
- **Monitoring**: Basic health check endpoint only (no external service health checks)
- **Logging**: Structured JSON logging with configurable levels
