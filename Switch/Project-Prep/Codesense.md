# Codesense

## RAG (Retrieval-Augmented Generation)
- **What it is**: A pattern where you retrieve relevant context from a knowledge base and inject it into the LLM prompt before generating an answer. The model answers from your data, not just its training data.
- **Why it matters over fine-tuning**:
  - Fine-tuning bakes knowledge into model weights — expensive, static, can't update without retraining
  - RAG retrieves at query time — cheap, always fresh, source-citable
- The pipeline in this codebase:
```java
User question
  → embed question (text-embedding-3-small → 1536-dim vector)
  → similarity search in pgvector (top-5 chunks, cosine similarity ≥ 0.65)
  → append retrieved code chunks to prompt as context
  → LLM generates answer grounded in actual code
  → return answer + source file citations

```
- **Interview Q: "What's the difference between RAG and fine-tuning?"**
  - RAG is retrieval at inference time — no model weight changes, works with any LLM, knowledge is updatable. Fine-tuning changes model weights — expensive, requires retraining when data changes, better for style/behavior changes than factual knowledge injection.
- **Interview Q: "What is the similarity threshold and why does it matter?"**
  - In this codebase: RAG uses 0.65, semantic search uses 0.55. Too high → misses relevant results. Too low → injects irrelevant noise into the prompt, confusing the LLM. It's a precision vs. recall tradeoff.

##  Vector Embeddings
- **What they are**: Dense numerical representations of text where semantic similarity = geometric proximity. "authenticate user" and "verify credentials" will have vectors close together even with no shared words.
- **How they're generated here**: OpenAI's text-embedding-3-small model converts each code chunk into a 1536-dimensional float vector. Locally, nomic-embed-text produces 768-dim vectors.
- **Why 1536 dimensions?** Higher dimensions capture more semantic nuance but cost more storage and compute. 1536 is OpenAI's sweet spot for quality vs. cost.
- **Interview Q: "Why can't you just use keyword search (grep) for code Q&A?"**
  - Keyword search requires exact token matches. Semantic search matches meaning — "exception handling patterns" finds catch (AuthenticationException e) even with zero shared words. This is the core value of embeddings.
 
## pgvector (PostgreSQL Vector Extension)
- **What it is**: A PostgreSQL extension that adds a vector data type and similarity search operators. Turns your existing Postgres into a vector database.
- **HNSW Index (Hierarchical Navigable Small World):**
  - A graph-based approximate nearest neighbor (ANN) algorithm
  - Builds a multi-layer graph where each node connects to its nearest neighbors
  - Search navigates from coarse top layers to fine bottom layers
  - Trade-off: slightly approximate results, but O(log n) search vs. O(n) brute force
  - Configured in application.yml: index-type: HNSW
- **Cosine Distance vs. Euclidean Distance:**
  - Cosine measures the angle between vectors — ignores magnitude, only cares about direction
  - Best for text embeddings because a long document and a short document about the same topic should be similar
  - Euclidean measures absolute distance — sensitive to vector magnitude
- **Interview Q: "Why HNSW over IVFFlat?"**
  - HNSW has better recall at high query speeds and doesn't require a training phase (IVFFlat needs to cluster data first). HNSW is preferred when data is continuously inserted (like ingesting new code files).
- **Metadata filtering**: pgvector stores metadata as JSONB alongside vectors. This codebase filters by project == 'my-app' so searches only return chunks from the relevant codebase, not everything in the DB.

## Spring AI
- **What it is**: Spring's abstraction layer over LLM providers (OpenAI, Ollama, Anthropic, etc.) — same code works with any model.
- **Key abstractions:**
  - ChatModel — interface for sending messages and getting responses. OpenAiChatModel and OllamaChatModel both implement it.
  - EmbeddingModel — converts text to vectors. Auto-configured from application.yml.
  - ChatClient — fluent builder API for constructing prompts with system messages, advisors, tools.
  - VectorStore — interface for storing/searching embeddings. PgVectorStore implements it.
  - Document — a chunk of text + metadata map. The unit of storage in the vector store.
