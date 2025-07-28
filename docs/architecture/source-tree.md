# Source Tree

```plaintext
ollama-openai-proxy/
├── src/
│   ├── __init__.py
│   ├── main.py                    # FastAPI application entry point
│   ├── config.py                  # Configuration management
│   ├── models/                    # Pydantic models
│   │   ├── __init__.py
│   │   ├── ollama.py             # Ollama request/response models
│   │   └── openai.py             # OpenAI request/response models
│   ├── handlers/                  # Endpoint handlers
│   │   ├── __init__.py
│   │   ├── tags.py               # /api/tags handler
│   │   ├── generate.py           # /api/generate handler
│   │   ├── chat.py               # /api/chat handler
│   │   └── embeddings.py         # /api/embeddings handler
│   ├── translators/              # Translation logic
│   │   ├── __init__.py
│   │   ├── request.py            # Ollama → OpenAI translation
│   │   ├── response.py           # OpenAI → Ollama translation
│   │   └── parameters.py         # Parameter mapping
│   ├── clients/                  # External API clients
│   │   ├── __init__.py
│   │   └── openai_client.py     # OpenAI API wrapper
│   ├── utils/                    # Utilities
│   │   ├── __init__.py
│   │   ├── errors.py             # Error handling
│   │   └── logging.py            # Logging configuration
│   └── middleware/               # FastAPI middleware
│       ├── __init__.py
│       └── logging.py            # Request/response logging
├── tests/                        # Test suite
│   ├── __init__.py
│   ├── conftest.py              # Pytest configuration
│   ├── unit/                    # Unit tests
│   │   ├── test_translators.py
│   │   ├── test_handlers.py
│   │   └── test_models.py
│   ├── integration/             # Integration tests
│   │   ├── test_ollama_sdk.py  # Tests using Ollama SDK
│   │   └── test_endpoints.py
│   └── fixtures/                # Test fixtures
│       ├── ollama_requests.json
│       └── openai_responses.json
├── scripts/                     # Development scripts
│   ├── setup_dev.sh            # Development environment setup
│   ├── run_tests.sh            # Run test suite
│   └── run_local.sh            # Run local server
├── docs/                       # Documentation
│   ├── architecture.md         # This document
│   ├── api.md                 # API documentation
│   └── deployment.md          # Deployment guide
├── docker/                    # Docker configuration
│   ├── Dockerfile            # Production container
│   └── Dockerfile.dev        # Development container
├── .env.example              # Environment variable template
├── .gitignore               # Git ignore file
├── docker-compose.yml       # Container orchestration
├── requirements.txt         # Python dependencies
├── requirements-dev.txt     # Development dependencies
├── pyproject.toml          # Python project configuration
├── setup.py                # Package setup
└── README.md               # Project documentation
```
