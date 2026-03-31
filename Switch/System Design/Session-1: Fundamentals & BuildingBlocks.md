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

### Cheat sheet for BOTEE:
#### Quick reference: The 5 core calculation
- Every system design interview needs these 5 calcualtions:
  1. Traffic (QPS) - How many requests per second?
     -  Daily active users to QPS:
       ```text
       Average QPS = (DAU * Actions per user per day) % 86400 seconds
       Peak QPS = Average QPS * Peak factor (2-3x)
       ```
     -  Read vs. write QPS:
     ```text
     Read QPS = Total QPS * Read ratio
     Write QPS = Total QPS * write ratio

     Common ratios:
     - Social media: 100: 1 (read:write)
     - Chat apps: 1:1 (read: write)
     - Analytics: 1000:1 (read:write)
     - Content creation: 10:1 (read:write)
     ```
  2. Storage - How much data storage needed?
     - Basic storage forumulas:
     ```text
     Total storage = Records per day * record size (bytes) * Days to store * Replication factor (usually 3)
     ```
     - With growth rate:
     ```text
     Storage after N years = Daily storage * 365 * N years * (1+ annual growth rate)^N
     ```
     - Media storage:
     ```text
     Total Media storage = Media uploads per day * Average file size * Days to store * Replication factor * (1+compression savings) // typically 0.7 = 30% savings
     ```
     
  3. Bandwidth - How much network capacity needed?
     - Incoming bandwidth (writes):
     ```text
     Write Bandwidth (MB/s) = Write QPS × Average Request Size (KB) ÷ 1,024
     ```
     - Outgoing bandwidth (reads):
     ```text
     Read Bandwidth (MB/s) = Read QPS × Average Response Size (KB) ÷ 1,024
     ```
     - Total Bandwidth:
     ```text
     Total Bandwidth = Write Bandwidth + Read Bandwidth
     ```
     - Peak Bandwidth:
     ```text
     Peak Bandwidth = Total Bandwidth × Peak Factor (2-3x)
     ```

  4. Memory (Cache) - How much RAM for caching?
    - 80-20 Rule (Pareto Principle):
    ```text
    Cache Size = Daily Requests × 20% (hot data)
                    × Average Response Size
                      × Cache Overhead (1.2x)
    ```
    - Memory for Connections:
    ```text
    Connection Memory =  Max Concurrent Connections 
                         × Memory per Connection (1-5 MB)
    ```
    - Cache Hit Ratio Impact:
    ```text
    Effective DB QPS = Total QPS × (1 - Cache Hit Ratio)
   Example:
   If Cache Hit = 80%, DB only sees 20% of traffic
   100K QPS with 80% hit → 20K QPS to database
    ``` 

  5. Servers - How many servers needed?
     - Application Servers:
     ```text
     Number of Servers = Peak QPS ÷ QPS per Server× Safety Factor (1.5-2x for redundancy)
     Typical QPS per server: 1,000 - 10,000 (depends on complexity)
     ```
     - Database Servers:
     ```text
     Read Replicas = 
     Read QPS ÷ QPS per DB Server (typically 1,000-5,000)+ 1 (for failover)
     Master DB = 1 (or 2 for master-master replication)
     ```
     - Cache Servers:
     ```text
     Cache Instances = Total Cache Size ÷ Memory per Instance × Replication Factor (2-3x)
     Typical cache server: 16-64 GB RAM
     ```
#### Critical numbers to remember
-  Time Conversions
   ```text
   1 day = 86,400 seconds (~100K for quick math)
   1 month = 2.6M seconds (30 days)
   1 year = 31.5M seconds
   ```
-  Data Sizes (Powers of 2)
   ```text
   1 KB = 1,024 bytes ≈ 10³ = 1,000 bytes
   1 MB = 1,024 KB ≈ 10⁶ = 1 million bytes
   1 GB = 1,024 MB ≈ 10⁹ = 1 billion bytes
   1 TB = 1,024 GB ≈ 10¹² = 1 trillion bytes
   1 PB = 1,024 TB ≈ 10¹⁵ = 1 quadrillion bytes
   ```
-  Common Record Sizes
   ```text
   User ID (int64): 8 bytes
   Timestamp: 8 bytes
   UUID: 16 bytes
   Short text (tweet): 300 bytes
   User record: 1-2 KB
   Photo metadata: 1 KB
   Small image: 200 KB
   Large image: 2-5 MB
   1 min video (HD): 50-100 MB
   ```
-  Network & Latency
   ```text
   L1 cache: 0.5 ns
   L2 cache: 7 ns
   RAM access: 100 ns
   SSD read: 150 μs
   Network within datacenter: <1 ms
   HDD seek: 10 ms
   Network across continents: 100-150 ms
   ```
-  Throughput Benchmarks
   ```text
   Redis: 100K ops/sec per instance
   Memcached: 100K ops/sec per instance
   MySQL: 1K-10K QPS per server
   MongoDB: 10K-50K QPS per server
   Cassandra: 10K+ writes/sec per node
   Kafka: 1M+ messages/sec per cluster
   Nginx/HAProxy: 50K requests/sec per instance
   ```
-  Scale Reference
   ```text
   Small: <100 QPS, <10K DAU
   Medium: 100-1K QPS, 10K-1M DAU
   Large: 1K-10K QPS, 1M-10M DAU
   Very Large: 10K-100K QPS, 10M-100M DAU
    Massive: >100K QPS, >100M DAU
   ```
#### Step-by-Step Estimation Template
- Step 1: Clarify Requirements (2 min)
```
Ask:
- How many users? (DAU, MAU)
- What's the read/write ratio?
- What features are critical?
- What's the scale? (current + 5 years)
- What are the latency requirements?
```
 
-  Step 2: Make Assumptions (1 min)
```
State clearly:
- "Assuming 100M DAU"
- "Assuming 80% read, 20% write"
- "Assuming 5 actions per user per day"
- "Assuming peak traffic is 3x average"
```
 
- Step 3: Calculate Traffic (2 min)
```
1. Calculate daily requests
   = DAU × Actions per User
 
2. Calculate average QPS
   = Daily Requests ÷ 86,400
 
3. Calculate peak QPS
   = Average QPS × 3
 
4. Split into read/write
   Read QPS = Total × 0.8
   Write QPS = Total × 0.2
```
 
