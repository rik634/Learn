# Client server architecture and network basics:
## Scale from zero to millions of users
### Single server setup
<img width="892" height="531" alt="image" src="https://github.com/user-attachments/assets/fcf10f6e-0af6-456a-9fcf-3765c31d8100" />
- Lets understand the request flow:

  1. User access the site through domain name, such as "api.mysite.com". The request goes with this site url to the DNSm which returns the IP address of the site. Usually, DNS (Domain Name System) is a paid service provided by 3rd parties and not hosted by our servers.
  2. Internet Protocol (IP) address is returned back to the browser or mobile app. In the above example  the IP address 15.125.23.214 is returned.
  3. Once the IP address is obtained, HTTP (HyperText Transfer Protocol) request is sent to the web server.
  4. The web server returns the HTML pages or JSON response for rendering.

- The traffic to our web server comes from 2 sources: Web application and mobile application
  1. Web application:
     - A web application generally uses the combination of server side languages like Java, Python etc, to handle business logic, storage etc, and client side languages like JavaScript, HTMLO etc.
  2. Mobile application:
     - HTTP is the communication protocol between mobile app and web server. JSON (JavaScript Object Notation) is the commonly used API response format for data transfer due to its simplicity.  

### Database
- With the increase in the number of users, one server is not enough to deal with the traffic. Here, we need multiple servers, one for web/mobile traffic, another for database.
- Separating web/mobile traffic (web tier) and database (data tier) server allows both of them to be scaled independently.

  <img width="999" height="539" alt="image" src="https://github.com/user-attachments/assets/4e1a1696-e998-4e70-b1b1-a43634a89f56" />

- There are 2 types of databases we can use:

  1. Relational database:
     - These are also known as RDBMS(Relational database management system) or SQL databases.
     - example: MySQL, Oracle database, PostgreSQL etc.
     - Here, the data is stored in form of rows and columns.
     - We can perform join operation using SQL across different database table.  
  2. Non-relational database:
     - These are also known as NoSQL database.
     - Example: CouchDB, Neo4j, Cassandra, DynamoDB, MongoDB etc.
     - These databases are fuether divided into below categories:
 
       1. key-value stores
       2. graph stores
       3. column store
       4. document store 
     - Join  operation is not supported 
     - Most of the people go for relational databases.
     - We go for non-relational database if:
 
       1. Application requires super-low latency
       2. Data is undtructured, and we don't have any structured data.
       3. We only need to serialize and deserialize data (JSON, XML, YAML etc)
       4. We need to store massive amount of data.

### Vertical and Horizontal scaling:
1. Vertical scaling:

   - It is also referred as "scale up", means the process of addin more power (CPU, RAM etc) to our server.
   - When traffic is low, vertical scaling is great option, and simplicity of of vertical scaling is its main advantage.
   - It also comes with various limitation:

     1. Vertical scaling has a hard limit, it is  impossible to add unlimited CPU and memory to single server after certain point.
     2. Vertical scaling does not have failover and redundancy. If one server goes down, the website/ app goes down with it completely. It has single point of failure. 
   - 
1. Horizontal scaling:

   - It is also referred as "scale-out", allows us to scale by adding more server to the pool of resources.
   - It is more desirable for large scale application due to limitations of vertical scaling.

### Load balancer:
- A load balancer is used to evenly distribute the traffic among the web servers defined in load-balanced set.

  <img width="865" height="613" alt="image" src="https://github.com/user-attachments/assets/056f3176-04fd-4382-ab6a-30becdcb032e" />

-  With this setup, web servers are unreachable directly by clients anymore.
-  As shown in above figure, users connect to public IP of load balancer directly.
-  For better security, private IPs are used communication between servers. A private IP is an IP address reachable only between the servers in the same network; however it is unreachable over the internet.
-  The load balancer communicates with web servers through private IPs.
-  By adding a load balancer, and horizontally scaling the web servers, we have solved the issue of failover, and improved the availabiity of web tier. Details are as below:

   1. If one server fails, traffic is routed to another server, maintaining the availability of web tier.
   2. Also, if the number of users increase in the future, we can easily add more number of servers, and load balancer will take care of distributing the traffic. 

- The current design has one database, so it does not support failover and redundancy. Database replication is common technique to address those problems.

