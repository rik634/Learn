## Netflix:

Netflix isn’t just about playing videos - it’s a highly optimized, fault-tolerant, and globally distributed architecture that handles billions of requests efficiently.

Let's try to understand this by breaking down the system into five key components that power Netflix’s system:

1. Load Balancing & Traffic Routing – ELB (Elastic Load Balancer) and Open Connect efficiently route traffic, ensuring minimal latency.

2. API Gateway & Request Filtering – Zuul Gateway applies security, authentication, and traffic filtering for optimized request handling.

3. Microservices & Data Storage – Netflix relies on microservices, using Cassandra for distributed storage and MySQL for billing and metadata.

4. Event Processing & Monitoring – Kafka, Apache Samza, and Mantis process real-time streaming data, monitor logs, and optimize system performance.

5. Video Processing & Content Delivery – S3, Amazon EMR, and Netflix’s transcoding pipeline ensure fast video encoding and seamless content streaming.

![1741905864115](https://github.com/user-attachments/assets/587cfa1e-e774-4616-ad0b-a05918887997)