-  Step 4: Calculate Storage (2 min)
```
1. Estimate data per record
   User: 2 KB
   Post: 1 KB
   Image: 300 KB
 
2. Calculate daily new data
   = Records per Day × Size per Record
 
3. Calculate 5-year storage
   = Daily Data × 365 × 5
 
4. Add replication
   = Total × 3
```
 
- Step 5: Calculate Bandwidth (1 min)
```
1. Write bandwidth
   = Write QPS × Request Size
 
2. Read bandwidth
   = Read QPS × Response Size
 
3. Total bandwidth
   = Write + Read
```
 
- Step 6: Calculate Memory/Cache (1 min)
```
1. Apply 80-20 rule
   Cache = Daily Data × 0.20
 
2. Add overhead
   = Cache Size × 1.2
```
 
- Step 7: Calculate Servers (1 min)
```
1. App servers
   = Peak QPS ÷ 10K × 2
 
2. DB servers
   = Read QPS ÷ 5K + Write Replicas
 
3. Cache servers
   = Cache Size ÷ 64GB × 2
```
 


## Power of two:
- Although data volume can become enormous when dealing with distributed systems, calculation all boils down to the basics. To obtain correct calculations, it is critical to know the data volume unit using the power of 2. A byte is a sequence of 8 bits. An ASCII character uses one byte of memory (8 bits). Below is a table explaining the data volume unit.

  <img width="501" height="270" alt="image" src="https://github.com/user-attachments/assets/206dcdbc-40c3-45cb-b89b-4b5d93308961" />

# API Design basics:

## Restful principles
- HTTP Methods Mapping

  <img width="688" height="263" alt="image" src="https://github.com/user-attachments/assets/ba62c6e7-416f-4fb2-97f7-50a8c35bf1c0" />

- HTTP Status Codes:

  <img width="685" height="396" alt="image" src="https://github.com/user-attachments/assets/19477985-a17c-4cb4-8726-634751dacf5e" />

- Stateless Communication:

  1. Server doesn't store client state
  2. Each request contains all needed info (auth token, params)
  3. Session data stored client-side or in distributed cache (Redis)

## Idempotency
- An operation is idempotent if calling it multiple times produces the same result as calling it once.
- Why It Matters

  1. Network failures: Client doesn't know if request succeeded
  2. Retry safety: Safe to retry without side effects
  3. Distributed systems: Prevents duplicate operations
- Idempotency by HTTP Method:

  1. GET, PUT, DELETE are idempotent:
     ```
     GET /users/123        → Always returns same user (or 404)
     PUT /users/123 {...}  → Set user to exact state, multiple calls = same state
     DELETE /users/123     → Delete user, calling again still means user deleted 
     ```
  2. POST is NOT idempotent:
     ```
     POST /users           → Creates new user each time
     POST /orders          → Creates new order each time
     ``` 
  3. Making POST Idempotent (Interview Gold!)
 
     1. solution-1: Idempotency keys
     ```
      POST /orders
     Headers:
       Idempotency-Key: uuid-12345
     Body:
        { "productId": 789, "quantity": 2 }

     Server logic:
       1. Check if idempotency key exists in cache/DB
       2. If exists → return cached response (201 or 200)
       3. If new → process request, store key + response
       4. Return response
     ```
       - Implementation
       ```java
       @PostMapping("/orders")
      public ResponseEntity<Order> createOrder(
         @RequestHeader("Idempotency-Key") String idempotencyKey,
           @RequestBody OrderRequest request) {
    
        // Check cache (Redis)
        Order existingOrder = redisTemplate.opsForValue()
        .get("idempotency:" + idempotencyKey);
    
        if (existingOrder != null) {
           return ResponseEntity.ok(existingOrder); // Already processed
         }
    
        // Process new order
        Order order = orderService.create(request);
    
       // Store with TTL (24 hours)
        redisTemplate.opsForValue()
         .set("idempotency:" + idempotencyKey, order, 24, TimeUnit.HOURS);
    
        return ResponseEntity.status(201).body(order);  
       }
       ``` 
     2. Solution-2: Natual idempotency
        ```
        PUT /users/123/preferences
        Body: { "theme": "dark", "language": "en" }

        → Setting same preference multiple times = same result
        → No idempotency key needed
        ```   
- Interview Tip: Always mention idempotency when designing payment/order APIs!

## Pagination
- Why Pagination?

  1. Performance: Can't return 1M records in one response
  2. Bandwidth: Reduce data transfer
  3. User experience: Load data progressively
- Pagination Strategies

  1.  Offset-Based (Simple but has issues)
      - Request:
        ```
        GET /posts?page=2&size=20
        or
        GET /posts?offset=20&limit=20
        ```
      - Response
        ```
        {
            "data": [...],
            "pagination": {
                "page": 2,
                "size": 20,
                "totalPages": 50,
                "totalElements": 1000
             }
        }
        ```
      - SQL Implementation:
        ```
        SELECT * FROM posts 
         ORDER BY created_at DESC 
        LIMIT 20 OFFSET 40;  -- page 2, skip first 40
        ```
      - Pros:

        1. Simple to implement
        2. Easy to jump to any page
      - Cons:

        1. Performance degrades with large offsets (OFFSET 1000000 is slow)
        2. Inconsistent results if data changes between requests (insert/delete)
        3. Example: User on page 2, new post added → sees duplicate
           
  2.  Cursor-Based (Recommended for feeds)
      - Request:
        ```
        GET /posts?cursor=eyJpZCI6MTIzfQ==&limit=20

        First request: GET /posts?limit=20
        ```
      - Response:
        ```
        {
           "data": [
              { "id": 123, "content": "..." },
              { "id": 122, "content": "..." },
                ...
            ],
         "pagination": {
            "nextCursor": "eyJpZCI6MTAzfQ==",
          "hasMore": true
           }
        }
        ```
      - Implementation (Spring boot)
        ```
        @GetMapping("/posts")
         public ResponseEntity<FeedResponse> getFeed(
               @RequestParam(required = false) String cursor,
             @RequestParam(defaultValue = "20") int limit) {
    
          Long lastId = cursor != null ? decodeCursor(cursor) : Long.MAX_VALUE;
    
          // Query: Get posts with id < lastId
            List<Post> posts = postRepository.findByIdLessThanOrderByIdDesc(lastId, 
               PageRequest.of(0, limit));
    
                 String nextCursor = posts.isEmpty() ? null : 
                encodeCursor(posts.get(posts.size() - 1).getId());
    
            return ResponseEntity.ok(new FeedResponse(posts, nextCursor));
             }
        ```
      - SQL:
        ```
        -- Cursor = last seen ID (e.g., 123)
          SELECT * FROM posts 
          WHERE id < 123 
          ORDER BY id DESC 
          LIMIT 20;
        ```
      - Pros:
 
        1. Consistent results (no duplicates/skips)
        2. Performant (uses indexed WHERE clause, not OFFSET)
        3. Works with infinite scroll
      - Cons:
 
        1. Can't jump to specific page
        2. Cursor needs to be encoded/decoded
      - When to Use What:
 
        1. Offset: Admin panels, dashboards (need page numbers)
        2. Cursor: Social feeds, chat history, real-time data
        
  3.  Keyset Pagination (Timestamp-based)
      - Request:
        ```
        GET /posts?since=2024-03-15T10:30:00Z&limit=20
        ```
      - SQL:
        ```
        SELECT * FROM posts 
        WHERE created_at > '2024-03-15 10:30:00' 
        ORDER BY created_at ASC 
        LIMIT 20;
        ```
      - Used for "load more since last check" scenarios.

