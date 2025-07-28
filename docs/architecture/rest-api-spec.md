# REST API Spec

```yaml
openapi: 3.0.0
info:
  title: Ollama-to-OpenAI Proxy API
  version: 1.0.0
  description: Proxy service translating Ollama API calls to OpenAI
servers:
  - url: http://localhost:11434
    description: Default Ollama port

paths:
  /api/tags:
    get:
      summary: List available models
      operationId: listModels
      responses:
        '200':
          description: List of available models
          content:
            application/json:
              schema:
                type: object
                properties:
                  models:
                    type: array
                    items:
                      type: object
                      properties:
                        name:
                          type: string
                          example: "gpt-3.5-turbo"
                        modified_at:
                          type: string
                          format: date-time
                        size:
                          type: integer
                          example: 1000000
                        digest:
                          type: string
                          example: "sha256:dummy"

  /api/generate:
    post:
      summary: Generate text completion
      operationId: generateCompletion
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - model
                - prompt
              properties:
                model:
                  type: string
                  example: "gpt-3.5-turbo"
                prompt:
                  type: string
                  example: "Why is the sky blue?"
                stream:
                  type: boolean
                  default: true
                options:
                  type: object
                  properties:
                    temperature:
                      type: number
                      example: 0.7
                    num_predict:
                      type: integer
                      example: 100
      responses:
        '200':
          description: Generated text
          content:
            application/json:
              schema:
                type: object
                properties:
                  model:
                    type: string
                  created_at:
                    type: string
                    format: date-time
                  response:
                    type: string
                  done:
                    type: boolean

  /api/chat:
    post:
      summary: Chat conversation
      operationId: chatCompletion
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - model
                - messages
              properties:
                model:
                  type: string
                messages:
                  type: array
                  items:
                    type: object
                    properties:
                      role:
                        type: string
                        enum: [system, user, assistant]
                      content:
                        type: string
                stream:
                  type: boolean
                  default: true
      responses:
        '200':
          description: Chat response

  /api/embeddings:
    post:
      summary: Generate embeddings
      operationId: createEmbeddings
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - model
                - prompt
              properties:
                model:
                  type: string
                  example: "text-embedding-ada-002"
                prompt:
                  oneOf:
                    - type: string
                    - type: array
                      items:
                        type: string
      responses:
        '200':
          description: Embeddings response

  /health:
    get:
      summary: Health check
      operationId: healthCheck
      responses:
        '200':
          description: Service is healthy
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                    example: "healthy"
                  version:
                    type: string
                    example: "1.0.0"
```
