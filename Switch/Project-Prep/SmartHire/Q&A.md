# SmartHire + CodeSense — Complete Interview Q&A Guide
### FAANG-level answers for product-based company interviews
### Covers: AI Agents · Kafka · pgvector · Spring AI · LangChain4j · System Design · Java · Spring Boot
---

> **How to use this document:**
> Read each answer out loud. Do not memorise word-for-word.
> Understand the reasoning, then say it in your own words.
> Answers marked ⭐ are asked in almost every AI engineer interview.
> Answers marked 🔥 are differentiators that most candidates cannot answer.

---

# SECTION 1 — AI AGENTS

---

**Q1. What is an AI agent and how is it different from a regular LLM call?** ⭐

A regular LLM call is a single request-response. You send a prompt, the model
returns text, done. You control everything — what gets called, in what order,
what happens with the output. The flow is deterministic and hardcoded by you.

An AI agent is fundamentally different. The LLM is not just answering — it is
deciding. You give the model a goal, a set of tools it can invoke, and the
authority to decide which tools to call, in what order, how many times, and
when the goal is achieved. The control flow is dynamic. The LLM drives it.

In SmartHire, I declare the goal in the system prompt: "evaluate this candidate."
I give the agent six tools. The LLM reads the resume, decides to call
parseResumeSkills first, reads the result, decides to call findMatchingRoles
next, and so on — until it decides the evaluation is complete. My Java code
never says "call tool 1 then tool 2." The LLM figures that out at runtime.

---

**Q2. What is the ReAct pattern? Walk me through it.** ⭐

ReAct stands for Reasoning and Acting. It is the foundational pattern for AI
agents. The model alternates between reasoning about its current state and
taking actions based on that reasoning.

The loop is: Thought → Action → Observation, repeated until done.

In SmartHire it looks like this:

Thought: I need to understand what skills this candidate has before I can match them to roles.
Action: parseResumeSkills(resumeText)
Observation: skills are Java, Spring Boot, Kafka. Three years experience.

Thought: I have the skills. Now I need to find which open roles match this profile.
Action: findMatchingRoles("Java, Spring Boot, Kafka")
Observation: Senior Java Engineer matched at 0.87 similarity.

Thought: Good match. I need to compute a formal score.
Action: scoreCandidate("Java, Spring Boot, Kafka", "1")
Observation: Score 82 out of 100. STRONG_FIT.

Thought: Score is above 75. I should check if the role is still open before scheduling.
Action: checkRoleAvailability("1")
Observation: Role is OPEN.

Thought: Role is open and candidate is strong. Schedule an interview.
Action: scheduleInterview(email, "1")
Observation: Monday 10 AM booked.

Thought: Everything is done. Notify the candidate.
Action: notifyCandidate(email, "STRONG_FIT", message)
Final Answer: Candidate Rahul is a strong fit...

What makes ReAct powerful is that each observation influences the next thought.
If the role was CLOSED, the agent would reason about finding the next best
role instead of scheduling. The flow adapts to reality.

---

**Q3. Why use LangChain4j instead of just calling the OpenAI API directly?** ⭐

Calling OpenAI directly gives you a single request-response. To implement
an agent loop manually, you would need to: parse tool call requests from
the LLM response, find the matching Java method, execute it, append the
result to the message history, send the updated history back to the LLM,
detect when the LLM stops requesting tool calls, and return the final answer.
That is hundreds of lines of boilerplate that you have to write, test, and maintain.

LangChain4j's AiServices does all of that from an interface declaration.
I declare RecruitingAgent as a Java interface with a @SystemMessage and a
@UserMessage method. AiServices.builder() generates the entire agent loop
at runtime. My production code is eight lines in AiConfig.java.

It is the same principle as Spring Data JPA. I declare
`interface JobRoleRepository extends JpaRepository` and Spring generates
the SQL queries. I declare `interface RecruitingAgent` and LangChain4j
generates the agent loop.

---

**Q4. How does @Tool work? How does the LLM know which method to call?** ⭐

When I call AiServices.builder().tools(agentTools), LangChain4j scans all
methods annotated with @Tool on the agentTools object. For each method,
it generates a JSON schema containing the method name, the @Tool description,
and the parameter descriptions from @P annotations. This JSON schema is sent
to the LLM alongside every prompt.

The LLM reads the descriptions and decides when a tool is needed. It does not
see the Java code — only the name and description. When it decides to call a
tool, it responds with a structured tool call request rather than text:
something like "call findMatchingRoles with argument candidateSkills = Java,
Spring Boot, Kafka."

LangChain4j intercepts this response, finds the matching Java method by name,
invokes it with the provided arguments, and returns the string result to the
LLM as a ToolResultMessage. The LLM continues reasoning with that result.

This is why the @Tool description is more important than the implementation.
If the description is vague, the LLM calls the wrong tool or passes wrong
arguments. The description is the API contract between the LLM and your Java code.

---

**Q5. What happens if the agent gets into an infinite loop?** 🔥

