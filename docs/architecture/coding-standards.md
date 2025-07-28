# Coding Standards

## Core Standards

- **Languages & Runtimes:** Python 3.12.3
- **Style & Linting:** black (formatting), flake8 (linting), mypy (type checking)
- **Test Organization:** tests/{unit,integration}/test_*.py pattern

## Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Files | snake_case | request_translator.py |
| Classes | PascalCase | RequestTranslator |
| Functions | snake_case | translate_request |
| Constants | UPPER_SNAKE | MAX_RETRIES |
| Test files | test_*.py | test_translator.py |

## Critical Rules

- **Logging:** Never use print() - always use structured logger
- **Secrets:** Never log API keys or sensitive data
- **Error Handling:** All endpoints must have explicit error handling
- **Type Hints:** All functions must have type hints
- **Docstrings:** All public functions must have docstrings
- **Async:** Use async/await for all I/O operations
