# Product Requirements Document: Ollama-to-OpenAI Proxy Service

## 1. Executive Summary

### 1.1 Purpose
This document defines the requirements for a proxy service that enables applications using the Ollama Python SDK to seamlessly connect to OpenAI-compatible endpoints (OpenAI, OpenRouter, LiteLLM) without code refactoring.

### 1.2 Vision
Provide a permanent, drop-in replacement solution for teams transitioning from Ollama servers to OpenAI-compatible services while preserving their existing codebase investments.

### 1.3 Success Criteria
- Zero code changes required in applications using Ollama SDK
- Support for core Ollama API endpoints used in production
- Minimal latency overhead compared to direct LLM response times
- Reliable streaming support for real-time applications

## 2. Product Overview

### 2.1 Problem Statement
Organizations using Ollama SDK in their applications face significant refactoring costs when migrating to OpenAI-compatible services. This creates friction in adopting more scalable or feature-rich LLM providers.

### 2.2 Solution
A lightweight proxy service that:
- Accepts Ollama SDK API calls
- Translates requests to OpenAI API format
- Forwards to OpenAI-compatible endpoints
- Translates responses back to Ollama format

### 2.3 Key Benefits
- **Zero Migration Cost**: No code changes required
- **Provider Flexibility**: Switch between OpenAI, OpenRouter, LiteLLM
- **Future-Proof**: Maintain compatibility as providers evolve

## 3. Functional Requirements

### 3.1 Supported Endpoints

#### 3.1.1 `/api/tags` - List Models
- **Purpose**: Return available models in Ollama format
- **Behavior**: 
  - Query configured OpenAI-compatible endpoint for available models
  - Transform response to Ollama's model list format
  - Provide dummy metadata for missing fields (size, digest, etc.)
- **Response Format**: Match Ollama's tags response structure

#### 3.1.2 `/api/generate` - Text Generation
- **Purpose**: Generate text completions
- **Features**:
  - Support streaming and non-streaming modes
  - Translate Ollama parameters to OpenAI equivalents where possible
  - Log warnings for unmappable Ollama-specific parameters
  - Map generation parameters (temperature, top_p, etc.)