This is a real failure mode. An agent can loop if each tool result triggers
another tool call without making progress toward the goal.

LangChain4j has a maxIterations setting on AiServices. I would set this to
something like 15. If the agent has not reached a conclusion after 15 tool
calls, it stops and returns whatever it has so far or an error message.

Beyond that, I would handle it at three levels. First, the system prompt should
explicitly say "complete the evaluation in no more than 6 tool calls." Second,
each tool should return a definitive result that moves the agent forward, not
ambiguous output that causes re-evaluation. Third, the Resilience4j rate limiter
acts as a budget governor — once 50 LLM calls per minute are exhausted,
further calls fail fast rather than accumulating.

I would also monitor for this using LangSmith tracing. If I see evaluations
with more than 8 tool calls in the traces, I investigate the system prompt.

---

**Q6. How do you evaluate whether the agent is making correct decisions?** 🔥

I maintain a labeled evaluation dataset. I have 50 resumes with human-verified
correct decisions: which are STRONG_FIT, which are NOT_FIT. I run the agent
against this dataset weekly and measure decision accuracy.

Beyond accuracy, I measure three things. Tool call efficiency — a well-designed
agent should evaluate a clear STRONG_FIT in 5 to 6 tool calls. If it is taking
10, the system prompt is ambiguous. Score correlation — I compare the agent's
fit scores against human recruiter scores for the same candidates. And I monitor
the audit trail for tool call ordering anomalies — if the agent ever calls
scheduleInterview before scoreCandidate, that indicates a system prompt problem.

I also run this evaluation in CI on every system prompt change, treating it
like a test suite for the agent's behavior.

---

**Q7. How do you make the agent's decisions explainable to business users?** 🔥

This is what the AgentStep entity solves. Every time a @Tool method is called,
I write a row to the agent_steps table with the evaluation ID, step number,
tool name, input, and output.

A GET /audit/{evaluationId} endpoint returns the complete trace. An HR manager
can see: step 1 the agent parsed the resume and found Java, Spring Boot, Kafka.
Step 2 it searched and found Senior Java Engineer as the top match with 0.87
similarity. Step 3 it scored the candidate at 82 out of 100. Step 4 it
confirmed the role was open. Step 5 it scheduled Monday 10 AM. Step 6 it
sent the notification.

Every decision is visible, traceable, and verifiable. This matters for two
reasons. First, HR managers need to understand and trust the system before
they rely on it. Second, in any regulated industry — like financial services
or government — algorithmic hiring decisions must be auditable and explainable.
You cannot ship an AI hiring tool without this.

---

**Q8. RAG is used inside the agent — explain how.** ⭐

RAG and agents are not competing patterns. RAG is a retrieval technique.
Agents are a control flow pattern. You use RAG inside an agent when one
of the actions the agent needs to take is semantic retrieval.

In SmartHire, the findMatchingRoles tool is where RAG lives. When the agent
decides it needs to find matching roles, it calls this tool with the
candidate's skills as a string. Inside the tool, Spring AI embeds that
string using text-embedding-3-small, then queries pgvector using cosine
similarity to find the top 4 job descriptions that are semantically closest
to the candidate's profile. The matching roles are returned to the agent
as a JSON string. The agent then reasons about which role to score
the candidate against.

So the full flow is: agent decides to retrieve → tool executes RAG → result
comes back as context → agent reasons with that context → agent decides next
action. RAG is one step in the agent loop, not the whole system.

---

**Q9. Why does the agent tool return a String instead of a typed object?** 🔥

The LLM processes everything as text. When a tool returns a result,
LangChain4j serializes it and adds it to the conversation as a
ToolResultMessage — which is text that the LLM reads as part of its
next reasoning step.

If I returned a Java object, LangChain4j would serialize it to JSON
and include that JSON as text in the message. The LLM reads the JSON
string and reasons about it. There is no difference in what the LLM
sees whether I return a String directly or return an object that gets
serialized.

I prefer returning a JSON string explicitly because it gives me control
over exactly what the LLM sees. I can trim large results, format key
fields prominently, and structure the output in a way that helps the
LLM reason correctly about the result.

I also never throw exceptions from tools. I catch all exceptions inside
the tool and return an error message as a string — something like "Error:
role not found for ID 99." The LLM reads the error and adjusts its approach
rather than crashing the entire agent loop.

---

# SECTION 2 — KAFKA

---

**Q10. Why did you use Kafka for this? Why not just process the resume synchronously?** ⭐

An agent evaluation takes 10 to 30 seconds. Multiple LLM API calls, pgvector
searches, database writes. An HTTP request cannot wait 30 seconds — clients
time out, user experience degrades, and under load you exhaust your thread pool.

Kafka solves this with the async pattern. The upload endpoint publishes a
lightweight event to Kafka and returns 202 Accepted in under 100 milliseconds.
A separate consumer thread picks up the event and runs the agent in the background.
The client polls a result endpoint every few seconds.

