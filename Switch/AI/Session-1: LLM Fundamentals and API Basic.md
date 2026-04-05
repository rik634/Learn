# Session 1: LLM Fundamentals & API Basics - Theory Guide

## Part 1: How Large Language Models Work (30 minutes)

### What is a Large Language Model (LLM)?

An LLM is a neural network trained on massive amounts of text data that learns to **predict the next word** in a sequence. That's it. Everything else—answering questions, writing code, translating languages—emerges from this simple task.

**Key Insight:** LLMs don't "understand" text like humans do. They recognize patterns in billions of text examples and generate statistically likely continuations.

### The Training Process (Simplified)

1. **Pre-training (Foundation)**
   - Model reads billions of web pages, books, code repositories
   - Learns: "After 'The capital of France is', the next word is likely 'Paris'"
   - Learns grammar, facts, reasoning patterns, coding syntax
   - This creates the "base model"

2. **Fine-tuning (Instruction Following)**
   - Model is trained on instruction-response pairs
   - Example: "Question: What is 2+2? Answer: 4"
   - Learns to follow instructions rather than just complete text
   - This creates models like GPT-4, Claude, etc.

3. **RLHF (Reinforcement Learning from Human Feedback)**
   - Humans rate model outputs (helpful vs. harmful)
   - Model learns to produce preferred responses
   - Reduces harmful outputs, improves helpfulness

### How LLMs Generate Text

**Step-by-step generation:**

```
User: "Write a haiku about code"
Model thinks:
1. Next token: "Code" (high probability)
2. Next token: "whispers" (selected from probability distribution)
3. Next token: "in" (likely continuation)
4. ... continues until complete

Output: "Code whispers in the night,
         Bugs hide in shadowed lines,
         Dawn brings clarity."
```

**Important:** Each word (token) is generated one at a time, based on all previous tokens. The model can't "plan ahead" in the traditional sense.

---

## Part 2: Core Concepts You Must Understand

### 1. Tokens - The Currency of LLMs

**What are tokens?**
- Tokens are pieces of words that the model processes
- NOT the same as words: "incredible" = 1 token, "ChatGPT" = 2 tokens
- Includes punctuation, spaces, numbers

**Examples:**
```
"Hello world" = 2 tokens ["Hello", " world"]
"AI is amazing!" = 4 tokens ["AI", " is", " amazing", "!"]
"anthropomorphization" = 5 tokens ["anth", "rop", "om", "orph", "ization"]
```

**Why tokens matter:**
- **Pricing:** APIs charge per token (input + output)
- **Limits:** Models have maximum token limits (context windows)
- **Performance:** More tokens = more processing time

**Rule of thumb:** 
- English: ~4 characters = 1 token (roughly 0.75 tokens per word)
- Code: Varies widely (dense code uses more tokens)

**Practical example:**
```
Input: "Summarize this article" (4 tokens)
Article: 3000 tokens
Output: 150 tokens
Total cost: 3154 tokens × price per token
```

### 2. Context Window - The Model's Memory

**What is it?**
The context window is the maximum amount of text (in tokens) the model can "remember" at once. Think of it as short-term memory.

**Model Comparison (as of 2024-2026):**
- GPT-3.5: 16K tokens (~12,000 words)
- GPT-4: 128K tokens (~96,000 words)
- GPT-4 Turbo: 128K tokens
- Claude 3.5 Sonnet: 200K tokens (~150,000 words)
- Gemini 1.5 Pro: 1M tokens (~750,000 words)

**What counts toward the context window?**
```
System prompt: 200 tokens
+ User message history: 1000 tokens
+ Current user message: 100 tokens
+ Model's response: 500 tokens
= Total: 1800 tokens used
```

**What happens when you exceed the limit?**
- Oldest messages are dropped (in chat applications)
- API returns an error (in single API calls)
- Model "forgets" earlier parts of the conversation

**Practical implications:**
- Long documents need chunking or summarization
- Chat history management is critical
- Context window ≠ how much the model can "think about" effectively

### 3. Temperature - Controlling Randomness

**What is temperature?**
Temperature controls how random/creative the model's output is.

**How it works:**
When predicting the next token, the model generates probabilities:
```
Next word probabilities:
"happy" - 40%
"joyful" - 30%
"ecstatic" - 20%
"elated" - 10%
```