### Database replication:
- Database replication can be used in many database management systems, usually with master/slave relationship between original (master) and the copies (slaves).
- A master database generally only supports write operations. A slave database gets copies of data from the master database and only supports read operations.
- All the data-modifying commands like insert, deleta and update must be sent to the master database.
- Most applications require a much higher ratio of read to writes; thus, the number of slave databses in a system is usually larger than the number of master databases.

  <img width="604" height="633" alt="image" src="https://github.com/user-attachments/assets/0b4e4de1-dd2e-4306-ba0e-310bf081ec80" />

- Advantages of database replication:

  1. Better performance:
     - In master-slave model, all write and update operations happen in master node; whereas, read operations are distributed across slave nodes. This model improves performance because it allows more queries to be processed in parallel.
  2. Reliability:
     - If one of our database servers is destroyed by a natural disaster, data is still preserved. We do not need to worry about data loss as data is replicated across multiple locations.  
  3. High availability:
     - By replicating data across different locations, our website remains in operation even if the database is offline, as we can access data stored in another daatabse server.   

- Let's understand the case: what is one of the databases goes offline?

  -  If only one slave database is available and it goes offline, read operations will be redirected to the master database temporarily. As soon as the issue is found, a new slave database will replace the old one. In case multiple slave databases are available, read operations are redirected to other healthy slave databases. A new database server will replace the old one.
  -  If the master databse goes offline, a slave databse will be promoted to be new master. All the database operations will be temporarily executed on the new master database. A new slave database will replace the old one for data replication immediately. In production systems, promoting a new master is more complicated as the data in slave database might not be up to date. The missing data needs to be updated by running the data recovery scripts. Although some other replication methods like multi-master and circular replication could help, those setups are more complicated.

 <img width="687" height="661" alt="image" src="https://github.com/user-attachments/assets/dc908380-3dc7-43e6-8f7f-867a151a882b" />

- Let's understand the above flow:

  1. A user gets the IP address of load balancer from DNS.
  2. A user connects with load balancer using this IP address.
  3. The HTTP request is routed to either server-1 or server-2.
  4. A web server reads the user data from a slave database.
  5. A web server routes any data-modifying operations to master dataabse. This includes write, delate and update operations.

- Now, we can improve the load/response time by adding a cache layer and shifting static content (JavaScript/CSS/imgae/video files) to the CDN (Content Delivery Network).

### Cache
- A cache is temporary storage area that stores the result of expensive operations or frequently accessed data in memory so that subsequent requests are served more quickly.
- Example: For fetching anyweb pages, one or more database calls are executed. The application performance is greatly affected by calling the database repeatedly. The cache can mitigate this problem.

#### Cache tier:
- The cache tier is a temporary data store layer, much faster than the database.
- The benefits of having a separate cache lier includes better system performance, ability to reduce the database workload, and the ability to scale the cache tier independently.
- The below shows the possible setup for cache server:

  <img width="910" height="153" alt="image" src="https://github.com/user-attachments/assets/729ae4a2-4e30-4d3e-a440-c75cdd3f8ac7" />

- After receiving a request, a web server first checks if the cache has the available response. If it has, it sends the data back to the client. If not, it queries the dataabse, stores the resonse in cache , and sends it back to the client.
- This caching strategy is called as read-through cache. Other caching strategies are avaiable depending on the data type, size and access patterns.
- Interacting with cache servers is simple because most cache servers provide APIs for common programming languages. The following code shows typical memcached API:

  <img width="474" height="87" alt="image" src="https://github.com/user-attachments/assets/4c03cfc8-1861-49f5-929a-fc206a7f5cfa" />

#### Considerations for using cache:
- Here are a few considerations for using cache system:

  1. Decide when to use cache. Consider using cahce when data is read frequently but modified infrequently. Since cached data is stored in volatile memorya cache server is not idle for persisting data. For instance, if a cache server restarts, all the data in memory is lost. Thus, important data should be stored in persistent data stores.
  2. Expiration policy: It is an good practice to implement expiration policy. Once cached data is expired, it is removed from cache. When there is no expiration policy, cached data will be stored in memory permanently. It is advised not to make the expiration date too short as this will cause the system to reload data from database too frequently. Meanwhile, it is advisable not to make the expiration date too long as data can become stale.
  3. Consistency: This involves keeping the data store and cache in sync. Inconsistency can happen because data-modifying operations on the data store and cache are not in a single transaction. When scaling scross multiple regions, maintaining consistency between data store and cache is challenging.
  4. Mitigating failures: A single cache server represents single point of failure  (SPOF is part of system that, if it fails, will stop the entire system from working). As a result, multiple cache servers across different data centers are recommended to avoid SPOF.Another recommended approach is to overprovision the required memory by certain percentages. This provides a buffer as memory usage increases.
  5. Eviction policy: Once the cache is full, any request to add items to cache might cause existing items to be removed. This is called cache eviction. Least-recently-used (LRU) is the most popular cache eviction policy. Other eviction policies, such as Least frequently used (LFU) or First in first out (FIFO) can be adopted to satisfy different use cases.  