But the deeper reason to choose Kafka over simpler alternatives is three
properties you cannot get any other way. Durability — if the server crashes
while an agent is running, the Kafka event is not lost. When the server
restarts, the consumer picks up from where it left off. @Async on a thread
pool loses all in-flight work on restart. Scalability — I can deploy 10
consumer pods each with 3 concurrent consumers and process 30 evaluations
simultaneously without touching the upload service. And observability —
Kafka gives me consumer lag metrics so I know immediately if evaluations
are falling behind.

---

**Q11. Explain Kafka topics, partitions, consumer groups and offsets.** ⭐

A topic is a named, append-only, ordered log of messages. Think of it like
a database table but you can only append to it, never update or delete.
SmartHire has three topics: resume-uploaded-events, evaluation-complete-events,
and evaluation-failed-events.

A partition is how Kafka achieves parallelism. Each topic is divided into
partitions. Messages with the same key always go to the same partition.
I use evaluationId as the Kafka key so all messages for one evaluation
go to the same partition, guaranteeing their order.

A consumer group is a set of consumers sharing the work of processing a topic.
Each partition is assigned to exactly one consumer in the group at a time.
If I have 3 partitions and 3 consumer threads, each thread gets one partition.
If one consumer dies, Kafka reassigns its partition to another consumer — that
is automatic failover.

An offset is the position of a message in a partition — a sequential integer
starting from zero. Each consumer tracks its current offset. When a message
is processed, the consumer commits the offset. On restart, it resumes from
the last committed offset — so no message is lost even if the consumer crashes
mid-processing.

---

**Q12. What is the difference between at-most-once, at-least-once, and exactly-once delivery?** 🔥

At-most-once means a message is delivered zero or one times. The consumer
commits the offset before processing. If it crashes after committing but
before processing, the message is lost. Use this only when losing messages
is acceptable — like metrics or logs.

At-least-once means a message is delivered one or more times. The consumer
commits the offset after processing. If it crashes after processing but
before committing, it processes the same message again on restart. This
is what SmartHire uses. I handle duplicates with an idempotency check —
before processing, I verify the evaluationId has not already been processed.

Exactly-once means each message is processed precisely once. Kafka achieves
this with transactions — atomic writes across multiple partitions and topics.
The producer and consumer coordinate via transaction IDs. This is the most
expensive guarantee and necessary for financial systems where a double
payment is a serious problem.

For SmartHire, at-least-once with idempotency is the right tradeoff. The
overhead of exactly-once transactions is not justified for a recruiting tool.

---

**Q13. What happens if the agent evaluation fails? How do you handle it?** ⭐

I handle failures at two levels.

First, transient failures — network timeout, OpenAI rate limit, temporary
database issue. For these, I rely on Kafka's retry mechanism. If the consumer
throws an exception, Kafka can be configured to retry with backoff. The message
stays in the topic and is retried a configurable number of times.

Second, permanent failures — the resume is malformed, the candidate data is
invalid, the OpenAI API returns a persistent error. For these I catch the
exception in the consumer and publish to the evaluation-failed-events dead
letter topic. The original event is not retried — I do not want an infinite
retry loop consuming resources. Operations can inspect the dead letter topic,
understand the failure, fix the root cause, and replay specific events.

I also expose metrics on the dead letter topic size via Actuator. If it grows
beyond a threshold, an alert fires. Zero dead letter messages is a good day.

---

**Q14. Why is the evaluationId used as the Kafka message key?** 🔥

Two reasons.

The first is ordering. Kafka guarantees order within a partition but not
across partitions. By using evaluationId as the key, all messages related
to one evaluation — the initial upload event, any retry events — go to the
same partition and are processed in order.

The second is idempotency. When I receive an event, I check whether this
evaluationId has already been processed by looking up the evaluation_results
table. If it exists, I skip processing. The evaluationId is the idempotency
key. This is important because Kafka's at-least-once delivery means the same
event can arrive twice — on consumer restart, network partition recovery,
or manual replay from the dead letter topic.

---

**Q15. Producer and consumer are in the same service — is that good architecture?** ⭐

It is acceptable and pragmatic at this stage. The fundamental decoupling
benefit of Kafka exists regardless — the HTTP thread publishes in milliseconds
and returns, while a separate consumer thread runs the agent over 30 seconds.
They operate independently even within the same JVM.

However, a mature product-based company would eventually split them for three
reasons. Independent scaling — the evaluation service is the bottleneck,
not the upload service. I should be able to scale evaluation pods without
scaling upload pods, which I cannot do if they are the same service.
Fault isolation — if the agent loop throws an OutOfMemoryError, it should
not take down the upload API. Candidates should still be able to submit
resumes even if evaluation is temporarily down.
Independent deployment — when I update the agent prompt or scoring logic,
I should not need to redeploy and restart the upload service.

The key architectural property I preserved is that Kafka sits between them.
Splitting into two services later means moving only the consumer code to a
new project. The Kafka topic, the producer, the contract — none of that
changes. This is the real value of putting Kafka in between even when both
sides are in the same service.

---

