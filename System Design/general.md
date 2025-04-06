What interviewers say vs what they really mean (system design edition) 



— "Let's start with a simple problem." 

👉 It’s not simple. You’re about to design a system more complex than your entire experience overall with any of the systems?

— "How would you handle a billion users?" 

👉 You probably won’t ever have a billion users, but I want to see if you panic. 

— "How would you scale this?" 

👉 I’m waiting for you to forget about consistency, caching, or rate limiting. 

— "What database would you use?" 

👉 Say SQL, I’ll ask why not NoSQL. Say NoSQL, I’ll ask why not SQL. 

— "How would you handle failures?" 

👉 If you don’t mention retries, circuit breakers, and failover strategies, I will fail YOU. 

— "Explain your trade-offs." 

👉 If you don’t mention CAP theorem, I’m legally required to shake my head in disappointment. 

— "How would you improve this design?" 

👉 I hope you don’t realize I left obvious flaws just to see if you’d catch them. 

— "Any questions for me?" 
👉 If you ask about work-life balance, I will deflect harder than a failing load balancer. 

## Mostly made mistakes:

→ Nothing is more underestimated than database indexing—until queries start timing out. 

👉  Be prepared to discuss indexing strategies, when to use composite indexes, and how they impact read vs. write performance. 

→  Nothing is more feared than the words "Let's add a queue to fix this." 

👉  Message queues (Kafka, RabbitMQ, SQS) solve problems but also introduce new ones (ordering, deduplication, backpressure). Understand trade-offs. 

→  Nothing is more abused than Redis as a primary database. 

👉  In an interview, clarify when caching is useful vs. when it becomes a single point of failure. Know Redis eviction policies and discuss cache invalidation strategies. 

→  Nothing is more painful than debugging a distributed system with "eventual consistency." 

👉  Be ready to explain strong vs. eventual consistency, CAP theorem, and how systems like DynamoDB or Cassandra handle consistency trade-offs. 

→  Nothing is more misleading than a system working perfectly in a local dev environment. 

👉  Show that you understand real-world deployment challenges like latency, network failures, and observability. 

→  Nothing is more terrifying than “We’ll handle retries on the client side.” 

👉  Be ready to discuss retry strategies, exponential backoff, and idempotency to prevent cascading failures. 

→  Nothing is more optimistic than "This will scale just fine." 

👉  Always justify scaling decisions. Horizontal vs. vertical scaling? Load balancing strategies? Partitioning? Show that you can predict bottlenecks. 

 →  Nothing is more career-defining than the first time you have to scale a monolith under heavy load. 

👉  Be prepared to discuss monolith vs. microservices, when to split services, and the hidden costs of distributed systems (latency, data consistency). 
__

