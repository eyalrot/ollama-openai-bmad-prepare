# Infrastructure and Deployment

## Infrastructure as Code

- **Tool:** Docker 24.0.7 & docker-compose 2.23.3
- **Location:** `docker/` and `docker-compose.yml`
- **Approach:** Container-based deployment with environment variable configuration

## Deployment Strategy

- **Strategy:** Rolling update with Docker Compose
- **CI/CD Platform:** Manual deployment (automated CI/CD out of scope for Phase 1)
- **Pipeline Configuration:** `scripts/deploy.sh` (to be created)

## Environments

- **Development:** Local Docker container with hot-reload enabled - Port 11434, debug logging
- **Testing:** Docker container with test configuration - Isolated network, test OpenAI credentials
- **Production:** Optimized Docker container - Port 11434, JSON structured logging, restart policy

## Environment Promotion Flow

```
Development (local) → Testing (docker) → Production (docker)
    ↓                    ↓                    ↓
  Manual              Manual              Manual
  Testing             Validation          Deployment
```

## Rollback Strategy

- **Primary Method:** Docker image rollback to previous version
- **Trigger Conditions:** Health check failures, high error rate, manual intervention
- **Recovery Time Objective:** < 5 minutes