- **Advisors pattern**: Advisors are interceptors that wrap every ChatClient call. They can modify the prompt before it goes to the LLM and process the response after.
  - QuestionAnswerAdvisor — the RAG advisor. Before each call: embeds the question, searches pgvector, appends top-K chunks to the prompt. After the call: stores retrieved documents in response metadata.
  - MessageChatMemoryAdvisor — the memory advisor. Before each call: loads conversation history for the sessionId and prepends it. After the call: saves the new exchange to memory.
- **Interview Q: "How does QuestionAnswerAdvisor work internally?"**
  - It implements the CallAroundAdvisor interface. In aroundCall(), it: (1) extracts the user query, (2) calls vectorStore.similaritySearch(), (3) formats retrieved documents as context, (4) appends them to the system/user prompt, (5) calls the LLM, (6) stores retrieved docs in ChatResponse metadata under RETRIEVED_DOCUMENTS.

## TokenTextSplitter & Chunking Strategy
- **Why chunking matters**: LLMs have context window limits. You can't embed an entire codebase at once. You split code into chunks, embed each chunk, and retrieve only the relevant ones.
- **Token-based vs. character-based splitting:**
  - Character splitting: naive, can cut mid-word or mid-token
  - Token splitting: respects the model's actual tokenization boundaries — more accurate for size estimation
- **Parameters in this codebase:**
```text
chunk size:  800 tokens  ≈ ~20-30 lines of Java code (1 average method)
overlap:     100 tokens  — last 100 tokens of chunk N appear at start of chunk N+1
                           prevents cutting a method signature from its body
min size:    5 tokens    — discard tiny fragments
```
- **Interview Q: "Why have overlap between chunks?"**
  - Without overlap, a method signature might be at the end of chunk N and its body at the start of chunk N+1. When you retrieve only chunk N, you get an incomplete picture. Overlap ensures context continuity at boundaries.
- **Metadata inheritance**: Each chunk inherits the parent document's metadata (project, filePath, className, packageName, fileType). This is how the system knows which file a retrieved chunk came from.

## Resilience4j
- **What it is**: A fault-tolerance library for Java. Protects external API calls (here: OpenAI) from cascading failures.
- **Rate Limiter**:
  - Limits calls to 60 per minute (limit-for-period: 60, limit-refresh-period: 1m)
  - If the limit is exceeded, throws RequestNotPermitted → triggers fallbackMethod
  - timeout-duration: 5s — how long to wait for a permit before failing fast
- **Circuit Breaker (the key pattern):**
  - Three states:
    1. CLOSED — normal operation, all calls go through
    2. OPEN — circuit is tripped, all calls fail immediately (no network calls) → fallback is called
    3. HALF-OPEN — after wait-duration-in-open-state: 30s, allows 3 test calls through. If they succeed → back to CLOSED. If they fail → back to OPEN
- **Sliding window: sliding-window-size**: 10 — tracks the last 10 calls. If ≥50% fail (failure-rate-threshold: 50) → circuit opens.
- **Interview Q: "Why use a circuit breaker for LLM calls specifically?"**
  - LLM APIs can be slow (2-10s per call), rate-limited, or temporarily down. Without a circuit breaker, a slow/down OpenAI API would cause all threads to block waiting for responses, exhausting the thread pool and taking down your entire service. The circuit breaker fails fast and returns a fallback immediately.
- **Interview Q: "What's the difference between rate limiter and circuit breaker?"**
  - Rate limiter is proactive — prevents you from sending too many requests. Circuit breaker is reactive — detects failures and stops sending requests when the downstream is unhealthy.

## Redis Caching with @Cacheable
- **What it is**: Spring's declarative caching. @Cacheable intercepts the method call, checks Redis for a cached result using the key, returns it if found (cache hit), or calls the method and stores the result (cache miss).
- **Cache key in this codebase:**
```java
key = "#projectName + ':' + #question.toLowerCase().trim().hashCode()"
```
  - Normalizes the question (lowercase, trimmed) before hashing → "What does UserService do?" and "what does userservice do?" hit the same cache entry
  - TTL: 1 hour (time-to-live: 3600000 ms)