**Q16. How would you scale SmartHire to handle 10,000 resumes per day?** 🔥

Current capacity with concurrency=3 and 30 seconds per evaluation:
3 evaluations per minute, about 4,000 per day. So 10,000 requires roughly
2.5x throughput.

I would approach it in layers. First, increase Kafka partitions from 3 to 30
and deploy 10 evaluation pods each with concurrency=3. That gives 90 parallel
evaluations and handles well over 10,000 per day.

Second, optimize cost. At 10,000 evaluations per day with gpt-4o-mini at
roughly $0.0005 per evaluation, API cost is $5 per day — acceptable.
I would add Redis semantic caching for candidates with identical skill sets,
which is common. If two candidates both have Java, Spring Boot, and Kafka
with similar profiles, the second evaluation can use cached results.

Third, the pgvector search becomes a bottleneck at scale. I would add
connection pooling tuning via HikariCP and potentially read replicas
for the vector store queries, which are read-heavy.

Fourth, add Kubernetes HPA based on Kafka consumer lag metric. When lag
exceeds 100 messages, scale out. When lag drops to zero, scale in.

---

# SECTION 3 — PGVECTOR AND VECTOR DATABASES

---

**Q17. What is a vector database and why do you need one?** ⭐

A vector database stores high-dimensional numerical representations of
data — called embeddings — and allows you to search by semantic similarity
rather than exact match.

The problem it solves is this: a candidate's resume says "built distributed
systems with Java." A job description says "microservices engineering with
Spring Boot." These have no common keywords, so a SQL LIKE search returns
nothing. But semantically they are similar. A vector database finds this
similarity by measuring the distance between their embedding vectors.

In SmartHire, I embed all job descriptions when they are created and store
the vectors in pgvector. When evaluating a candidate, I embed their skills
and search for the closest job description vectors. The database returns
semantically similar roles even when there is no exact keyword overlap.

---

**Q18. Why pgvector instead of Pinecone or Weaviate?** ⭐

The most pragmatic reason is that pgvector is a PostgreSQL extension.
SmartHire already uses PostgreSQL for structured data — job_roles and
agent_steps. Adding pgvector means zero additional infrastructure, zero
additional operational overhead, and both the structured data writes
and the vector writes can be in the same ACID transaction.

If a job role is saved to the job_roles table and the vector insertion
fails, @Transactional rolls back both. With a separate Pinecone instance,
you would have a distributed transaction problem — the PostgreSQL save
might succeed but the Pinecone insert fails, leaving inconsistent state.

The tradeoff is scale. pgvector is excellent for hundreds of thousands
of vectors. For tens of millions of vectors with strict sub-millisecond
SLA requirements, Pinecone or Qdrant have better performance. SmartHire
deals with thousands of job descriptions, so pgvector is the right choice.
I would revisit when the vector count exceeds a few hundred thousand.

---

**Q19. Explain cosine similarity. Why is it better than Euclidean distance for text?** 🔥

Cosine similarity measures the angle between two vectors, not the distance
between their endpoints. It returns 1 for identical direction, 0 for
perpendicular, and -1 for opposite directions.

The formula is the dot product of two vectors divided by the product of
their magnitudes.

For text embeddings, cosine similarity is preferred over Euclidean distance
because text embeddings have the same magnitude by convention — they are
normalized to unit length. More importantly, a short document and a long
document on the same topic should have similar embeddings even though
their Euclidean distance might be large due to different scales.
Cosine similarity captures directional similarity regardless of magnitude.

In practical terms: "Java backend developer with Spring Boot experience" and
"experienced Java engineer specializing in Spring Boot microservices" are
semantically the same. Their embeddings point in the same direction in the
1536-dimensional space. Cosine similarity will be close to 1. Their Euclidean
distance might be non-trivial due to different text lengths generating
slightly different magnitude embeddings.

---

**Q20. What is HNSW and why does SmartHire use it?** 🔥

HNSW stands for Hierarchical Navigable Small World. It is a graph-based
data structure for approximate nearest neighbor search.

The intuition is layers. At the top layer, each node connects to distant
nodes — allowing fast traversal of the search space. At the bottom layer,
nodes connect to their nearest neighbors — allowing precise local search.
To query, you enter at the top, greedily navigate toward the query vector,
drill down to lower layers, and refine at each level.

SmartHire configures this with:
spring.ai.vectorstore.pgvector.index-type: HNSW

The alternative is flat scan — compute exact cosine similarity against
every vector. For 1000 job descriptions, flat scan is fine. For 1 million,
it becomes too slow. HNSW trades a small amount of accuracy for dramatic
speed improvement — typically 100x to 1000x faster than flat scan with
less than 1% accuracy loss.

The approximate part means it might not always return the absolute closest
vector but will return a vector that is very close. For job matching, this
is perfectly acceptable. The difference between the 1st and 2nd closest
role is rarely meaningful.

---

**Q21. What is metadata filtering and how does it work in pgvector?** 🔥