### CDN
- A CDN is a network of geographically dispersed servers used to deliver static content.  CDN servers cache static content such as images, videos, CSS, javascript files etc.
- Dynamic content cache is new concept, which enables caching of HTML pages that are based on request path, query strings, cookies and request headers etc.
- Below shows the working of CDN at high level:

  <img width="697" height="317" alt="image" src="https://cloud.githubusercontent.com/assets/83593359//13b3cea4-90c6-4af6-a6ad-c554a0c59158.png" />

- When a user visits a website, a CDN server closest to the user will deliver static content. Intuitively, the further users are from CDN servers, the slower the website loads.
- CDN workflow:

  <img width="793" height="264" alt="image" src="https://github.com/user-attachments/assets/bcc29069-b01e-496b-9505-8ca394af0cbc" />

  1. User A tries to get image.png by using an image URL. The URL’s domain is provided by the CDN provider. The following two image URLs are samples used to demonstrate what image URLs look like on Amazon and Akamai CDNs:

     <img width="598" height="74" alt="image" src="https://github.com/user-attachments/assets/aab94d6d-893d-4694-9f12-734939af7e46" />

  2. If the CDN server does not have image.png in the cache, the CDN server requests the file from the origin, which can be a web server or online storage like Amazon S3 
  3. The origin returns image.png to the CDN server, which includes optional HTTP header Time-to-Live (TTL) which describes how long the image is cached.
  4. The CDN caches the image and returns it to User A. The image remains cached in the CDN until the TTL expires.
  5. User B sends a request to get the same image
  6. The image is returned from the cache as long as the TTL has not expired
 
#### Considerations of using CDN:
- Cost: CDNs are run by third-party providers, and you are charged for data transfers in and out of the CDN. Caching infrequently used assets provides no significant benefits so you should consider moving them out of the CDN.
- Setting an appropriate cache expiry: For time-sensitive content, setting a cache expiry time is important. The cache expiry time should neither be too long nor too short. If it is too long, the content might no longer be fresh. If it is too short, it can cause repeat reloading of content from origin servers to the CDN.
- CDN fallback: You should consider how your website/application copes with CDN failure. If there is a temporary CDN outage, clients should be able to detect the problem and request resources from the origin.
- Invalidating files: You can remove a file from the CDN before it expires by performing one of the following operations:

  1. Invalidate the CDN object using APIs provided by CDN vendors.
  2. Use object versioning to serve a different version of the object. To version an object, you can add a parameter to the URL, such as a version number. For example, version number 2 is added to the query string: image.png?v=2.

  <img width="656" height="638" alt="image" src="https://github.com/user-attachments/assets/397b34c3-1f48-4597-a116-bb0d634e41b1" />

- Static assets (JS, CSS, images, etc.,) are no longer served by web servers. They are fetched from the CDN for better performance.
- The database load is lightened by caching data.

### Stateless web tier
- Now it is time to consider scaling the web tier horizontally. For this, we need to move state (for instance user session data) out of the web tier. A good practice is to store session data in the persistent storage such as relational database or NoSQL. Each web server in the cluster can access state data from databases. This is called stateless web tier.

#### Stateful architecture:
- A stateful server and stateless server has some key differences. A stateful server remembers client data (state) from one request to the next. A stateless server keeps no state information.

  <img width="922" height="507" alt="image" src="https://github.com/user-attachments/assets/6df32f42-93d4-41fa-ada0-9d681a9542cb" />

- The above mentioned in example of stateful architecture. In above Figure, user A’s session data and profile image are stored in Server 1. To authenticate User A, HTTP requests must be routed to Server 1. If a request is sent to other servers like Server 2, authentication would fail because Server 2 does not contain User A’s session data. Similarly, all HTTP requests from User B must be routed to Server 2; all requests from User C must be sent to Server 3.
- The issue is that every request from the same client must be routed to the same server. This can be done with sticky sessions in most load balancers [10]; however, this adds the overhead. Adding or removing servers is much more difficult with this approach. It is also challenging to handle server failures.

