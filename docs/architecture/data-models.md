# Data Models

## Model: OllamaTagsResponse

**Purpose:** Represents the response format for /api/tags endpoint listing available models

**Key Attributes:**
- models: List[OllamaModel] - List of available models
- OllamaModel.name: str - Model identifier (e.g., "gpt-3.5-turbo")
- OllamaModel.modified_at: str - ISO timestamp of last modification
- OllamaModel.size: int - Model size in bytes (dummy value)
- OllamaModel.digest: str - Model digest hash (dummy value)

**Relationships:**
- Maps from OpenAI model list response
- No persistent storage required

## Model: OllamaGenerateRequest

**Purpose:** Represents the request format for /api/generate text completion endpoint

**Key Attributes:**
- model: str - Model name to use
- prompt: str - Text prompt for generation
- stream: bool - Whether to stream response (default: true)
- options: Dict[str, Any] - Generation parameters (temperature, max_tokens, etc.)

**Relationships:**
- Translates to OpenAI ChatCompletion request
- Parameter mapping required (e.g., num_predict → max_tokens)

## Model: OllamaChatRequest

**Purpose:** Represents the request format for /api/chat conversational endpoint

**Key Attributes:**
- model: str - Model name to use
- messages: List[OllamaChatMessage] - Conversation history
- stream: bool - Whether to stream response
- options: Dict[str, Any] - Generation parameters

**Relationships:**
- Translates to OpenAI ChatCompletion with message history
- Role mapping required (user/assistant/system)

## Model: OllamaEmbeddingRequest

**Purpose:** Represents the request format for /api/embeddings endpoint

**Key Attributes:**
- model: str - Model name to use
- prompt: str | List[str] - Text(s) to embed
- options: Dict[str, Any] - Embedding parameters

**Relationships:**
- Translates to OpenAI Embeddings request
- Batch processing support required