Vector similarity search finds semantically similar documents across all
stored vectors. But sometimes you need to combine semantic similarity with
structured constraints — for example, only search among OPEN roles.

Spring AI's metadata filtering adds a WHERE clause to the similarity search:

```
SearchRequest.query(skills).withFilterExpression("status == 'OPEN'")
```

Internally, this translates to a PostgreSQL query:
```
SELECT content, metadata, 1 - (embedding <=> $1) AS score
FROM vector_store
WHERE metadata->>'status' = 'OPEN'
ORDER BY embedding <=> $1
LIMIT 5;
```

The metadata is stored as JSONB alongside each vector. The filter operates
on the JSONB column, and the similarity operates on the vector column.
This is the advantage of pgvector over standalone vector databases —
PostgreSQL's full query engine is available including JSON path operations,
secondary indexes on metadata fields, and joins with other tables.

---

# SECTION 4 — SPRING AI AND EMBEDDINGS

---

**Q22. Explain how embeddings are generated and what they represent.** ⭐

An embedding is a dense numerical vector representation of text. You send
text to an embedding model — in SmartHire, text-embedding-3-small — and
it returns a vector of 1536 floating point numbers. The embedding model
was trained to place semantically similar texts close to each other in
this 1536-dimensional space.

When I call vectorStore.add(documents), Spring AI calls the OpenAI
embeddings API for each document's text, receives the 1536-dimensional
vector, and stores it in the pgvector vector column alongside the
original text and metadata.

When I query with vectorStore.similaritySearch(request), Spring AI
first embeds the query string — making an API call to get its 1536-d vector
— then finds the stored vectors with the smallest cosine distance.

The key property is that the embedding model is consistent. The same text
always produces the same embedding. Similar texts produce similar embeddings.
This consistency is what makes semantic search work.

---

**Q23. Why use text-embedding-3-small instead of text-embedding-3-large?** 🔥

text-embedding-3-small produces 1536-dimensional vectors.
text-embedding-3-large produces 3072-dimensional vectors.

For SmartHire's job matching use case, small provides sufficient semantic
quality. The difference in matching accuracy between small and large for
job description similarity is marginal — both models understand that Java
and Spring Boot are related, that microservices and distributed systems
are related. The extra 1536 dimensions in large capture subtler semantic
nuances that matter more for tasks like long-document understanding or
cross-language retrieval.

The cost difference is significant. text-embedding-3-small is approximately
5x cheaper than large. At 10,000 job descriptions indexed, this is a
meaningful saving. And the storage difference — 1536 vs 3072 floats per
vector — directly affects the size of the pgvector index and query speed.

I would switch to large if I measured meaningful quality degradation in
job matching accuracy on a labeled test set. But you start with small and
upgrade when you have evidence that you need to.

---

# SECTION 5 — SYSTEM DESIGN

---

**Q24. Design SmartHire from scratch. Walk me through your architecture decisions.** ⭐

I start by clarifying requirements. What scale — resumes per day? What
latency — how quickly should a candidate hear back? What accuracy — what
is the acceptable wrong-decision rate? What privacy — can resume data go
to external APIs?

Assuming: 1000 resumes per day, results within 5 minutes, and external
APIs acceptable.

The core architectural decision is async processing. Agent evaluation
takes 30 seconds. This cannot be synchronous. So: upload API publishes
to Kafka, consumer runs agent, client polls for result. This decouples
ingestion from evaluation and makes both independently scalable.

For the agent, I use LangChain4j AiServices with @Tool methods. The tools
cover: parse skills from resume, find matching roles via semantic search,
score fit, check availability, schedule interview, notify candidate.
The LLM decides the flow at runtime.

For job matching, pgvector on PostgreSQL. Both structured data and vectors
in one database eliminates a distributed transaction problem.

For reliability: Resilience4j rate limiter to protect the OpenAI API budget,
circuit breaker to handle API outages, dead letter topic for permanent failures.

For observability: every tool call logged to agent_steps, Kafka consumer lag
monitored, token usage tracked per evaluation for cost visibility.

For testing: Testcontainers with real PostgreSQL and real Kafka — no mocks
for infrastructure.

---

**Q25. How does the data flow from resume upload to interview scheduled?** ⭐

Step one: candidate uploads a PDF. The controller extracts the text using
Spring AI's PagePdfDocumentReader, generates a UUID evaluationId, and
creates a ResumeUploadEvent.

Step two: the event is published to the resume-uploaded-events Kafka topic
with evaluationId as the key. The controller returns 202 Accepted immediately.

Step three: the Kafka consumer picks up the event. This runs in a separate
thread pool, completely decoupled from the HTTP thread.

Step four: AgentOrchestrationService initializes the audit trail with the
evaluationId and calls recruitingAgent.evaluate(resumeText).

Step five: LangChain4j runs the agent loop. The LLM calls tools in order,
each tool writes an AgentStep to PostgreSQL, and the loop continues for
approximately 15 to 30 seconds.

Step six: when the agent returns, the result is saved to evaluation_results
in PostgreSQL and published to evaluation-complete-events.

