# Test Strategy and Standards

## Testing Philosophy

- **Approach:** Test-after development with comprehensive coverage
- **Coverage Goals:** 80% minimum coverage, 100% for translation logic
- **Test Pyramid:** 60% unit, 30% integration, 10% end-to-end

## Test Types and Organization

### Unit Tests

- **Framework:** pytest 8.0.0
- **File Convention:** test_<module_name>.py
- **Location:** tests/unit/
- **Mocking Library:** pytest-mock
- **Coverage Requirement:** 80% minimum

**AI Agent Requirements:**
- Generate tests for all public methods
- Cover edge cases and error conditions
- Follow AAA pattern (Arrange, Act, Assert)
- Mock all external dependencies

### Integration Tests

- **Scope:** API endpoint testing with Ollama SDK client
- **Location:** tests/integration/
- **Test Infrastructure:**
  - **OpenAI API:** Mock responses via pytest fixtures
  - **HTTP Server:** Test client via FastAPI TestClient

### End-to-End Tests

- **Framework:** pytest 8.0.0 with real OpenAI API
- **Scope:** Full request/response flow with actual API
- **Environment:** Requires .env with valid OPENAI_API_KEY
- **Test Data:** Minimal API calls to control costs

## Test Data Management

- **Strategy:** Fixtures from Postman collection examples
- **Fixtures:** tests/fixtures/*.json
- **Factories:** Not required (simple data structures)
- **Cleanup:** No cleanup needed (stateless)

## Continuous Testing

- **CI Integration:** Run all tests before deployment
- **Performance Tests:** Basic load test with locust (Epic 3)
- **Security Tests:** Dependency scanning with pip-audit