## Api Versioning
- Why Version APIs?

  1. Breaking changes: Can't break existing clients
  2. Evolution: Add features without disrupting users
  3. Multiple clients: Old mobile apps still in use
- Versioning Strategies

  1. URI Versioning (Most Common)
     - request:
       ```
       GET /v1/users/123
       GET /v2/users/123
       ```
     - Pros: Clear, easy to route
     - Cons: URL pollution
     - Used by: Twitter, Stripe, Facebook
  2.  Header Versioning
      - header:
        ```
        GET /users/123
        Headers:
            Accept: application/vnd.myapp.v1+json
        ```
      - Pros: Clean URLs
      - Cons: Less visible, harder to test
      - Used by: GitHub API
  3.  Query Parameter
      - request:
        ```
        GET /users/123?version=1
        ```
      - Pros: Simple
      - Cons: Not RESTful, easy to forget
      - Avoid in interviews unless asked
  4.  Content Negotiation
      - request:
        ```
        GET /users/123
        Headers:
           Accept: application/vnd.myapp+json; version=1
        ```
- When to Use: URI versioning (v1, v2) is the industry standard. Mention it in interviews.
- Best Practice:

  1. Version only when breaking changes occur
  2. Maintain backward compatibility when possible
  3. Deprecate old versions gradually (e.g., v1 deprecated → v2 → v1 removed after 6 months)

## Rate limiting:
- Purpose:

  1. Prevent abuse: Stop DOS attacks
  2. Fair usage: Ensure all users get service
  3. Cost control: Limit expensive operations
  4. API monetization: Free tier (100 req/hr), Paid tier (10,000 req/hr)
- Common Algorithms:

  1. Token Bucket (Most Popular)
     - Concept:
        
       1. Bucket has capacity (e.g., 100 tokens)
       2. Refills at constant rate (e.g., 10 tokens/second)
       3. Each request consumes 1 token
       4. If no tokens → reject request (429) 
     - Example:
       ```
       Bucket capacity: 100 tokens
       Refill rate: 10 tokens/sec
        Time 0s:  100 tokens available
       Request:  99 tokens left
       Time 1s:  109 tokens (99 + 10 refill, capped at 100)
        Burst of 50 requests: 50 tokens left
        Time 5s:  100 tokens (50 + 50 refill)
       ```
     - Pros: Allows bursts, smooth rate limiting
     - Used by: AWS, Stripe

  2. Fixed Window Counter
     - Concept:
 
       1. Window: 1 minute intervals (00:00-00:59, 01:00-01:59)
       2. Limit: 100 requests per window
       3. Counter resets at window boundary
     - Problem (Boundary Issue):
     ```
     12:00:30 - 12:00:59 → 100 requests
     12:01:00 - 12:01:29 → 100 requests
     Total in 60 sec: 200 requests (double the limit!)
     ```
  3. Sliding Window Log
     - Concepts:
 
       1. Store timestamp of each request
       2. Count requests in last 60 seconds
       3. Remove old timestamps
     - Accurate but memory-intensive
    
  4. Sliding Window Counter (Best Balance)
     - Combines fixed window + sliding window for accuracy with low memory.

- HTTP Headers for Rate Limiting

  1. Response headers:
  ```
  HTTP/1.1 200 OK
   X-RateLimit-Limit: 100          # Total allowed per hour
  X-RateLimit-Remaining: 23       # Requests left
  X-RateLimit-Reset: 1678901234   # Unix timestamp when limit resets

  When rate limit exceeded:
  HTTP/1.1 429 Too Many Requests
  Retry-After: 3600               # Seconds to wait
  ```
  
  2. Implementation (Spring Boot + Redis)
     - f
     ```java
       @Component
       public class RateLimiter {
    
          @Autowired
          private StringRedisTemplate redisTemplate;
    
          public boolean allowRequest(String userId, int limit, int windowSecs) {
                String key = "rate_limit:" + userId;
                Long count = redisTemplate.opsForValue().increment(key);
        
                if (count == 1) {
                    redisTemplate.expire(key, windowSecs, TimeUnit.SECONDS);
                }
        
               return count <= limit;
           }
         }

      @RestControllerAdvice
      public class RateLimitInterceptor {
    
           @Autowired
           private RateLimiter rateLimiter;
    
           @Before
          public void checkRateLimit(HttpServletRequest request) {
               String userId = getUserIdFromToken(request);
        
              if (!rateLimiter.allowRequest(userId, 100, 3600)) {
                 throw new RateLimitExceededException();
            }
         }
        }
     ```
     - explanation of above implementation:

        <img width="863" height="426" alt="image" src="https://github.com/user-attachments/assets/71161973-5f1c-49d0-8222-2b1ff85ef88b" />
        <img width="849" height="383" alt="image" src="https://github.com/user-attachments/assets/11b83e34-692c-45de-a2f3-31f7817662be" />
        <img width="839" height="520" alt="image" src="https://github.com/user-attachments/assets/f8131f8d-6b98-4126-87b4-af51bbbfa311" />
  