**Temperature = 0 (Deterministic)**
- Always picks the highest probability token
- Output: "happy" every time
- Use for: Math, code generation, factual answers
- Predictable, consistent, boring

**Temperature = 0.7 (Balanced - Default)**
- Samples from probability distribution with moderate randomness
- Output: Might pick "happy" 60% of the time, "joyful" 25%, etc.
- Use for: General conversation, creative tasks with some consistency
- Good balance of creativity and coherence

**Temperature = 1.5 (Creative/Chaotic)**
- Flattens probability distribution, gives low-probability tokens a chance
- Output: Might pick "elated" or even lower-probability words
- Use for: Creative writing, brainstorming, artistic content
- More surprising, less coherent

**Visual representation:**
```
Temperature = 0:    ████████ happy (100%)
                    -------- joyful (0%)
                    -------- ecstatic (0%)

Temperature = 0.7:  ████---- happy (60%)
                    ███----- joyful (35%)
                    █------- ecstatic (5%)

Temperature = 1.5:  ███----- happy (30%)
                    ███----- joyful (30%)
                    ██------ ecstatic (25%)
                    ██------ elated (15%)
```

**API usage:**
```json
{
  "temperature": 0.0,  // For code, math, facts
  "temperature": 0.7,  // For chat, general use
  "temperature": 1.2   // For creative writing
}
```

### 4. Top-p (Nucleus Sampling) - Alternative to Temperature

**What is top-p?**
Instead of controlling randomness globally, top-p limits the pool of tokens the model can choose from.

**How it works:**
- Model generates probability distribution for all possible next tokens
- Top-p = 0.9 means: "Only consider tokens that make up the top 90% of probability mass"
- Cuts off unlikely tokens dynamically

**Example:**
```
All possible next tokens:
"happy" - 40%
"joyful" - 30%
"ecstatic" - 15%
"elated" - 8%
"thrilled" - 4%
"delighted" - 2%
"cheerful" - 1%

With top-p = 0.9:
- Cumulative: happy (40%) + joyful (30%) + ecstatic (15%) = 85%
- Need to reach 90%, so add "elated" (8%) = 93%
- Model can only pick from: happy, joyful, ecstatic, elated
- Other tokens are excluded
```

**Top-p vs Temperature:**
- **Temperature:** Changes how random selection is across all tokens
- **Top-p:** Limits which tokens are available for selection
- **Best practice:** Use one or the other, not both
  - `temperature=0.7, top_p=1.0` (standard)
  - OR `temperature=1.0, top_p=0.9` (alternative)

**Common settings:**
```json
// Conservative (factual tasks)
{ "temperature": 0.3, "top_p": 1.0 }

// Balanced (default)
{ "temperature": 0.7, "top_p": 1.0 }

// Creative
{ "temperature": 1.0, "top_p": 0.95 }

// Alternative creative
{ "temperature": 1.0, "top_p": 0.85 }
```

---

## Part 3: API Integration Fundamentals

### API Call Anatomy

**Basic structure of every LLM API call:**
1. **Authentication:** API key in header
2. **Model selection:** Which model to use
3. **Messages:** Your conversation/prompt
4. **Parameters:** Temperature, max_tokens, etc.
5. **Response handling:** Parse JSON, extract text

### OpenAI API Structure

**Endpoint:**
```
POST https://api.openai.com/v1/chat/completions
```

**Request format:**
```json
{
  "model": "gpt-4o",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "What is the capital of France?"}
  ],
  "temperature": 0.7,
  "max_tokens": 150
}
```

**Response format:**
```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "created": 1677652288,
  "model": "gpt-4o",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "The capital of France is Paris."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 20,
    "completion_tokens": 8,
    "total_tokens": 28
  }
}
```

**Key fields:**
- `choices[0].message.content` - The actual response text
- `usage` - Token consumption (for billing tracking)
- `finish_reason` - Why generation stopped ("stop" = natural end, "length" = hit max_tokens)

### Anthropic Claude API Structure

**Endpoint:**
```
POST https://api.anthropic.com/v1/messages
```

**Request format:**
```json
{
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 1024,
  "messages": [
    {"role": "user", "content": "What is the capital of France?"}
  ],
  "temperature": 0.7
}
```

**Key differences from OpenAI:**
- No separate "system" role in messages array (use system parameter instead)
- `max_tokens` is REQUIRED (OpenAI has defaults)
- Different model naming convention