-  unless = "#result == null" — don't cache null results (e.g., if the LLM returned nothing)
- **Interview Q: "Why cache LLM responses specifically?"**
  - LLM calls are: (1) expensive ($0.15/1M tokens), (2) slow (1-5 seconds), (3) deterministic for the same question at low temperature. Caching identical questions saves money and latency. The 1-hour TTL balances freshness vs. cost.
- **Interview Q: "What's the eviction policy in Redis here?"**
  - allkeys-lru (Least Recently Used) — when Redis hits its 256MB memory limit, it evicts the least recently accessed keys. This is set in docker-compose.yml: redis-server --maxmemory 256mb --maxmemory-policy allkeys-lru.
 
## @Async + CompletableFuture
- **What it is**: @EnableAsync on the main class + @Async on a method makes that method run in a separate thread pool, returning immediately to the caller.
- **Why ingestion is async**: Embedding a large ZIP (hundreds of Java files) can take 30-120 seconds (each file requires an OpenAI API call). Making the user wait would be terrible UX. Instead:
  1. Controller returns 202 Accepted immediately
  2. Ingestion runs in background thread
  3. Client polls or gets notified when done
- **CompletableFuture**: The async method returns CompletableFuture<Integer>. The controller chains .thenAccept() to log completion without blocking.
- **Interview Q: "What thread pool does @Async use?"**
  - By default, Spring uses SimpleAsyncTaskExecutor (creates a new thread per task — not ideal for production). For production, you'd define a ThreadPoolTaskExecutor bean with bounded queue size and rejection policy.
 
## Server-Sent Events (SSE) + Reactive Streams (Flux)
- **SSE**: A one-way HTTP streaming protocol. Server pushes events to the client over a single long-lived HTTP connection. Used here for streaming LLM token-by-token responses.
- **Why streaming for LLMs?** LLMs generate tokens sequentially. Without streaming, the user stares at a blank screen for 5-10 seconds. With streaming, tokens appear as they're generated — much better UX.
- **Flux (Project Reactor)**: A reactive stream that emits 0..N items asynchronously. Flux<String> here emits one string per token chunk from the LLM.
- **In the controller**:
```java
@GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<String> stream(...)
```
- Spring WebFlux automatically serializes each Flux emission as an SSE event.
- **Interview Q: "Why can't you cache a streaming response?"**
  - A Flux is a lazy, one-time-use stream. You can't serialize it to Redis and replay it. The codebase explicitly notes this: streaming uses @RateLimiter but not @Cacheable. For caching, use the synchronous ask() endpoint

## Multi-Turn Chat Memory
- **The problem**: LLMs are stateless — each API call is independent. To have a conversation, you must send the entire history with every request.
- **MessageChatMemoryAdvisor**: Before each call, loads all previous messages for the sessionId from InMemoryChatMemoryRepository (a ConcurrentHashMap<String, List<Message>>), prepends them to the prompt
- **MessageWindowChatMemory**: Keeps only the last N messages (sliding window) to prevent the context window from overflowing as conversations grow long.
- **Interview Q: "What's the production limitation of InMemoryChatMemory?"**
  - It's stored in the JVM heap — lost on restart, not shared across multiple instances (can't scale horizontally). Production alternatives: Redis-backed memory (serialize messages to Redis with TTL), or DB-backed (store in PostgreSQL). The codebase comments explicitly call this out.
- **Session management**: The sessionId is a UUID generated by the frontend (or auto-generated server-side if not provided). It's the key in the ConcurrentHashMap.

## Structured Output with BeanOutputConverter
- **The problem**: LLMs return free-form text. You want a typed Java object (CodeReview record).
- **How BeanOutputConverter works:**
  1. Inspects CodeReview.class via reflection → generates a JSON Schema
  2. Appends the schema as instructions to the prompt: "Return your response as JSON matching this schema: {...}"
  3. LLM returns JSON string
  4. converter.convert(response) deserializes JSON → CodeReview object