- Interview Answer: "For rate limiting, I'd use Token Bucket algorithm with Redis. Store user token count with TTL for window reset. Return 429 with Retry-After header when limit exceeded."

## Pratical Exercise: Design API:
### Task 1: Design "Create Post" API
- Requirements
   1. User can create a post with text/image
   2. Must be idempotent (prevent duplicate posts)
   3. Should handle large images
   4. Rate limited

 #### API Design
 
**Endpoint:**
```http
POST /v1/posts
```
 
**Request Headers:**
```http
Content-Type: application/json
Authorization: Bearer {jwt_token}
Idempotency-Key: {uuid}
```
 
**Request Body:**
```json
{
  "content": "This is my post content",
  "imageUrl": "https://cdn.example.com/images/abc123.jpg",
  "visibility": "public"  // public, friends, private
}
```
 
**Response (201 Created):**
```json
{
  "id": "post_789",
  "userId": "user_123",
  "content": "This is my post content",
  "imageUrl": "https://cdn.example.com/images/abc123.jpg",
  "visibility": "public",
  "createdAt": "2024-03-15T10:30:00Z",
  "updatedAt": "2024-03-15T10:30:00Z",
  "likesCount": 0,
  "commentsCount": 0
}
```
 
**Response (429 Too Many Requests):**
```json
{
  "error": "rate_limit_exceeded",
  "message": "You can create maximum 50 posts per hour",
  "retryAfter": 3600
}
```
 
**Design Decisions:**
 
1. **Idempotency**: Use `Idempotency-Key` header
   - Store key in Redis with 24hr TTL
   - Prevent duplicate posts if retry
 
2. **Image Handling**:
   - Don't upload image in POST body (too large)
   - Separate flow:
     ```
     Step 1: POST /v1/media/upload → Get presigned URL
     Step 2: Upload to S3 directly
     Step 3: POST /v1/posts with imageUrl
     ```
 
3. **Rate Limiting**:
   - 50 posts per hour per user
   - Token bucket algorithm
   - Return 429 with Retry-After
 
4. **Validation**:
   - Content max 5000 characters
   - Image URL must be from CDN domain
   - Visibility enum validation
 
---
 
### **Task 2: Design "Get Feed" API**
 
#### Requirements
- Show posts from followed users
- Should be fast (millions of users)
- Pagination for infinite scroll
- Real-time updates
 
#### API Design
 
**Endpoint:**
```http
GET /v1/feed
```
 
**Request:**
```http
GET /v1/feed?cursor={base64_encoded}&limit=20
Authorization: Bearer {jwt_token}
```
 
**Query Parameters:**
- `cursor` (optional): Pagination cursor (base64 encoded post ID + timestamp)
- `limit` (optional): Number of posts per page (default: 20, max: 50)
 
**Response (200 OK):**
```json
{
  "posts": [
    {
      "id": "post_789",
      "userId": "user_456",
      "userName": "John Doe",
      "userAvatar": "https://cdn.example.com/avatars/456.jpg",
      "content": "Amazing sunset today!",
      "imageUrl": "https://cdn.example.com/images/abc123.jpg",
      "createdAt": "2024-03-15T10:30:00Z",
      "likesCount": 42,
      "commentsCount": 5,
      "isLiked": false,
      "isSaved": false
    },
    // ... 19 more posts
  ],
  "pagination": {
    "nextCursor": "eyJpZCI6InBvc3RfNzg5IiwidCI6MTcxMDQ5ODYwMH0=",
    "hasMore": true
  }
}
```
 
**Design Decisions:**
 
1. **Pagination Strategy**: Cursor-based
   ```
   Cursor structure (base64 encoded):
   {
     "id": "post_789",
     "timestamp": 1710498600
   }
   
   SQL Query:
   SELECT * FROM posts 
   WHERE user_id IN (SELECT followed_id FROM follows WHERE follower_id = ?)
     AND (created_at, id) < (?, ?)  -- Cursor values
   ORDER BY created_at DESC, id DESC
   LIMIT 20;
   ```
 
2. **Performance Optimization**:
   - **Pre-computed feed**: Use fan-out approach
     - When user creates post → push to followers' feed cache (Redis)
     - GET /feed → read from Redis
     - Hybrid: Pre-compute for active users, on-demand for inactive
   
   - **Caching Strategy**:
     ```
     L1: Redis cache (feed:user_123) → TTL 15 min
     L2: Database query
     ```
 
3. **Real-time Updates**: 
   - WebSocket connection for new posts notification
   - Client polls with `GET /feed/updates?since={timestamp}`
 
4. **Data Optimization**:
   - Embed user info (name, avatar) to avoid N+1 queries
   - Return denormalized data (likesCount from cache)
   - Lazy load comments (separate API)

## Interview Answers (Ready-to-Use)
 
**Q: How would you make POST API idempotent?**
 
"I'd use an idempotency key approach. Client sends a unique UUID in the `Idempotency-Key` header. Server checks Redis cache:
- If key exists → return cached response (likely 200 or 201)
- If new → process request, cache the key-response pair with 24hr TTL, return response
 
This prevents duplicate operations if client retries due to network failure. Similar to how Stripe handles payment APIs."
 
**Q: Offset vs Cursor pagination?**
 
"Offset (page/size) is simple but has two issues:
1. Performance degrades with large offsets (OFFSET 1000000 is slow)
2. Inconsistent results if data changes (new insert → see duplicates)
 
Cursor-based uses last seen ID (e.g., WHERE id < 123). Benefits:
- Uses index (fast)
- Consistent results
- Perfect for infinite scroll
 
I'd use offset for admin panels (need page numbers), cursor for feeds (Instagram, Twitter style)."
 
**Q: How would you implement rate limiting?**
 
"Token Bucket algorithm with Redis:
1. Each user has a bucket (Redis key: `rate_limit:user_123`)
2. Bucket capacity = limit (e.g., 100 requests)
3. Refills at constant rate (e.g., 10/sec)
4. Each request: INCR key, check count, set EXPIRE if first request
5. If count > limit → return 429 with Retry-After header
 