Step seven: the client, which has been polling GET /result/{evaluationId}
every 5 seconds, receives a 200 with status COMPLETE and the full result.

---

**Q26. How would you handle a situation where OpenAI is down?** ⭐

Three layers of handling.

First, Resilience4j circuit breaker detects the failure. If 50% of LLM calls
in the last 10-request window fail, the circuit opens. All subsequent agent
evaluation attempts immediately return the fallback response without waiting
for a timeout. This prevents threads from piling up waiting for a dead API.

Second, the Kafka consumer catches the exception and publishes to the
evaluation-failed-events dead letter topic. The event is not lost — it
is durably stored in Kafka waiting for manual or automated replay.

Third, after 30 seconds, the circuit breaker enters half-open state and
sends a probe request to OpenAI. If it succeeds, the circuit closes and
normal processing resumes. When it closes, I can replay events from the
dead letter topic to process evaluations that were queued during the outage.

From the candidate's perspective: their resume was accepted (202 Accepted
was returned), they will receive a result — just slightly later than usual.
The upload experience is unaffected by the LLM being down.

---

**Q27. How do you ensure consistency between the PostgreSQL job_roles table and pgvector?** 🔥

Both tables live in the same PostgreSQL instance. pgvector is a PostgreSQL
extension, not a separate system. So I can wrap both writes in a single
@Transactional method.

In JobIndexingService.createAndIndexRole():
- jobRoleRepository.save(role) writes to the job_roles table
- vectorStore.add(doc) writes to the vector_store table

Both are in the same transaction. If the vector insertion fails after the
JPA save succeeds, Spring's transaction manager rolls back the JPA save.
The database is always consistent — either both records exist or neither does.

This is a significant advantage of pgvector over a dedicated vector database
like Pinecone. With Pinecone, you have two separate systems and no distributed
transaction support. A failure between the two writes would require a
reconciliation job to detect and fix inconsistencies.

---

**Q28. How do you handle the case where a candidate applies twice?** 🔥

This is an idempotency concern. The evaluationId is generated fresh for each
upload — so two uploads by the same candidate produce two different
evaluationIDs and two separate evaluations. That is the correct behavior —
a candidate improving their resume and reapplying should get a fresh evaluation.

The idempotency protection is against Kafka at-least-once delivery causing
the same upload event to be processed twice. I check
resultRepository.existsByEvaluationId(evaluationId) at the start of
the consumer. If the evaluation already exists, I skip processing and return.

The candidate deduplication — should the same email be evaluated multiple
times — is a business rule, not a technical rule. I would implement it
as a configurable policy: allow re-evaluation after 30 days, or always
allow it if the resume content changed. This would be a query against
the evaluation_results table by email before publishing the Kafka event.

---

# SECTION 6 — SPRING BOOT AND JAVA

---

**Q29. Why constructor injection instead of @Autowired field injection?** ⭐

Three reasons. Testability — with field injection, I cannot inject mock
dependencies in unit tests without using Spring's test context, which is
slow. With constructor injection, I create the service with mock dependencies
directly: new AgentOrchestrationService(mockAgent, mockTools). Tests are
fast and Spring-independent.

Immutability — constructor-injected dependencies are final. They cannot be
reassigned after construction. Field-injected dependencies can be replaced
at any time, which is a source of subtle bugs especially in multithreaded
environments.

Fail-fast — if a required dependency is missing, the application fails
at startup with a clear error. Field injection fails at the first call to
the dependent method, which could be much later and harder to diagnose.

Spring itself recommends constructor injection for mandatory dependencies.
The @RequiredArgsConstructor from Lombok makes it ergonomic — it generates
the constructor for all final fields automatically.

---

**Q30. Explain @Transactional and when you need it.** ⭐

@Transactional wraps a method in a database transaction. Either all the
database operations in the method succeed and are committed, or any failure
causes all of them to be rolled back.

In SmartHire, createAndIndexRole saves to job_roles and then adds to
vector_store. Without @Transactional, if the vector store insertion fails,
the job_roles record is already committed — the database is inconsistent.
With @Transactional, both succeed or both are rolled back.

The key things to understand about @Transactional in Spring: it works via
a proxy — Spring wraps the bean in a proxy that intercepts calls and manages
the transaction. Self-invocation — calling a @Transactional method from
within the same class bypasses the proxy and the transaction is not applied.
Propagation — REQUIRED is the default, meaning the method joins an existing
transaction or creates a new one. And the default rollback is only for
unchecked exceptions. Checked exceptions do not trigger rollback unless
you specify rollbackFor.

---

**Q31. What is the difference between @Component, @Service, and @Repository?** ⭐

All three are specializations of @Component — they all result in Spring
creating a bean and managing it in the application context.

The difference is semantic and functional. @Component is the generic annotation
— use it for beans that do not fit a specific layer. @Service indicates a
business logic bean — service layer. @Repository indicates a data access
bean — it also activates Spring's exception translation, converting
JPA-specific exceptions like PersistenceException into Spring's unified
DataAccessException hierarchy. @Controller indicates an MVC controller.
@RestController is @Controller + @ResponseBody.