#### Stateless architecture:

<img width="713" height="545" alt="image" src="https://github.com/user-attachments/assets/55a596d4-b9cf-4c6a-a0a1-ffd14b51f810" />

- In this stateless architecture, HTTP requests from users can be sent to any web servers, which fetch state data from a shared data store. State data is stored in a shared data store and kept out of web servers. A stateless system is simpler, more robust, and scalable.

  <img width="856" height="685" alt="image" src="https://github.com/user-attachments/assets/2825ecc9-b298-47f2-bc71-72f4e0a6a4cd" />

- we move the session data out of the web tier and store them in the persistent data store. The shared data store could be a relational database, Memcached/Redis, NoSQL, etc. The NoSQL data store is chosen as it is easy to scale. Autoscaling means adding or removing web servers automatically based on the traffic load. After the state data is removed out of web servers, auto-scaling of the web tier is easily achieved by adding or removing servers based on traffic load.
- our website grows rapidly and attracts a significant number of users internationally. To improve availability and provide a better user experience across wider geographical areas, supporting multiple data centers is crucial.

### Data centers

<img width="735" height="651" alt="image" src="https://github.com/user-attachments/assets/ccbb0925-cdb8-4899-b1ce-026a908af77d" />

- In normal operation, users are geoDNS-routed, also known as geo-routed, to the closest data center, with a split traffic of x% in US-East and (100 – x)% in US-West. geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user.
- In the event of any significant data center outage, we direct all traffic to a healthy data center. The data center 2 (US-West) is offline, and 100% of the traffic is routed to data center 1 (US-East)
- Several technical challenges must be resolved to achieve multi-data center setup:

  1. Traffic redirection: Effective tools are needed to direct traffic to the correct data center. GeoDNS can be used to direct traffic to the nearest data center depending on where a user is located.
  2. Data synchronization: Users from different regions could use different local databases or caches. In failover cases, traffic might be routed to a data center where data is unavailable. A common strategy is to replicate data across multiple data centers. A previous study shows how Netflix implements asynchronous multi-data center replication.
  3. Test and deployment: With multi-data center setup, it is important to test your website/application at different locations. Automated deployment tools are vital to keep services consistent through all the data centers. 

- To further scale our system, we need to decouple different components of the system so they can be scaled independently. Messaging queue is a key strategy employed by many real-world distributed systems to solve this problem.

### Message queues:
- A message queue is a durable component, stored in memory, that supports asynchronous communication. It serves as a buffer and distributes asynchronous requests. The basic architecture of a message queue is simple. Input services, called producers/publishers, create messages, and publish them to a message queue. Other services or servers, called consumers/subscribers, connect to the queue, and perform actions defined by the messages

  <img width="729" height="113" alt="image" src="https://github.com/user-attachments/assets/bb940b48-8fc6-496d-b3a1-f81ef9bf07c8" />

- Decoupling makes the message queue a preferred architecture for building a scalable and reliable application. With the message queue, the producer can post a message to the queue when the consumer is unavailable to process it. The consumer can read messages from the queue even when the producer is unavailable.
- Consider the following use case: your application supports photo customization, including cropping, sharpening, blurring, etc. Those customization tasks take time to complete. Web servers publish photo processing jobs to the message queue. Photo processing workers pick up jobs from the message queue and asynchronously perform photo customization tasks. The producer and the consumer can be scaled independently. When the size of the queue becomes large, more workers are added to reduce the processing time. However, if the queue is empty most of the time, the number of workers can be reduced.

  <img width="710" height="157" alt="image" src="https://github.com/user-attachments/assets/3049e677-eaab-4d65-a41d-ba3a5dcff470" />

### Logging, metrics, and automation:
- When working with a small website that runs on a few servers, logging, metrics, and automation support are good practices but not a necessity. However, now that your site has grown to serve a large business, investing in those tools is essential.
- Logging: Monitoring error logs is important because it helps to identify errors and problems in the system. You can monitor error logs at per server level or use tools to aggregate them to a centralized service for easy search and viewing.
- Metrics: Collecting different types of metrics help us to gain business insights and understand the health status of the system. Some of the following metrics are useful:

  1. Host level metrics: CPU, Memory, disk I/O, etc.
  2. Aggregated level metrics: for example, the performance of the entire database tier, cache tier, etc.
  3. Key business metrics: daily active users, retention, revenue, etc.