Token Bucket allows bursts while maintaining average rate, which feels natural to users."

## Code Templates (Copy-Paste Ready)
 
### Spring Boot Idempotency Interceptor
```java
@Component
public class IdempotencyInterceptor implements HandlerInterceptor {
    
    @Autowired
    private StringRedisTemplate redis;
    
    @Override
    public boolean preHandle(HttpServletRequest request, 
                           HttpServletResponse response, 
                           Object handler) throws Exception {
        
        if (!"POST".equals(request.getMethod())) {
            return true;
        }
        
        String key = request.getHeader("Idempotency-Key");
        if (key == null) {
            throw new BadRequestException("Idempotency-Key required");
        }
        
        String cached = redis.opsForValue().get("idem:" + key);
        if (cached != null) {
            response.setStatus(200);
            response.getWriter().write(cached);
            return false; // Don't proceed to controller
        }
        
        request.setAttribute("idempotencyKey", key);
        return true;
    }
}
```
 
### Cursor Pagination Repository
```java
public interface PostRepository extends JpaRepository<Post, Long> {
    
    @Query("SELECT p FROM Post p WHERE p.id < :cursor " +
           "ORDER BY p.id DESC")
    List<Post> findByCursor(@Param("cursor") Long cursor, 
                           Pageable pageable);
    
    // First page (no cursor)
    @Query("SELECT p FROM Post p ORDER BY p.id DESC")
    List<Post> findFirstPage(Pageable pageable);
}
 
// Usage
Long cursor = request.getCursor() != null ? 
    decodeCursor(request.getCursor()) : null;
 
List<Post> posts = cursor != null ? 
    repository.findByCursor(cursor, PageRequest.of(0, 20)) :
    repository.findFirstPage(PageRequest.of(0, 20));
```

# Review question

## Question 1: Explain CAP Theorem in 2 Minutes to a Non-Technical Person
 
### The Explanation (Interview-Ready)
 
**"Imagine you and your friends are planning a trip together using a shared calendar app.**
 
**Three things you want:**
 
1. **Consistency (C)**: Everyone sees the same information at the same time
   - If you add "Beach trip - Saturday 2 PM" to the calendar
   - Your friend should immediately see "Beach trip - Saturday 2 PM"
   - Not old/different information
 
2. **Availability (A)**: The app always works when you need it
   - Even if your internet is slow or one server is down
   - You can always open the app and use it
   - No "Service Unavailable" errors
 
3. **Partition Tolerance (P)**: The app works even when servers can't talk to each other
   - Imagine servers in India and US can't communicate (network problem)
   - The app still functions in both locations
   - This is a real problem in distributed systems
 
**The CAP Theorem says: You can only pick 2 out of these 3.**
 
**Real-world examples:**
 