**Response format:**
```json
{
  "id": "msg_abc123",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "The capital of France is Paris."
    }
  ],
  "model": "claude-3-5-sonnet-20241022",
  "stop_reason": "end_turn",
  "usage": {
    "input_tokens": 15,
    "output_tokens": 8
  }
}
```

**Key fields:**
- `content[0].text` - The response text (note: it's an array)
- `usage` - Input/output token counts
- `stop_reason` - Why it stopped ("end_turn", "max_tokens", "stop_sequence")

---

## Part 4: OpenAI vs Anthropic - Detailed Comparison

### Model Lineup (April 2026)

**OpenAI:**
- **GPT-4o** (flagship): 128K context, multimodal, fast
- **GPT-4o-mini**: Cheaper, 128K context, good for simple tasks
- **GPT-3.5-turbo**: Legacy, 16K context, cheapest

**Anthropic:**
- **Claude 3.5 Sonnet**: 200K context, excellent at coding/analysis
- **Claude 3 Opus**: Most capable, 200K context, expensive
- **Claude 3 Haiku**: Fastest, cheapest, 200K context

### Pricing Comparison (approximate, USD)

**OpenAI GPT-4o:**
- Input: $2.50 per 1M tokens
- Output: $10.00 per 1M tokens

**OpenAI GPT-4o-mini:**
- Input: $0.15 per 1M tokens
- Output: $0.60 per 1M tokens

**Claude 3.5 Sonnet:**
- Input: $3.00 per 1M tokens
- Output: $15.00 per 1M tokens

**Claude 3 Haiku:**
- Input: $0.25 per 1M tokens
- Output: $1.25 per 1M tokens

**Cost example (1000-token question, 500-token answer):**
- GPT-4o: (1000 × $2.50 + 500 × $10.00) / 1M = $0.0075
- Claude Sonnet: (1000 × $3.00 + 500 × $15.00) / 1M = $0.0105
- GPT-4o-mini: (1000 × $0.15 + 500 × $0.60) / 1M = $0.00045

### API Differences Summary

| Feature | OpenAI | Anthropic |
|---------|--------|-----------|
| **Authentication** | `Authorization: Bearer sk-...` | `x-api-key: sk-ant-...` |
| **System message** | In messages array | Separate `system` parameter |
| **max_tokens** | Optional (default 256-4096) | REQUIRED |
| **Response path** | `choices[0].message.content` | `content[0].text` |
| **Streaming** | `stream: true` | `stream: true` |
| **Model names** | `gpt-4o`, `gpt-4o-mini` | `claude-3-5-sonnet-...` (full version string) |

### When to Choose Which?

**Use OpenAI if:**
- You need multimodal (vision + text)
- You're building consumer-facing products (brand recognition)
- You need the cheapest option (GPT-4o-mini)
- You want the largest ecosystem (libraries, examples)

**Use Anthropic Claude if:**
- You need longer context (200K tokens standard)
- You're doing complex reasoning or coding tasks
- You prioritize safety/alignment
- You want higher context window at standard pricing

**For product companies:**
- Most use both: OpenAI for general tasks, Claude for coding/analysis
- A/B test for your specific use case
- Monitor costs and quality metrics

---

## Part 5: Spring Boot Integration - Theory

### Why RestTemplate vs WebClient?

**RestTemplate (Traditional):**
- Synchronous, blocking API
- Simple, familiar to most Java developers
- Good for: Simple API calls, low-concurrency scenarios
- **Deprecated** but still widely used

**WebClient (Modern):**
- Asynchronous, non-blocking (reactive)
- Part of Spring WebFlux
- Better for: High-concurrency, streaming responses
- **Recommended** for new projects

### API Call Flow in Spring Boot

```
1. Controller receives user request
   └─> @PostMapping("/ask")
   
2. Extract user message from request body
   └─> String userMessage = request.getMessage();
   
3. Build API request JSON
   └─> Create request body with model, messages, params
   
4. Send HTTP POST to LLM API
   └─> RestTemplate.postForObject() or WebClient.post()
   
5. Parse JSON response
   └─> Extract choices[0].message.content or content[0].text
   
6. Return to user
   └─> Return ResponseEntity with AI response
```

### Error Handling Considerations

**Common API errors:**
- **401 Unauthorized:** Invalid API key
- **429 Too Many Requests:** Rate limit exceeded
- **400 Bad Request:** Invalid parameters (missing max_tokens, invalid model)
- **500 Server Error:** API is down
- **Context length exceeded:** Input + output > model's context window

**Best practices:**
```java
try {
    // API call
} catch (HttpClientErrorException e) {
    if (e.getStatusCode().value() == 401) {
        // Handle invalid API key
    } else if (e.getStatusCode().value() == 429) {
        // Handle rate limit (retry with backoff)
    }
} catch (HttpServerErrorException e) {
    // API is down, retry or fail gracefully
}
```

### Security Best Practices

**API Key Management:**
1. **Never** hardcode API keys in source code
2. Store in environment variables or application.properties
3. Use Spring's `@Value` annotation:
   ```java
   @Value("${openai.api.key}")
   private String apiKey;
   ```
4. Add `.env` to `.gitignore`

**Input Validation:**
- Sanitize user input (remove/escape special characters)
- Limit input length (prevent expensive API calls)
- Rate-limit your own API (prevent abuse)

**Response Handling:**
- Don't expose raw API errors to users (security risk)
- Log errors for debugging, return generic messages
- Validate API responses before sending to users

---

## Part 6: Practical Implementation Pattern

### Step-by-Step Implementation Guide

**Step 1: Add Dependencies (pom.xml)**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**Step 2: Configuration (application.properties)**
```properties
openai.api.key=${OPENAI_API_KEY}
openai.api.url=https://api.openai.com/v1/chat/completions
openai.model=gpt-4o-mini
```

**Step 3: Create Request/Response DTOs**
```java
// Request to your API
public class ChatRequest {
    private String message;
    // getters/setters
}

// OpenAI API request structure
public class OpenAIRequest {
    private String model;
    private List<Message> messages;
    private double temperature;
    // getters/setters
}

public class Message {
    private String role;
    private String content;
    // getters/setters
}

// OpenAI API response structure
public class OpenAIResponse {
    private List<Choice> choices;
    private Usage usage;
    // getters/setters
}

public class Choice {
    private Message message;
    // getters/setters
}
```

**Step 4: Service Layer**
```java
@Service
public class LLMService {
    
    @Value("${openai.api.key}")
    private String apiKey;
    
    private final RestTemplate restTemplate = new RestTemplate();
    
    public String chat(String userMessage) {
        // 1. Build request
        OpenAIRequest request = buildRequest(userMessage);
        
        // 2. Set headers
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Bearer " + apiKey);
        headers.setContentType(MediaType.APPLICATION_JSON);
        
        // 3. Make API call
        HttpEntity<OpenAIRequest> entity = new HttpEntity<>(request, headers);
        OpenAIResponse response = restTemplate.postForObject(
            "https://api.openai.com/v1/chat/completions",
            entity,
            OpenAIResponse.class
        );
        
        // 4. Extract response
        return response.getChoices().get(0).getMessage().getContent();
    }
    
    private OpenAIRequest buildRequest(String userMessage) {
        OpenAIRequest request = new OpenAIRequest();
        request.setModel("gpt-4o-mini");
        request.setTemperature(0.7);
        
        Message userMsg = new Message();
        userMsg.setRole("user");
        userMsg.setContent(userMessage);
        
        request.setMessages(Arrays.asList(userMsg));
        return request;
    }
}
```

**Step 5: Controller**
```java
@RestController
@RequestMapping("/api/chat")
public class ChatController {
    
    @Autowired
    private LLMService llmService;
    
    @PostMapping
    public ResponseEntity<String> chat(@RequestBody ChatRequest request) {
        String response = llmService.chat(request.getMessage());
        return ResponseEntity.ok(response);
    }
}
```

---

## Part 7: Testing Your Implementation

### Manual Testing with cURL

```bash
# Test your Spring Boot API
curl -X POST http://localhost:8080/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What is the capital of France?"}'
```

### Testing OpenAI API Directly

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

### Common Issues & Solutions

**Issue 1: "API key not valid"**
- Solution: Check environment variable is set correctly
- Verify key format: `sk-...` (OpenAI) or `sk-ant-...` (Anthropic)

**Issue 2: "max_tokens is required" (Anthropic only)**
- Solution: Add `"max_tokens": 1024` to request body

**Issue 3: NullPointerException when parsing response**
- Solution: Check response structure, add null checks
- Verify DTOs match actual API response format

**Issue 4: Connection timeout**
- Solution: LLM APIs can be slow (5-30 seconds)
- Increase timeout: `restTemplate.setRequestFactory(factory with longer timeout)`

---

## Part 8: Cost and Performance Optimization

### Token Management

**Calculate before calling:**
```java
// Rough estimate: 4 characters ≈ 1 token
int estimatedTokens = userMessage.length() / 4;
if (estimatedTokens > 1000) {
    // Warn user or truncate
}
```

**Track usage:**
```java
// Log token usage from API response
int totalTokens = response.getUsage().getTotalTokens();
double cost = calculateCost(totalTokens, "gpt-4o-mini");
log.info("API call cost: ${}", cost);
```

### Caching Strategies

**Cache identical requests:**
```java
@Cacheable(value = "llmResponses", key = "#userMessage")
public String chat(String userMessage) {
    // Expensive API call only happens once per unique message
}
```

**When to cache:**
- FAQ-style questions (same questions repeatedly)
- Static content generation
- When response consistency matters

**When NOT to cache:**
- Creative writing (want different outputs)
- Time-sensitive queries ("What's the weather?")
- Personalized responses

### Rate Limiting

**Protect your API:**
```java
@RateLimiter(name = "llmApi", fallbackMethod = "rateLimitFallback")
public String chat(String userMessage) {
    // API call
}

public String rateLimitFallback(String userMessage, Throwable t) {
    return "Too many requests. Please try again later.";
}
```

---

## Part 9: 15-Minute Review Checklist

After completing Session 1, you should be able to answer:

### Conceptual Understanding
- [ ] How does an LLM generate text? (Next-token prediction)
- [ ] What's the difference between pre-training and fine-tuning?
- [ ] What are tokens and why do they matter? (Pricing, limits)
- [ ] What is a context window? (Model's memory limit)
- [ ] When would you use temperature=0 vs temperature=1? (Deterministic vs creative)
- [ ] What's the difference between temperature and top-p? (Randomness vs token pool)

### API Knowledge
- [ ] What are the required fields for OpenAI API? (model, messages)
- [ ] What are the required fields for Anthropic API? (model, messages, max_tokens)
- [ ] How do you extract the response text from each API?
- [ ] What's the pricing difference between GPT-4o and Claude Sonnet?

### Implementation
- [ ] Can you make a successful API call from Spring Boot?
- [ ] How do you handle API errors (401, 429, 500)?
- [ ] Where should you store API keys? (Environment variables, NOT code)
- [ ] What's the difference between RestTemplate and WebClient?

### Practical Application
- [ ] What would you build: a chatbot, code reviewer, or summarizer?
- [ ] How would you estimate the cost of 1000 API calls?
- [ ] When would you choose OpenAI vs Anthropic?

---

## Part 10: Tomorrow's Preparation

**Set up your development environment:**

1. **Get API keys:**
   - OpenAI: https://platform.openai.com/api-keys
   - Anthropic: https://console.anthropic.com/

2. **Create Spring Boot project:**
   ```bash
   spring init --dependencies=web --build=maven --language=java myapp
   ```

3. **Environment variables:**
   ```bash
   export OPENAI_API_KEY="sk-..."
   export ANTHROPIC_API_KEY="sk-ant-..."
   ```

4. **Test API access:**
   ```bash
   curl https://api.openai.com/v1/models \
     -H "Authorization: Bearer $OPENAI_API_KEY"
   ```

**Explore on your own:**
- Try different temperature values (0, 0.5, 1.0, 1.5)
- Compare response quality between models
- Test with different prompt styles (questions, instructions, creative tasks)

---

## Key Takeaways

1. **LLMs are next-token predictors** - Everything else emerges from this
2. **Tokens are the unit of cost and limits** - Always be aware of token usage
3. **Temperature controls creativity** - 0 for facts, 1+ for creative tasks
4. **Context window is limited** - Plan for long conversations and documents
5. **APIs are similar but different** - Test both OpenAI and Anthropic
6. **Security matters** - Never hardcode keys, validate inputs
7. **Start simple** - Get one API call working, then expand

**You're now ready to build your first LLM-powered Spring Boot application!**

---

**Next Session Preview:**
Session 2 will teach you **prompt engineering** - how to write instructions that get reliable, high-quality outputs. You'll learn patterns like few-shot learning, chain-of-thought, and structured output formatting.
