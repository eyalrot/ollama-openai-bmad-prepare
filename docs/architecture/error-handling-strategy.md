# Error Handling Strategy

## General Approach

- **Error Model:** Centralized error handling with consistent response format
- **Exception Hierarchy:** Custom exceptions for Ollama/OpenAI translation errors
- **Error Propagation:** Catch at handler level, translate, return appropriate HTTP status

## Logging Standards

- **Library:** structlog 24.1.0
- **Format:** JSON structured logging
- **Levels:** DEBUG, INFO, WARNING, ERROR
- **Required Context:**
  - Correlation ID: UUID per request
  - Service Context: endpoint, method, duration
  - User Context: No PII, only model name and request type

## Error Handling Patterns

### External API Errors

- **Retry Policy:** No automatic retry (KISS principle)
- **Circuit Breaker:** Not implemented in Phase 1
- **Timeout Configuration:** 30 seconds for OpenAI API calls
- **Error Translation:** Map OpenAI errors to Ollama error format

### Business Logic Errors

- **Custom Exceptions:** TranslationError, UnsupportedParameterError
- **User-Facing Errors:** Simple error messages without internal details
- **Error Codes:** HTTP status codes only (400, 404, 500, 502)

### Data Consistency

- **Transaction Strategy:** N/A (stateless service)
- **Compensation Logic:** N/A (no state modifications)
- **Idempotency:** All endpoints naturally idempotent