- **Interview Q: "What happens if the LLM returns malformed JSON?"**
  - converter.convert() throws a JsonParseException. Production-grade handling: retry with a "fix this JSON" prompt, or use OpenAI's structured outputs feature (enforces JSON schema at the API level).
 
## Function Calling / Tool Use (@Tool)
- **What it is:** The LLM can invoke Java methods during its reasoning process. The model decides when to call a tool and what arguments to pass.
- **In this codebase**: classifySeverity(String issueCategories) is annotated with @Tool. When the LLM is reviewing code, it can call this method to determine severity before composing the final JSON response. This forces the model to reason in steps rather than guess.
- **How it works under the hood**:
  1. Spring AI serializes the @Tool method signature into an OpenAI function definition (name, description, parameter schema)
  2. Sends it to OpenAI alongside the prompt
  3. OpenAI returns a tool_call response instead of text
  4. Spring AI invokes the Java method with the provided arguments
  5. Returns the result to OpenAI as a tool message
  6. OpenAI continues generating with the tool result in context
- **Interview Q: "What's the difference between @Tool and just putting logic in the prompt?"**
  - Tools give the LLM access to deterministic, real-time computation. The LLM can't reliably do arithmetic or apply business rules in its head. A tool call is guaranteed correct. It also enables agentic workflows where the LLM orchestrates multiple steps.
 
## Spring Profiles (@Profile)
- **What it is**: Conditional bean registration based on the active profile. Allows different implementations for different environments.
- **In this codebase**:
  - @Profile("!local") → OpenAiModelConfig — registers OpenAiChatModel as the primary ChatModel in production
  - @Profile("local") → OllamaModelConfig — registers OllamaChatModel as primary in local dev
- @Primary: When multiple beans of the same type exist, @Primary marks the one Spring should inject by default. Without it, Spring throws NoUniqueBeanDefinitionException.
- **Interview Q: "How does Spring know which ChatModel to inject into AiConfig?"**
  - Both OpenAiChatModel and OllamaChatModel are auto-configured by their starters. The profile-specific config classes create a ChatModel bean marked @Primary, which wins the injection contest. @Qualifier("openAiChatModel") is used to reference the specific auto-configured bean.

## Testcontainers
- **What it is:** A Java library that spins up real Docker containers during tests. No mocks — tests run against real PostgreSQL with pgvector, real Ollama.
- **Key annotations:**
  - @Testcontainers — enables Testcontainers JUnit 5 extension
  - @Container — manages container lifecycle (start before tests, stop after)
  - static container field → shared across all tests in the class (started once, not per test)
- **@DynamicPropertySource**: Injects container-assigned ports/URLs into Spring's Environment before the application context starts. This is how the test app connects to the test container instead of localhost:5432.
- **DockerImageName.parse(...).asCompatibleSubstituteFor("postgres")**: Tells Testcontainers to treat pgvector/pgvector:pg16 as a PostgreSQL-compatible image, so the PostgreSQLContainer abstraction works with it.
- **Interview Q: "Why use Testcontainers instead of H2 in-memory DB?"**
  - H2 doesn't support the vector data type or pgvector-specific SQL. Integration tests with H2 would miss vector store behavior entirely. Testcontainers gives you the exact same database as production — no behavioral differences.
 
## Micrometer + Prometheus Metrics
- **What it is**: Micrometer is a metrics facade (like SLF4J for metrics). micrometer-registry-prometheus exports metrics in Prometheus format at /actuator/prometheus.
- **Metrics tracked in this codebase:**
```java
meterRegistry.counter("ai.tokens.input", tags).increment(usage.getPromptTokens());
meterRegistry.counter("ai.tokens.output", tags).increment(usage.getCompletionTokens());
meterRegistry.timer("ai.latency", tags).record(Duration.ofMillis(elapsedMs));
```
- **Tags**: operation=rag-chat, model=gpt-4o-mini — allows filtering in Grafana dashboards.
- **Interview Q: "Why track token usage as metrics?"**
  - OpenAI charges per token. Tracking input/output tokens per operation lets you: (1) monitor costs in real time, (2) alert on cost spikes, (3) optimize prompts to reduce token usage, (4) capacity plan.
 