In SmartHire: AgentOrchestrationService and JobIndexingService are @Service.
Repositories are interfaces extending JpaRepository — Spring Data creates
implementations and handles @Repository behavior automatically.

---

**Q32. Explain the Builder pattern used in SmartHire. Why use it?** ⭐

The Builder pattern separates object construction from representation.
Instead of a constructor with many parameters, you chain setter-like methods
and call build() at the end.

In SmartHire, AgentStep is built as:
```
AgentStep.builder()
    .evaluationId(evalId)
    .stepNumber(++stepCounter)
    .toolName(toolName)
    .toolInput(input)
    .toolOutput(output)
    .build();
```

Two reasons to prefer this over a constructor. Readability — you can see
what each value represents at the call site. With a 6-parameter constructor,
builder.build() is confusing. With builder, every field is labeled.
Safety — with Lombok's @Builder, if I add a new required field to AgentStep,
every existing builder call that does not set the new field will still compile.
I can review each call site deliberately rather than having the compiler
break every constructor call across the codebase.

---

**Q33. What are Java records and why use them for DTOs?** ⭐

A record is a special class declaration introduced in Java 16. It declares
a fixed set of immutable fields and automatically generates the constructor,
getters, equals, hashCode, and toString.

For ResumeUploadEvent and EvaluationResult, records are ideal because
these are pure data transfer objects — they carry data between layers and
have no behavior. Making them immutable — via record — means once created,
no code can accidentally modify them in transit. This is important in
concurrent systems like Kafka consumers where the same event object might
be referenced across threads.

The accessor methods are named evaluationId() not getEvaluationId() —
this is the record convention. Jackson handles this correctly for JSON
serialization without any extra configuration.

One limitation: JPA entities cannot be records because Hibernate requires
a no-arg constructor and mutable fields. Entities should remain regular
classes. Records for DTOs, regular classes for entities — that is the clean split.

---

# SECTION 7 — RESILIENCE AND RELIABILITY

---

**Q34. Explain the circuit breaker pattern. Draw the state machine.** ⭐

The circuit breaker pattern prevents cascading failures when a downstream
service is unavailable. It works like an electrical circuit breaker —
it trips when it detects too many failures and blocks further calls until
the service recovers.

Three states. CLOSED is normal operation — requests pass through to the LLM.
The circuit breaker tracks the failure rate in a sliding window. If the
failure rate exceeds the threshold — in SmartHire, 50% of the last 10 calls
— the circuit OPENS.

OPEN means the circuit is tripped. All requests fail immediately with the
fallback response, without attempting to call OpenAI. This is fast-fail —
no timeouts, no thread blocking. After a configured wait period — 30 seconds
in SmartHire — the circuit enters HALF_OPEN.

HALF_OPEN allows a limited number of probe requests through. If they succeed,
the circuit CLOSES and normal operation resumes. If they fail, it returns
to OPEN and the wait period resets.

The benefit over simple retry: during an OpenAI outage, retries would keep
threads blocked for timeout duration before failing. With circuit breaker,
the first few requests detect the failure, open the circuit, and subsequent
requests fail in microseconds. The system stays responsive even with a dead
dependency.

---

**Q35. What is a rate limiter and how does it differ from a circuit breaker?** 🔥

A rate limiter controls throughput — it limits how many requests are sent
in a given time window. In SmartHire, 50 LLM calls per minute. This protects
against exceeding the OpenAI API's rate limits, which would result in
429 Too Many Requests errors.

A circuit breaker responds to failures — it stops sending requests when too
many are failing. It protects against cascading failures to a degraded service.

They solve different problems and work together. The rate limiter prevents
you from overloading the LLM API when it is healthy. The circuit breaker
protects you when the API is down or degraded. In SmartHire both are stacked
on the same method — the rate limiter runs first, and if a permit is available,
the circuit breaker decides whether to let the call through based on recent
failure history.

A practical difference: rate limiter is proactive — it caps throughput
regardless of whether requests succeed or fail. Circuit breaker is reactive —
it responds to observed failure patterns.

---

# SECTION 8 — TESTING

---

**Q36. Why Testcontainers instead of H2 in-memory database?** ⭐

H2 is convenient but it has fundamental limitations. H2 is not PostgreSQL.
It does not support pgvector — so any test involving the vector store
would fail or need to be mocked. H2's SQL dialect is subtly different from
PostgreSQL — queries that work in H2 can fail in production, and vice versa.
H2 does not replicate PostgreSQL's JSONB behavior, index types, or locking
semantics.

Testcontainers spins up a real PostgreSQL container with pgvector installed.
Every test runs against the exact same database engine as production.
The only difference is the container is temporary and torn down after tests.
This means if my HNSW index query works in the test, it works in production.
If my JSONB metadata filter works in the test, it works in production.

