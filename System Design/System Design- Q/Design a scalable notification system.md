# Notification System

"I’ll build a modular system with Kafka for queuing, Redis for deduplication, and a microservice to handle delivery retries." 

- Focuses on scalability and reliability. 
- System handles high traffic but doesn’t consider long-term maintainability or cost-effectiveness. 

Senior Engineer: "What’s the business use case for these notifications? Are they time-sensitive, or can they be batched? Should we optimize for cost, speed, or reliability?" 

- Focuses on business goals first: 
 → Suggests prioritizing urgent notifications like OTPs while batching less critical ones. 
 → Recommends monitoring tools for visibility into system health. 
 → Evaluates whether to build in-house or integrate an external service to reduce costs. 
