# External APIs

## OpenAI API

- **Purpose:** Provides access to GPT models for text generation, chat, and embeddings
- **Documentation:** https://platform.openai.com/docs/api-reference
- **Base URL(s):** https://api.openai.com/v1
- **Authentication:** Bearer token via Authorization header
- **Rate Limits:** Varies by account tier (not enforced in Phase 1)

**Key Endpoints Used:**
- `GET /models` - List available models
- `POST /chat/completions` - Create chat completion (used for both generate and chat)
- `POST /embeddings` - Create embeddings

**Integration Notes:** No rate limiting or circuit breaker in Phase 1 (KISS principle). API key via environment variable. Streaming responses via Server-Sent Events.