##  HikariCP Connection Pool
- **What it is**: The default JDBC connection pool in Spring Boot. Maintains a pool of pre-opened database connections.
- **Config**: maximum-pool-size: 10 — at most 10 simultaneous DB connections. pgvector queries (similarity search) are fast but each requires a connection.
- **Interview Q: "What happens if all 10 connections are in use?"**
  - New requests queue up waiting for a connection. If the queue fills or the wait exceeds connectionTimeout (default 30s), HikariCP throws SQLTransientConnectionException. This is why the circuit breaker matters — a slow LLM API shouldn't hold DB connections open.
 
## Java Records (model classes)
- **What they are**: Immutable data carriers introduced in Java 16. Compiler auto-generates constructor, getters, equals(), hashCode(), toString().
- **Used for**: ChatMessage, CodeReview, SearchResult, ConversationRequest — all pure data transfer objects with no behavior.
- **Interview Q: "When would you use a record vs. a class?"**
  - Records for immutable DTOs/value objects with no business logic. Classes when you need: mutability, inheritance, custom constructors with validation logic, or JPA entities (JPA requires no-arg constructor + setters).
 
## Lombok
- **@RequiredArgsConstructor**: Generates a constructor for all final fields. Combined with Spring's constructor injection, this replaces @Autowired on fields (which is discouraged — makes testing harder).
- **@Slf4j**: Generates private static final Logger log = LoggerFactory.getLogger(ClassName.class).
- **Interview Q: "Why prefer constructor injection over @Autowired field injection?"**
  - Constructor injection: (1) makes dependencies explicit and required, (2) allows the class to be instantiated in unit tests without a Spring context, (3) enables final fields (immutability), (4) fails fast at startup if a dependency is missing.
 
## Temperature in LLMs
- **What it is**: Controls randomness in token selection. Range: 0.0 to 2.0.
- **In this codebase**:
  - temperature: 0.2 for RAG chat — low temperature = consistent, factual, deterministic answers. Good for code Q&A where you want the same answer every time.
  - Code review client has no explicit temperature (defaults to model default ~1.0) — slightly more creative for generating suggestions.
- **Interview Q: "Why low temperature for code Q&A?"**
  - Code questions have correct answers. High temperature introduces hallucinations and inconsistency. At 0.2, the model almost always picks the highest-probability token — reliable and reproducible.
 
## The Full Data Flow (End-to-End)
- Ingestion
```java
POST /api/ingest/upload (ZIP file)
  → 202 Accepted (async)
  → background: extract .java files
  → TokenTextSplitter: 800-token chunks, 100-token overlap
  → each chunk: Document(content, {project, filePath, className, ...})
  → vectorStore.add() → OpenAI text-embedding-3-small → 1536-dim vector
  → INSERT INTO vector_store (content, metadata, embedding) in pgvector
```
- Query:
```java
GET /api/chat/ask?project=my-app&q=How+does+auth+work
  → @Cacheable: check Redis → cache miss
  → @RateLimiter: check rate limit → OK
  → @CircuitBreaker: circuit CLOSED → proceed
  → QuestionAnswerAdvisor:
      embed question → 1536-dim vector
      SELECT ... FROM vector_store ORDER BY embedding <=> query_vector LIMIT 5
      WHERE metadata->>'project' = 'my-app' AND score >= 0.65
  → build prompt: system + retrieved code chunks + user question
  → POST to OpenAI gpt-4o-mini
  → parse response → ChatMessage(answer, sources, tokens, latency)
  → store in Redis (TTL 1hr)
  → return JSON
```