The tradeoff is speed — Testcontainers is slower than H2 because it starts
a real Docker container. But the confidence it provides is worth the extra
seconds. And with Docker layer caching, after the first pull the container
starts in under 5 seconds.

---

**Q37. What does @ServiceConnection do and how does it work?** 🔥

@ServiceConnection is a Spring Boot 3.1+ annotation that automatically
configures the application to connect to a Testcontainers container.

Without @ServiceConnection, I would need to manually extract the container's
dynamic port and override Spring properties:
```java
@DynamicPropertySource
static void setProperties(DynamicPropertyRegistry registry) {
    registry.add("spring.datasource.url", postgres::getJdbcUrl);
    registry.add("spring.datasource.username", postgres::getUsername);
    registry.add("spring.datasource.password", postgres::getPassword);
}
```

With @ServiceConnection, Spring Boot detects the container type — it knows
PostgreSQLContainer maps to datasource properties — and configures everything
automatically. No manual property wiring. This works because Spring Boot
ships connection details handlers for common container types: PostgreSQL,
Redis, Kafka, MongoDB, and others.

---

# SECTION 9 — BEHAVIORAL AND ARCHITECTURE THINKING

---

**Q38. You joined a team and found producer and consumer in the same service.
       What would you do?** 🔥

First, I would understand why. There is usually a reason — simplicity,
team size, early-stage product. I would not immediately push to split them
without understanding the context and measuring the actual pain.

I would ask: are we hitting scaling issues? Is a failure in the evaluation
logic causing upload outages? Are deployments of agent changes causing
downtime for the upload API? If the answer to all three is no, I would
document the current architecture, add a note about the known limitation,
and focus engineering effort elsewhere.

If the answer to any of those is yes, I would propose splitting with a
clear migration plan. Since Kafka already sits between them, the migration
is low-risk. I would extract the consumer and orchestration service into a
new Spring Boot project, pointing to the same Kafka cluster and the same
PostgreSQL instance. The upload service changes not at all. I would deploy
the new service alongside the old one, drain the consumer from the original
service, and validate the new service is processing correctly before removing
the consumer code from the original.

This incremental approach means zero downtime and rollback is trivial.

---

**Q39. A recruiter complains the agent rejected a strong candidate.
       How do you debug it?** 🔥

This is exactly why the audit trail exists. I query agent_steps by
evaluationId for that candidate's evaluation.

I look at what parseResumeSkills extracted — did it correctly identify
all the candidate's skills or did it miss something? If the resume was
formatted unusually — tables, graphics, non-standard sections — the PDF
text extraction might have produced garbled output. That is the first
place failures happen.

Then I look at findMatchingRoles — did it retrieve the right job description
or did it match against a tangentially related role? I check the similarity
scores returned. If the best match was only 0.56 similarity, the candidate's
language might differ enough from the job description language that the
embedding search is not finding the right role.

Then scoreCandidate — I look at what skills were matched versus missing.
Maybe the candidate listed "J2EE" but the job description requires "Java."
Semantically identical but the keyword matcher did not catch it. This would
be a signal to improve the scoring logic to use semantic comparison rather
than exact string matching.

The audit trail turns debugging from a black box investigation into a
step-by-step trace. This is why I built it — not just for compliance,
but for this exact scenario.

---

**Q40. How would you A/B test a new agent prompt?** 🔥

I would implement feature flags at the evaluation request level. When
a new prompt is ready, I deploy both versions: control (existing prompt)
and treatment (new prompt). I route 10% of evaluations to the treatment
based on a hash of the evaluationId — this ensures consistent assignment
for the same evaluation.

I measure three things: decision accuracy against my labeled test dataset,
average tool call count per evaluation (efficiency), and human recruiter
agreement rate when I show both decisions to a recruiter blind.

After 500 evaluations in each group — enough for statistical significance —
I compare the metrics. If the treatment is neutral or better on all three,
I promote it to 100%. If it is worse on any metric, I roll back.

The critical thing is the labeled test dataset. Without ground truth, you
cannot know if a new prompt is better — you can only measure it differently.
Maintaining that dataset and expanding it with each new hiring cycle is
the ongoing investment that makes reliable iteration possible.

---

## Summary — The 5 answers that separate you from other candidates

**1. On agents:** "The LLM controls the flow. My Java code declares the goal and tools.
The LLM decides which tools to call and when to stop."

**2. On Kafka:** "202 Accepted plus Kafka is the standard pattern for any AI workload
over 2 seconds. Durability, scalability, and observability are things a thread pool cannot give you."

**3. On pgvector:** "Same PostgreSQL instance as structured data. Same @Transactional
scope. No distributed transaction problem. Right choice until you hit millions of vectors."

**4. On agent reliability:** "Every tool call logged to agent_steps. Rate limiter protects
the API budget. Circuit breaker handles outages. Dead letter topic catches permanent failures."

**5. On architecture evolution:** "Producer and consumer in the same service is acceptable
at this stage because Kafka sits between them. Splitting later means moving only the consumer.
The contract does not change."