- Automation: When a system gets big and complex, we need to build or leverage automation tools to improve productivity. Continuous integration is a good practice, in which each code check-in is verified through automation, allowing teams to detect problems early. Besides, automating your build, test, deploy process, etc. could improve developer productivity significantly.
- Adding message queues and different tools:

  <img width="513" height="574" alt="image" src="https://github.com/user-attachments/assets/efdcba0b-a747-405d-afb8-e843ee47227e" />

  1. The design includes a message queue, which helps to make the system more loosely coupled and failure resilient.
  2. Logging, monitoring, metrics, and automation tools are included.

- As the data grows every day, your database gets more overloaded. It is time to scale the data tier.

### Database scaling:
- There are two broad approaches for database scaling: vertical scaling and horizontal scaling.

#### Vertical scaling
- Vertical scaling, also known as scaling up, is the scaling by adding more power (CPU, RAM, DISK, etc.) to an existing machine. There are some powerful database servers. According to Amazon Relational Database Service (RDS) [12], you can get a database server with 24 TB of RAM. This kind of powerful database server could store and handle lots of data. For example, stackoverflow.com in 2013 had over 10 million monthly unique visitors, but it only had 1 master database [13]. However, vertical scaling comes with some serious drawbacks:

  1. You can add more CPU, RAM, etc. to your database server, but there are hardware limits. If you have a large user base, a single server is not enough.
  2. Greater risk of single point of failures.
  3. The overall cost of vertical scaling is high. Powerful servers are much more expensive.

#### Horizontal scaling
- Horizontal scaling, also known as sharding, is the practice of adding more servers.

  <img width="701" height="479" alt="image" src="https://github.com/user-attachments/assets/149c53c3-5084-4745-8533-274aeafeef3e" />

- Sharding separates large databases into smaller, more easily managed parts called shards. Each shard shares the same schema, though the actual data on each shard is unique to the shard.
- The below image shows an example of sharded databases. User data is allocated to a database server based on user IDs. Anytime you access data, a hash function is used to find the corresponding shard. In our example, user_id % 4 is used as the hash function. If the result equals to 0, shard 0 is used to store and fetch data. If the result equals to 1, shard 1 is used. The same logic applies to other shards.

  <img width="471" height="307" alt="image" src="https://github.com/user-attachments/assets/d3d74262-67a9-4a8c-a92a-4403a2043a03" />
 <img width="656" height="685" alt="image" src="https://github.com/user-attachments/assets/c182e7f3-4787-4a59-a092-807bd11061c8" />

- The most important factor to consider when implementing a sharding strategy is the choice of the sharding key. Sharding key (known as a partition key) consists of one or more columns that determine how data is distributed. As shown in Figure 22, “user_id” is the sharding key. A sharding key allows you to retrieve and modify data efficiently by routing database queries to the correct database. When choosing a sharding key, one of the most important criteria is to choose a key that can evenly distributed data.
- Sharding is a great technique to scale the database but it is far from a perfect solution. It introduces complexities and new challenges to the system:

  1. Resharding data: Resharding data is needed when 1) a single shard could no longer hold more data due to rapid growth. 2) Certain shards might experience shard exhaustion faster than others due to uneven data distribution. When shard exhaustion happens, it requires updating the sharding function and moving data around. Consistent hashing is a commonly used technique to solve this problem.
  2. Celebrity problem: This is also called a hotspot key problem. Excessive access to a specific shard could cause server overload. Imagine data for Katy Perry, Justin Bieber, and Lady Gaga all end up on the same shard. For social applications, that shard will be overwhelmed with read operations. To solve this problem, we may need to allocate a shard for each celebrity. Each shard might even require further partition.
  3. Join and de-normalization: Once a database has been sharded across multiple servers, it is hard to perform join operations across database shards. A common workaround is to de-normalize the database so that queries can be performed in a single table.
- In below imgae, we shard databases to support rapidly increasing data traffic. At the same time, some of the non-relational functionalities are moved to a NoSQL data store to reduce the database load.

  <img width="719" height="726" alt="image" src="https://github.com/user-attachments/assets/11041b3e-0840-48e7-99c2-98ce067ad20f" />

### Some important terms:

- HTTP (Hypertext Transfer Protocol) is the foundation of data communication on the web. It's a request-response protocol where clients (like browsers) send requests to servers, which respond with data. Each request is independent and stateless - the server doesn't remember previous interactions
- HTTPS is HTTP with encryption via TLS/SSL. It ensures that data transmitted between client and server is encrypted and secure, protecting against eavesdropping and tampering. The "S" stands for "Secure" and is essential for any sensitive data like passwords or financial information.
- REST APIs (Representational State Transfer) are a design pattern for building web services using HTTP. They treat everything as a "resource" (users, products, posts) accessed via URLs. REST uses standard HTTP methods: GET (retrieve), POST (create), PUT/PATCH (update), DELETE (remove). A REST API might have endpoints like GET /users/123 to fetch user 123 or POST /users to create a new user. The key principles are statelessness and using HTTP's existing features rather than reinventing them.
- WebSockets solve a different problem than HTTP. While HTTP requires the client to initiate every exchange, WebSockets establish a persistent, two-way connection. After an initial HTTP handshake, the connection "upgrades" to WebSocket, allowing the server to push data to clients in real-time. This is perfect for chat applications, live dashboards, or multiplayer games where you need instant updates without constantly polling the server.
- DNS (Domain Name System) is the internet's phone book. When you type "example.com" into your browser, DNS translates that human-readable name into an IP address like 93.184.216.34 that computers use to locate servers. DNS works hierarchically: your request goes to a recursive resolver, which queries root servers, then top-level domain servers (.com), then the authoritative nameserver for that specific domain. DNS responses are cached at multiple levels to speed up subsequent requests.
- Load Balancers distribute incoming traffic across multiple servers to prevent any single server from being overwhelmed. They provide redundancy (if one server fails, others handle the traffic) and scalability (add more servers as traffic grows).

  1. Layer 4 (L4) Load Balancers operate at the transport layer. They make routing decisions based on IP addresses and TCP/UDP ports without inspecting the actual content of packets. They're fast and efficient because they don't need to parse application data. An L4 balancer sees a connection to port 443 and routes it to an available backend server based on simple algorithms (round-robin, least connections, etc.).
  2. Layer 7 (L7) Load Balancers operate at the application layer and understand HTTP/HTTPS. They can inspect URLs, headers, cookies, and request content to make intelligent routing decisions. An L7 balancer can send /api/* requests to API servers and /images/* to image servers, or route users to different server pools based on cookies for A/B testing. They're more powerful but require more processing since they must parse application protocols.
- Latency is the time it takes for data to travel from source to destination. It's measured in milliseconds and represents delay. Think of it as the time for a single ping to complete a round trip. Latency is affected by physical distance (speed of light limitations), routing complexity, and processing time at each hop. Low latency matters for interactive applications - video calls, online gaming, trading systems - where delays are perceptible and problematic.
- Bandwidth is the maximum capacity of a network connection, measured in bits per second (Mbps, Gbps). It's like the width of a pipe - how much data can potentially flow through simultaneously. A 1 Gbps connection has more bandwidth than a 100 Mbps connection. Bandwidth is your theoretical maximum throughput.
- Throughput is the actual amount of data successfully transferred over time. It's measured the same way as bandwidth (bits per second) but represents real performance rather than capacity. Throughput is always less than or equal to bandwidth because of overhead (protocol headers, retransmissions, congestion). A 1 Gbps connection might only achieve 800 Mbps throughput in practice.

  - The classic analogy: latency is how long it takes a truck to travel from warehouse to store, bandwidth is how many trucks can drive on the highway simultaneously, and throughput is how many trucks actually arrive with goods per hour. You can have high bandwidth but poor throughput (congested highway where trucks move slowly), or low latency but low bandwidth (fast sports car that carries little cargo).

- Basic 3 tier architecture diagram:

  <img width="390" height="399" alt="image" src="https://github.com/user-attachments/assets/1a3e7814-bca8-4c7d-9935-384984917bed" />

# CAP theorem and trade-offs:
- The core idea: in any distributed system, when the network breaks (and it always eventually does), you face a forced choice. You can't have all three:

  1. Consistency (C) — every read gets the most recent write, or an error. All nodes see the same data at the same time.
  2. Availability (A) — every request gets a response (not an error), even if that data might be stale.
  3. Partition Tolerance (P) — the system keeps working even when network packets get dropped between nodes.
- Here's the critical insight: P is not optional. Networks fail in production. So the real choice is always: when a partition happens, do you sacrifice C or A?
- Create a comparison table: CP vs AP systems:

  <img width="738" height="638" alt="image" src="https://github.com/user-attachments/assets/53737666-5de5-4454-8b26-1072c5611ca6" />

- The 3 mental models worth internalizing:

  1. "Can I live with stale data?" — This is the real question. If the answer is yes, go AP. If wrong data causes money loss, legal issues, or security violations, go CP.
  2. Eventual consistency is a contract, not a bug. AP systems promise "you'll get the right answer soon." DNS is the classic example — after you update a domain, propagation takes time, but every node will eventually agree. The system is designed that way.
  3. CAP is about extreme edge cases. On a happy network, CP systems are just as fast as AP. The difference only surfaces during partitions. This is why most people only discover they made the wrong tradeoff during an outage. 
- The nuance Kleppmann adds: Real systems blur these lines. Cassandra (typically AP) lets you tune per-query with consistency levels like QUORUM. DynamoDB has strongly consistent reads as an option. The pure CP/AP framing is a mental model, not a strict binary.

# Back-of-the-envelope estimation:
- Modern systems consist of many interconnected computational components. Although architectures vary, such as monolithic and microservice architectures, accounting for every node type, load balancers, caches, and databases is impractical in a design interview. Back-of-the-envelope calculations (BOTECs) help estimate capacity and resource requirements without modeling every infrastructure component.
- Back-of-the-envelope calculations are estimates you create using a combination of thought experiments and common performance numbers to get a good feel for which designs will meet your requirements.
- Common estimations include:

  - Concurrent TCP connections a server can support.
  - Requests per second (RPS) a web, database, or cache server can handle.
  - Storage requirements for a service
- We use BOTECs to abstract hardware specifics, latencies, and throughput rates. We will first examine server types and latencies to understand the system’s reality. Then, we will simplify these details to estimate RPS, bandwidth, and storage capacity.

## Types of data center servers:
- Data centers use commodity hardware to scale cost-effectively. Below, we discuss common server types used to handle different workloads:

  <img width="1224" height="530" alt="image" src="https://github.com/user-attachments/assets/cdf820b5-f2da-431e-a931-f51e112584bf" />

### Web server:
- Web servers are the first point of contact after load balancers and typically handle API calls. They are decoupled from application servers for scalability. While memory and storage requirements are often moderate, web servers need strong processing power. For example, Facebook has used web servers with 32 GB RAM and 500 GB storage.

### Application server:
- Application servers execute business logic and generate dynamic content. They often require significant computational and storage resources. Facebook has deployed application servers with 256 GB RAM and 6.5 TB of hybrid storage (flash and rotating disk).

### Storage server:
- As data grows, services use specialized storage units. YouTube, for example, uses:

  1. Blob storage: For encoded videos
  2. Temporary processing queue storage: Holds daily video uploads pending processing
  3. Bigtable: Specialized storage for video thumbnails
  4. RDBMS: For metadata (comments, likes, user channels)
- Other systems, like Hadoop’s HDFS, are used for analytics. Storage servers manage both structured (SQL) and unstructured (NoSQL) data.
- Returning to the example of Facebook, they’ve used servers with a storage capacity of up to 120 TB. With the number of servers in use, Facebook can store exabytes of data. (Note: One exabyte is 10^18 bytes. Storage and bandwidth are conventionally measured in base 10.) However, the RAM of these storage servers is often only 32 GB.
- Note: Data centers also require servers for configuration, monitoring, load balancing, analytics, accounting, and caching
- We need a reference point to ground our calculations. In the table below, we depict the capabilities of a typical server that can be used in the data centers of today:

  <img width="730" height="564" alt="image" src="https://github.com/user-attachments/assets/c4439a08-9697-420c-95ce-2dc7177f9838" />

### Standard number to remember:
- Effective planning requires understanding the workloads machines can handle. Latency is a key factor in resource estimation. The table below outlines important numbers for system designers.

 <img width="626" height="713" alt="image" src="https://github.com/user-attachments/assets/c2970b72-7318-4c7a-b14e-db2fe5c041f6" />
 <img width="625" height="242" alt="image" src="https://github.com/user-attachments/assets/8e806814-dd20-4f13-b5d0-2c9edbc5de5d" />

- Focus on the order of magnitude difference between components rather than exact numbers. For example, IO-bound work (e.g., reading 1 MB sequentially from SSD) is roughly two orders of magnitude slower than CPU-bound work (e.g., compressing 1 KB of data).
- Compression time is relatively consistent because the data usually fits within the processor’s L1, L2, or L3 caches. For instance, a typical server has an L3 cache of around 45 MB. Data fitting within this limit avoids the latency of fetching from slower memory or storage.
- In addition to latency, throughput is measured as the number of queries per second (QPS) that a single server can handle 

  <img width="570" height="263" alt="image" src="https://github.com/user-attachments/assets/a88fb87b-7ff9-4799-8ae4-1d79c17003fa" />
- The numbers above are approximations. Real performance varies based on query type (e.g., point query vs. range query), machine specifications, database design, indexing, and server load.

### Request types:
- While we often estimate generic “requests,” real workloads fall into three categories: CPU-bound, memory-bound, and IO-bound.

  1. CPU-bound requests: Limited by processing speed. Example: Compressing 1 KB of data. In our table, this takes 3 microseconds.
  2. Memory-bound requests: Limited by the memory subsystem. For example, reading 1 MB from RAM sequentially. In our table, this takes 9 microseconds (3x slower than CPU-bound).
  3. IO-bound requests: Limited by the IO subsystem (disk or network). For example, reading 1 MB sequentially from disk. In our table, this takes 200 microseconds (~66x slower than CPU-bound).
- To simplify calculations, we often approximate these differences as orders of magnitude: if a CPU task takes X time, a memory task takes 10X, and an IO task takes 100X.

### Abstracting away real system complexities:
- We have seen that real systems are complex. Considering every variable during a time-limited interview is impractical.
- BOTECs are valuable for making high-level estimates and decisions early in the design process. Moving forward, we will focus on performing these calculations efficiently.

### Request estimation in system design
- We can estimate the number of requests a typical server can handle by calculating the CPU time required per request. A real request touches many nodes, but we will accumulate the work for estimation

  <img width="1215" height="611" alt="image" src="https://github.com/user-attachments/assets/d1711113-171b-4caa-bd70-74aa3d563303" />
  <img width="1161" height="631" alt="image" src="https://github.com/user-attachments/assets/95a0cb90-081f-42cc-93bc-1619b287a043" />

- Changing assumptions, such as the number of instructions per request, will change the final estimate. Without precise measurements, these approximations are typically sufficient for high-level capacity planning. This approach avoids modeling detailed CPU, memory, and I/O constraints. This level of abstraction is central to BOTECs.

### Availability number:
- High availability is the ability of a system to be continuously operational for a desirably long period of time. High availability is measured as a percentage, with 100% means a service that has 0 downtime. Most services fall between 99% and 100%.
- A service level agreement (SLA) is a commonly used term for service providers. This is an agreement between you (the service provider) and your customer, and this agreement formally defines the level of uptime your service will deliver. Cloud providers Amazon [4], Google [5] and Microsoft [6] set their SLAs at 99.9% or above. Uptime is traditionally measured in nines. The more the nines, the better. As shown in Table 3, the number of nines correlate to the expected system downtime.

  <img width="798" height="230" alt="image" src="https://github.com/user-attachments/assets/219f76bf-2bcd-4e38-b473-35dc87900cf4" />

### Tips:
- Back-of-the-envelope estimation is all about the process. Solving the problem is more important than obtaining results. Interviewers may test your problem-solving skills. Here are a few tips to follow:
  1. Rounding and Approximation. It is difficult to perform complicated math operations during the interview. For example, what is the result of “99987 / 9.1”? There is no need to spend valuable time to solve complicated math problems. Precision is not expected. Use round numbers and approximation to your advantage. The division question can be simplified as follows: “100,000 / 10”.
  2. Write down your assumptions. It is a good idea to write down your assumptions to be referenced later.
  3. Label your units. When you write down “5”, does it mean 5 KB or 5 MB? You might confuse yourself with this. Write down the units because “5 MB” helps to remove ambiguity.
  4. Commonly asked back-of-the-envelope estimations: QPS, peak QPS, storage, cache, number of servers, etc. You can practice these calculations when preparing for an interview. Practice makes perfect.


## Power of two:
- Although data volume can become enormous when dealing with distributed systems, calculation all boils down to the basics. To obtain correct calculations, it is critical to know the data volume unit using the power of 2. A byte is a sequence of 8 bits. An ASCII character uses one byte of memory (8 bits). Below is a table explaining the data volume unit.

  <img width="501" height="270" alt="image" src="https://github.com/user-attachments/assets/206dcdbc-40c3-45cb-b89b-4b5d93308961" />

 