- **Parameter Handling**:
  - `num_predict` → `max_tokens`
  - `repeat_penalty` → `frequency_penalty`
  - `top_k` → Log warning (OpenAI doesn't support)
  - `seed` → `seed` (if supported by model)
- **Integration Test**: Must pass Ollama SDK's generate tests

#### 3.1.3 `/api/chat` - Chat Completions
- **Purpose**: Handle conversational interactions
- **Features**:
  - Support message history format conversion
  - Handle system/user/assistant role mapping
  - Support streaming responses
- **Integration Test**: Must pass Ollama SDK's chat tests

#### 3.1.4 `/api/embeddings` - Text Embeddings
- **Purpose**: Generate text embeddings
- **Features**:
  - Handle batch embedding requests
  - Format embedding responses correctly
- **Integration Test**: Must pass Ollama SDK's embedding tests

### 3.2 Configuration

#### 3.2.1 Environment Variables
Configuration via `.env` file:
```
OPENAI_API_BASE_URL=https://api.openai.com/v1
OPENAI_API_KEY=sk-...
DEFAULT_MODEL=gpt-3.5-turbo
LOG_LEVEL=INFO
PORT=11434  # Ollama default port
```

#### 3.2.2 Model Configuration
- Phase 1 focuses on OpenAI models only (gpt-3.5-turbo, gpt-4, etc.)
- Users specify OpenAI model names directly in their requests
- No automatic model name mapping or translation

### 3.3 Error Handling

#### 3.3.1 Error Code Mapping
- Map OpenAI error responses to appropriate Ollama error codes
- Preserve error messages while ensuring SDK compatibility
- Priority: Return errors that Ollama SDK can handle gracefully

#### 3.3.2 Common Error Scenarios
- Invalid API key → 401 Unauthorized
- Model not found → 404 Not Found
- Rate limiting → 429 Too Many Requests
- Server errors → 500 Internal Server Error

## 4. Non-Functional Requirements

### 4.1 Performance
- **Latency**: Proxy overhead < 50ms (excluding LLM response time)
- **Concurrency**: Support minimum 100 concurrent requests
- **Streaming**: Maintain real-time streaming with minimal buffering

### 4.2 Reliability
- **Uptime**: Target 99.9% availability
- **Error Recovery**: Graceful handling of upstream failures
- **Timeout Handling**: Configurable request timeouts

### 4.3 Compatibility
- **Ollama SDK**: Support latest stable version
- **Python**: Support Python 3.8+
- **OpenAI API**: Compatible with OpenAI API v1

## 5. Technical Architecture

### 5.1 High-Level Components
1. **FastAPI Server**: Ollama-compatible REST endpoints
2. **Translation Layer**: Request/response format conversion
3. **OpenAI Client**: Communication with OpenAI-compatible services

### 5.2 Architecture Principles (from Architecture Document)
- **KISS Principle**: Keep solutions simple, avoid over-engineering
- **YAGNI**: Build only what's currently needed
- **Story-Driven**: Each story implements only its defined scope
- **Test-First**: No story is complete without passing tests

*Note: Detailed architecture specifications are provided in the accompanying Architecture document.*

### 5.3 Critical First Step: API Contract Analysis
**IMPORTANT**: Before any endpoint implementation, the first story MUST be:
- Obtain Ollama API specification (swagger/OpenAPI)
- Generate Pydantic models from the specification
- Validate against Ollama SDK behavior
- Create test fixtures

This is a BLOCKER - no endpoint development can begin until this is complete.

### 5.4 Testing Strategy
- **Integration Tests**: Use Ollama Python SDK as client
- **Compatibility Tests**: Verify against real Ollama server responses
- **Load Tests**: Validate concurrency requirements

## 6. Development Phases

### 6.1 Phase 1 - Core Functionality
**Timeline**: 4-6 weeks

**Prerequisites**:
- API Contract Analysis must be completed first (see Section 5.3)

**Deliverables**:
- Implement four core endpoints
- Basic error handling
- Configuration via environment variables
- Integration tests with Ollama SDK
- Documentation and deployment guide

### 6.2 Phase 2 - Monitoring & Optimization
**Timeline**: 2-3 weeks
- Add monitoring and metrics:
  - Request/response latency
  - Error rates by endpoint
  - Model usage statistics
  - Upstream API health
- Performance optimizations
- Enhanced error handling
- Request/response logging options

## 7. Product Owner Responsibilities

### 7.1 EPIC and Story Creation
The Product Owner is responsible for:
- Breaking this PRD into EPICs and Stories
- Ensuring API Contract Analysis is the first story
- Referencing Architecture document sections in stories
- Enforcing KISS principle in story scope
- Preventing feature creep

### 7.2 Story Prioritization
- API Contract Analysis is a BLOCKER - must complete first
- Implement endpoints one at a time in order
- Apply learnings from each endpoint to the next
- No parallel endpoint development

## 7. Success Metrics

### 7.1 Technical Metrics
- 100% pass rate on Ollama SDK integration tests
- < 50ms average proxy overhead
- Zero code changes required in client applications

### 7.2 Adoption Metrics
- Number of legacy applications successfully migrated
- Reduction in migration time vs. refactoring
- User satisfaction scores

## 8. Constraints and Assumptions

### 8.1 Constraints
- No support for Ollama-specific model management (pull, push, delete)
- Single OpenAI endpoint per instance (Phase 1)
- Phase 1 supports only OpenAI models
- No caching of responses

### 8.2 Assumptions
- Users will provide valid OpenAI model names (gpt-3.5-turbo, gpt-4, etc.)
- Ollama SDK version compatibility maintained
- OpenAI API format remains stable
- Proxy overhead is negligible compared to LLM response time

## 9. Implementation Notes

### 9.1 Parameter Translation Policy
- **Untranslatable Parameters**: Log warnings for any Ollama-specific parameters that cannot be mapped to OpenAI equivalents
- **Logging Format**: `WARNING: Ollama parameter '{param_name}' has no OpenAI equivalent, ignoring`

### 9.2 Development Process
- Open questions will be addressed during development as they arise
- Developer discretion for implementation details not specified in this PRD
- Technical decisions to be documented in the Architecture PRD

### 9.3 Version Compatibility
- No compatibility matrix required for Phase 1
- Focus on supporting current Ollama SDK version
- Future phases may introduce version compatibility tracking if needed

## 10. Appendix

### 10.1 Reference Documentation
- [Ollama API Documentation](https://github.com/ollama/ollama/blob/main/docs/api.md)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [LiteLLM Documentation](https://docs.litellm.ai/)
- [OpenRouter API Docs](https://openrouter.ai/docs)

### 10.2 Related Documents
- Architecture PRD (to be created)
- API Mapping Specification (to be created)
- Integration Test Plan (to be created)