- **Banking App (Choose C + P, sacrifice A)**:
  - When you transfer money, consistency is critical (can't show different balances)
  - If there's a network issue, the app might say "Try again later" (not available)
  - Better to be temporarily unavailable than show wrong balance
 
- **Instagram/Facebook Feed (Choose A + P, sacrifice C)**:
  - Feed is always available (you can always scroll)
  - But sometimes you see old posts or duplicate posts (not perfectly consistent)
  - That's okay - seeing yesterday's photo a few seconds late doesn't break anything
 
- **Single Server App (Choose C + A, but no P)**:
  - If you only have one server, C + A is easy
  - But if that server goes down, everything stops
  - No distribution = no partition tolerance
 
**Key Insight**: In distributed systems (multiple servers), network partitions WILL happen, so P is mandatory. Your real choice is between C and A."
 
---
 
### **Follow-up: CA, CP, AP Examples**
 
| Type | Systems | Why? |
|------|---------|------|
| **CP** (Consistency + Partition Tolerance) | Banking systems, Financial transactions, MongoDB (default), HBase, Redis (single master) | Wrong data is unacceptable. Better to fail than show incorrect balance. |
| **AP** (Availability + Partition Tolerance) | Social media feeds (Instagram, Twitter), DNS, Cassandra, DynamoDB | User experience matters more. Stale data is tolerable. Always available > perfect data. |
| **CA** (Theoretical only) | Traditional RDBMS (single server), Local cache | Only works if no network partitions. Real distributed systems can't achieve this. |
 
---
 
## Question 2: Calculate Peak QPS for 100M DAU with 10 Requests/Day
 
### The Calculation (Show Your Work in Interviews)
 
**Given:**
- DAU (Daily Active Users) = 100M = 100,000,000
- Requests per user per day = 10
 
**Step 1: Total Requests per Day**
```
Total requests/day = DAU × Requests per user
                   = 100M × 10
                   = 1,000M requests/day
                   = 1 billion requests/day
```
 
**Step 2: Average QPS (Queries Per Second)**
```
Average QPS = Total requests per day / Seconds in a day
            = 1,000,000,000 / 86,400
            = ~11,574 QPS
            ≈ 11.5K QPS (round for simplicity)
```
 
**Step 3: Peak QPS (Industry Standard: 2-3x Average)**
```
Peak QPS = Average QPS × Peak multiplier
         = 11,574 × 2  (conservative)
         = 23,148 QPS
         ≈ 23K QPS
 
OR (aggressive estimate):
Peak QPS = 11,574 × 3
         = 34,722 QPS
         ≈ 35K QPS
```
 
### **Answer for Interview:**
 
**"Let me calculate this step by step:**
 
1. **Total daily requests**: 100M users × 10 requests = 1 billion requests/day
 
2. **Average QPS**: 1B / 86,400 seconds ≈ 11.5K QPS
 
3. **Peak QPS**: Traffic isn't evenly distributed. Most users are active during business hours (9 AM - 9 PM). Using a 2x peak multiplier: **~23K QPS**
 
   For a more aggressive estimate (all users in 8-hour window): **~35K QPS**
 
**System sizing**: To handle 35K QPS with 5ms latency per request, I'd provision servers with comfortable headroom (plan for 50K QPS to handle spikes)."
 
---
 
### **Useful Formulas to Memorize**
 
```
Average QPS = DAU × Requests per user / 86,400
 
Peak QPS = Average QPS × (2 to 5)
  - 2x: Normal apps (evenly distributed traffic)
  - 3x: E-commerce, social media
  - 5x+: News sites (event-driven spikes)
 
Seconds in a day = 24 × 60 × 60 = 86,400 ≈ 100,000 (for rough estimation)
```
 
---
 
## Question 3: Difference Between L4 and L7 Load Balancer
 
### **Short Answer (30 seconds)**
 
**L4 (Layer 4 - Transport Layer)**:
- Works at TCP/UDP level
- Routes based on IP address and port
- Doesn't understand HTTP content
- **Faster, simpler**
 
**L7 (Layer 7 - Application Layer)**:
- Works at HTTP level
- Routes based on URL, headers, cookies
- Can inspect request content
- **Smarter, but slower**
 
---
 
### **Detailed Comparison Table**
 
| Feature | L4 Load Balancer | L7 Load Balancer |
|---------|------------------|------------------|
| **OSI Layer** | Transport (Layer 4) | Application (Layer 7) |
| **Protocol** | TCP/UDP | HTTP/HTTPS/WebSocket |
| **Routing Based On** | IP address, Port | URL path, Headers, Cookies, Query params |
| **Understands** | Packets, connections | HTTP requests, content |
| **Performance** | Very fast (less processing) | Slower (must parse HTTP) |
| **SSL Termination** | No (pass-through) | Yes (can decrypt and inspect) |
| **Use Case** | Simple distribution, high throughput | Content-based routing, microservices |
| **Examples** | HAProxy (L4 mode), AWS NLB | Nginx, HAProxy (L7 mode), AWS ALB |
 
---
 
### **Practical Examples**
 
#### **L4 Use Case: Simple Round-Robin**
```
Client → L4 Load Balancer → Backend Servers
         (IP: 192.168.1.100, Port: 443)
         Routes to:
         - Server 1: 10.0.1.10:8080
         - Server 2: 10.0.1.11:8080
         - Server 3: 10.0.1.12:8080
 
Decision: Based on connection count, not content
```
 
**L4 sees:**
```
Source: 203.0.113.50:54321
Destination: 192.168.1.100:443
Protocol: TCP
 
→ Routes entire TCP connection to Server 1
```
 
#### **L7 Use Case: Content-Based Routing**
```
Client → L7 Load Balancer → Route by URL path
 
L7 sees:
  GET /api/users/123 → Route to User Service (10.0.2.10)
  GET /api/orders/456 → Route to Order Service (10.0.2.20)
  GET /images/logo.png → Route to CDN (10.0.2.30)
  
Can also route by:
  - Header: X-API-Version: 2 → Route to v2 servers
  - Cookie: region=EU → Route to EU servers
  - Query: ?mobile=true → Route to mobile API servers
```
 
**L7 sees:**
```
GET /api/users/123 HTTP/1.1
Host: api.example.com
Authorization: Bearer abc123
User-Agent: Mobile App
 
→ Inspects path, headers, and routes to User Service
```
 
---
 
### **When to Use Which?**
 
**Use L4 when:**
- ✅ Need maximum throughput (low latency critical)
- ✅ Simple round-robin/least-connections is enough
- ✅ Non-HTTP traffic (databases, game servers, DNS)
- ✅ Cost optimization (cheaper, less CPU)
- ✅ Example: Load balancing database replicas
 
**Use L7 when:**
- ✅ Microservices architecture (route /users to User Service)
- ✅ Need SSL termination at load balancer
- ✅ A/B testing (route 10% traffic to new version)
- ✅ Geographic routing (route EU users to EU servers)
- ✅ Rate limiting per API endpoint
- ✅ Web application firewall (WAF) integration
- ✅ Example: API Gateway for microservices
 
---
 
### **Interview Answer Template**
 
**"L4 operates at the transport layer and routes based on IP and port. It's fast but dumb - just distributes TCP connections without understanding content.**
 
**L7 operates at the application layer and understands HTTP. It can route based on URL paths, headers, or cookies. For example, it can send `/api/users` to User Service and `/api/orders` to Order Service.**
 
**I'd use L4 for database load balancing where throughput matters. I'd use L7 for microservices where I need intelligent routing and SSL termination.**
 
**In AWS terms: Network Load Balancer (NLB) is L4, Application Load Balancer (ALB) is L7."**
 
---
 
## Question 4: When Would You Choose AP Over CP System?
 
### **Short Answer**
 
Choose **AP (Availability + Partition Tolerance)** over **CP (Consistency + Partition Tolerance)** when:
- Stale data is acceptable
- User experience (uptime) matters more than perfect accuracy
- Eventually consistent is good enough
- Downtime costs more than showing old data
 
---
 
### **Decision Framework**
 
| Factor | Choose CP | Choose AP |
|--------|-----------|-----------|
| **Data Type** | Financial, Inventory | Social content, Analytics |
| **Cost of Inconsistency** | High (wrong balance = lawsuit) | Low (old tweet = minor UX issue) |
| **Cost of Unavailability** | Acceptable temporarily | Unacceptable (user leaves) |
| **User Expectation** | Accurate data | Fast response |
| **Write Frequency** | Low to medium | High (concurrent writes) |
| **Read Frequency** | Any | Very high |
 
---
 
### **Real-World Examples - AP Systems**
 
#### **1. Social Media Feed (Instagram, Twitter)**
**Why AP?**
```
Scenario: User posts a photo
 
CP Approach:
- Must wait for all replicas to confirm before showing post
- If one datacenter is down → post fails
- User sees error: "Try again later" ❌
 
AP Approach:
- Write to one datacenter immediately
- Replicate asynchronously to others
- User sees "Posted!" instantly ✅
- Some followers see it in 100ms, others in 2 seconds (eventual consistency)
- Acceptable trade-off
```
 
**Decision**: Stale feed (see yesterday's post) > App down
 
#### **2. Shopping Cart (Add to Cart)**
**Why AP?**
```
Scenario: Black Friday sale, 10M users adding items
 
CP Approach:
- Every "Add to Cart" waits for strong consistency
- During network partition → Cart service unavailable
- Users can't add items → Lost sales ❌
 
AP Approach:
- Allow "Add to Cart" even during partition
- Merge carts later (eventual consistency)
- Worst case: User has duplicate items in cart (easily fixable)
- Better than losing the sale ✅
```
 
**Decision**: Duplicate cart items > Cannot shop
 
#### **3. DNS (Domain Name System)**
**Why AP?**
```
Scenario: User types "google.com"
 
CP Approach:
- Wait for all DNS servers to agree on IP
- If any server is down → "Cannot resolve domain" ❌
- Website unreachable
 
AP Approach:
- Return IP from any available DNS server
- Might return slightly stale IP (old cached value)
- Website loads (even if not latest IP) ✅
```
 
**Decision**: 5-second stale DNS > Website unreachable
 
#### **4. Analytics Dashboard (Page Views, Metrics)**
**Why AP?**
```
Scenario: Real-time analytics showing website traffic
 
CP Approach:
- Wait for exact count from all servers
- If datacenter is partitioned → Show no data ❌
 
AP Approach:
- Show approximate count from available servers
- Might be 95,324 instead of 95,341 (off by 17)
- Still useful for decision-making ✅
```
 
**Decision**: Approximate metrics > No metrics
 
#### **5. Content Delivery Network (CDN)**
**Why AP?**
```
Scenario: Serving images/videos globally
 
CP Approach:
- Ensure all edge servers have identical content
- If sync fails → Don't serve content ❌
- User sees broken image
 
AP Approach:
- Serve from nearest edge server
- Might serve slightly old version of image
- User sees content (even if 1 min old) ✅
```
 
**Decision**: Stale image > No image
 
---
 
### **When You MUST Choose CP (Counter-Examples)**
 
#### **1. Banking Transactions**
```
Scenario: Transfer $1000 from Account A to Account B
 
Why NOT AP:
- Cannot show different balances to different users
- Cannot process duplicate transfers
- Regulatory compliance requires consistency
 
CP Approach:
- Use distributed transactions (2-phase commit)
- If network partition → Fail the transaction
- Show "Service temporarily unavailable" ✅
- User retries later when network is stable
```
 
#### **2. Ticket Booking (Flight, Concert)**
```
Scenario: Only 1 seat left, 2 users try to book
 
Why NOT AP:
- Cannot oversell (legal issue)
- Cannot show "Booked!" to both users
 
CP Approach:
- Lock seat during booking (distributed lock)
- Only one user gets it
- Second user sees "Sold out" ✅
```
 
#### **3. Inventory Management**
```
Scenario: Only 5 items in stock
 
Why NOT AP:
- Cannot accept 10 orders (angry customers)
- Cannot show wrong inventory count
 
CP Approach:
- Strong consistency on inventory count
- Decrement atomically
- Better to show "Out of stock" than oversell ✅
```
 
---
 
### **Interview Answer Template**
 
**"I'd choose AP over CP when eventual consistency is acceptable and availability is critical.**
 
**Examples:**
 
1. **Social media feeds**: Better to show a 2-second-old post than have the app down. Users expect instant responses.
 
2. **Shopping cart (add items)**: During Black Friday, allowing users to add items even during network partitions is crucial. We can resolve duplicate items later.
 
3. **Analytics dashboards**: Approximate metrics (off by 0.1%) are better than no metrics at all.
 
**The key question is: What's the business impact?**
- If inconsistency costs money (banking, ticketing) → Choose CP
- If unavailability costs money (e-commerce, social media) → Choose AP
 
**Implementation**: AP systems use eventual consistency with techniques like:
- Multi-master replication (Cassandra)
- Conflict-free replicated data types (CRDTs)
- Last-write-wins with timestamps
- Merge strategies for conflicts"**
 
---
 
## Practical Task: Sketch URL Shortener Architecture
 
### **High-Level Architecture Diagram**
 
```
┌─────────────┐
│   Client    │ (Browser/Mobile App)
│             │
└──────┬──────┘
       │ HTTPS
       │
       ▼
┌─────────────────────────────────────────────────┐
│         Load Balancer (L7)                      │
│         (HAProxy / AWS ALB)                     │
└──────┬──────────────────────┬───────────────────┘
       │                      │
       │                      │
       ▼                      ▼
┌──────────────┐      ┌──────────────┐
│  App Server  │      │  App Server  │  (Spring Boot)
│  (API Layer) │      │  (API Layer) │
└──────┬───────┘      └──────┬───────┘
       │                      │
       │                      │
       └──────────┬───────────┘
                  │
       ┌──────────┼──────────┐
       │          │          │
       ▼          ▼          ▼
   ┌─────┐   ┌─────┐    ┌──────────┐
   │Redis│   │MySQL│    │ Object   │
   │Cache│   │ DB  │    │ Storage  │
   │     │   │     │    │ (S3)     │
   └─────┘   └─────┘    └──────────┘
```
 
---
 
### **Detailed Architecture with Data Flow**
 
```
USER CREATES SHORT URL:
=====================
 
   Client
     │
     │ POST /shorten
     │ Body: {"longUrl": "https://example.com/very/long/url"}
     ▼
  Load Balancer
     │
     ▼
  App Server
     │
     ├─→ Generate Short Code (Base62: "aB3xY9")
     │
     ├─→ Check Redis Cache (code exists?)
     │   └─→ If exists: Return error (collision)
     │
     ├─→ Store in MySQL:
     │   TABLE: url_mappings
     │   ┌────────┬──────────────────┬───────────┬─────────────┐
     │   │  id    │   short_code     │ long_url  │ created_at  │
     │   ├────────┼──────────────────┼───────────┼─────────────┤
     │   │ 123456 │    aB3xY9        │ https://..│ 2024-03-15  │
     │   └────────┴──────────────────┴───────────┴─────────────┘
     │
     ├─→ Cache in Redis:
     │   Key: "short:aB3xY9"
     │   Value: "https://example.com/very/long/url"
     │   TTL: 1 hour
     │
     └─→ Return: {"shortUrl": "https://short.ly/aB3xY9"}
 
 
USER ACCESSES SHORT URL:
========================
 
   Client
     │
     │ GET /aB3xY9
     ▼
  Load Balancer
     │
     ▼
  App Server
     │
     ├─→ Check Redis Cache (Key: "short:aB3xY9")
     │   │
     │   ├─→ Cache HIT (99% of requests)
     │   │   └─→ Return long URL
     │   │
     │   └─→ Cache MISS (1% of requests)
     │       └─→ Query MySQL
     │           └─→ Update Redis Cache
     │           └─→ Return long URL
     │
     └─→ HTTP 301 Redirect to long URL
     
     
  Client follows redirect to original URL
```
 
---
 
### **Component Responsibilities**
 
#### **1. Client (Browser/App)**
```
Responsibilities:
- Send POST /shorten with long URL
- Send GET /{shortCode}
- Follow 301 redirect
```
 
#### **2. Load Balancer (L7)**
```
Responsibilities:
- SSL termination
- Distribute traffic across app servers
- Health checks
- Rate limiting (100 req/min per IP)
 
Why L7?
- Need to inspect URL path (/shorten vs /{code})
- Rate limiting per endpoint
```
 
#### **3. App Server (Spring Boot API)**
```
Endpoints:
 
POST /api/shorten
- Validate long URL
- Generate short code (Base62 encoding)
- Check for collisions
- Store in DB + Cache
- Return short URL
 
GET /{shortCode}
- Lookup in Cache → DB
- Return 301 redirect
- (Optional) Increment analytics counter
 
Core Logic:
┌──────────────────────────────────────┐
│ Short Code Generation Algorithm:     │
│                                      │
│ 1. Get auto-increment ID from DB    │
│    Example: ID = 123456              │
│                                      │
│ 2. Convert to Base62                 │
│    Base62 = [a-zA-Z0-9] (62 chars)  │
│    123456 → "w7e" (6 chars)         │
│                                      │
│ 3. Collision check in Cache/DB      │
│    If exists → Add random suffix     │
│                                      │
│ Result: "w7e" or "w7eX"             │
└──────────────────────────────────────┘
```
 
#### **4. Redis Cache**
```
Purpose: Fast lookups (avoid DB hit)
 
Data Structure:
- Key-Value Store
- Key: "short:aB3xY9"
- Value: "https://example.com/very/long/url"
- TTL: 1 hour (for popular URLs, no TTL)
 
Cache Strategy:
- Write-through: Write to DB + Cache simultaneously
- Read pattern: Cache-aside (check cache → DB → update cache)
 
Expected Cache Hit Rate: 95-99%
(Pareto principle: 20% URLs get 80% traffic)
```
 
#### **5. MySQL Database**
```
Schema:
 
CREATE TABLE url_mappings (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    short_code VARCHAR(10) UNIQUE NOT NULL,
    long_url TEXT NOT NULL,
    user_id BIGINT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    expires_at TIMESTAMP,
    INDEX idx_short_code (short_code),
    INDEX idx_user_id (user_id)
);
 
CREATE TABLE analytics (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    short_code VARCHAR(10),
    accessed_at TIMESTAMP,
    ip_address VARCHAR(45),
    user_agent TEXT,
    referrer TEXT,
    INDEX idx_short_code_time (short_code, accessed_at)
);
 
Why MySQL (not NoSQL)?
- Strong consistency needed (no duplicate short codes)
- ACID transactions
- Auto-increment IDs for Base62 encoding
- Small dataset (millions of URLs easily fit)
```
 
#### **6. Object Storage (S3) - Optional**
```
Purpose: Store QR codes for short URLs
 
- Generate QR code for short URL
- Store in S3: s3://bucket/qr-codes/{shortCode}.png
- Serve via CloudFront CDN
```
 
---
 
### **Key Design Decisions**
 
#### **1. Short Code Generation: Base62 Encoding**
```
Why Base62 (not Base10 or Base64)?
- Base10: Only 10^6 = 1M combinations for 6 chars
- Base64: Has special chars (+, /) → URL unsafe
- Base62: [a-zA-Z0-9] → URL safe + 62^6 = 56B combinations
 
Algorithm:
def base62_encode(num):
    chars = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
    result = ""
    while num > 0:
        result = chars[num % 62] + result
        num //= 62
    return result
 
Example:
ID 123456 → Base62 "w7e"
```
 
#### **2. Caching Strategy: Write-Through**
```
Why write-through (not cache-aside)?
- Avoids cache-DB inconsistency
- New short URLs immediately available in cache
- Read-heavy workload benefits from this
 
Flow:
1. Generate short code
2. Write to MySQL (ACID ensures uniqueness)
3. Write to Redis cache
4. Return short URL
```
 
#### **3. 301 vs 302 Redirect**
```
301 Permanent Redirect:
- Browser caches redirect
- Faster for user (no server hit after first time)
- Problem: Can't track analytics accurately
 
302 Temporary Redirect:
- Browser doesn't cache
- Every click hits server (can track analytics)
- Better for business (know click stats)
 
Decision: Use 302 for tracking, or 301 with async analytics
```
 
#### **4. Read vs Write Ratio**
```
Typical ratio: 100:1 (read-heavy)
- Most users click short URLs (read)
- Few users create short URLs (write)
 
Optimization:
- Heavy caching for reads (Redis)
- DB only for writes + cache misses
- Read replicas for MySQL (if needed)
```
 
---
 
### **Scalability Numbers**
 
```
Capacity Planning:
 
Daily URL creations: 1M
Daily URL clicks: 100M (100:1 ratio)
 
Storage:
- 1M URLs/day × 365 days = 365M URLs/year
- Assume 500 bytes per URL (includes metadata)
- Storage = 365M × 500 bytes = 182 GB/year
- 5 years = 1 TB (very manageable)
 
QPS:
- Clicks: 100M/day → 100M/86400 ≈ 1157 QPS average
- Peak: 1157 × 3 = 3500 QPS
- With 95% cache hit rate: Only 175 DB QPS (easy)
 
Servers needed:
- 1 server handles ~1000 QPS
- Need 4 servers for 3500 QPS (with headroom)
- + Load balancer
- + Redis cluster
- + MySQL master-replica setup
```
 
---
 
### **Interview Checklist**
 
When presenting URL Shortener architecture, mention:
 
- [x] Load balancer for distribution
- [x] Multiple app servers (horizontal scaling)
- [x] Redis cache (explain cache-aside pattern)
- [x] MySQL for persistence (explain schema)
- [x] Base62 encoding algorithm
- [x] Auto-increment ID strategy
- [x] 301 vs 302 redirect trade-off
- [x] Read-heavy optimization (caching)
- [x] Capacity estimation (storage, QPS)
- [x] Analytics tracking (optional)
 
