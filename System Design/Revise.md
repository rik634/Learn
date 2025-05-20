# System Design

- ShortCut: ADSCW LCM If RC
1. APIs: 
2. Databases(SQL vs. NoSQL)
3. Scaling
4. CAP Theorem
5. Web authentication and basic security
6. LoadBalancer
7. Caching
8. Message Queues
9. Indexing
10. Failover
11. Replication
12. Consistent Hashing

1. APIs: 
- APIs is the set of rules or contracts through which 2 or more software entities communicate with each other.
- API: Application Programming Interface
- REST, RPC, GraphQL
- URI: Uniform Resource Identifier
  a. REST: We use single URI to access the resource, and various actions based on the HTTP verbs(PUT/POST/GET/PATCH/DELETE) can be performed on the resource.
     - Pros: This method has tooling which supports generation of documentation.
     - Cons: Required to write request for each type of entity in database, which is not the case of GraphQL. Also, not as space efficient as that of RPA.
  b.  RPC: It allows the execution of procedure or command in remote machine.
     - Pros: It is more space efficient, and easy to developer.
     - Cons: Can only be used for internal communication. 
  c. GraphQL: More like pay as u go service. Here, we structure the data in graph relationships, and leave it to those using the service to define what they need. It is used for systems where graph like data fits in. Schema encompasses all the queries which we can make using APIs, and data types that can be returned.
     - Pros: Well suited for web and mobile applications, changes can be made without requiring backend work.
     - Cons: Not suitable when certain data needs to be aggregated on backend. Complex for the development at initial stages.


2. Databases(SQL vs. NoSQL):
   - SQl database is the relational database composed of tables.
   - NoSQL database is the nested key-value store.
   - SQL is fast for read and slow for write.
   - NoSQL is fast for write and slow for read.
   - The choice of database depends upon what u are storing, how often u are writing, what sorts of retrievals u make.
   a. SQL:
      - SQL: Structured Query Language
      - Compiler transforms the SQL query into machine code.
      - SQL ensures strong consistency of data. It is used when we are required to show the up-to date data. It is used in case of payment services, where we are required to show the accurate details about the transactions. While in case of messaging apps, it is acceptable for the messages to be out of sync for a moment of time.
      - SQL ensures ACID properties on database. So, it cam be used in cases of high consistency requirement.
      - ACID: Atomicity, Consistency, Isolation and Durability
        i. Atomicity: Fullness, transaction can ve either success or failure. If failure, it is rolled back. It is never halfway through.
        ii. Consistency: SQL have strong consistency. While, NoSQL have eventual consistency, i.e., data might be stale by couple of minutes.
        iii. Isolation: It ensures that changes being made in one transaction are not available in other transactions.
        iv. Durability: in case of failure during transaction, data is not lost and can be recovered.
      - SQL database is stored in B-Tree structure. Here, total space is divided into fixed size blocks, known as pages. Each page is traditionally 4kB in size and maps onto specific sector of hard drive space. when a page is too large, it is partitioned into child pages, and values/data gets sorted into them.
      - Cons: B-trees used for write operation are slow as compared to write operation in NoSQL. Writing is stores due to the way data is stored. NoSQL database have slow read due to their implementation.
      - Pros: 
   b. NoSQL:
      - Underlying data structure used is LSMT (Log-structured merge tree).
      - DynamoDB and MongoDB are managed NoSQL services. They come with sharding and scaling.
      - 4 types: key value, document database, columnar database, graph database
      - Key value database: Map like structure
      - document database: Most commonly used. Same as key-value, along with the ability for aggregate searches, and can store variety of formats like JSON, XML, and YAML
      - columnar database: store in tables, have denormalized data. indexing is based on columns. 
      - graph database: Store complicated node and edge relationship. 
      - Pros: fast for write
      - Cons: slow for read, not suitable where strong consistency is required.
3. Scaling:
   - Types: Horizontal(get more servers to distribute the load) and vertical (make current server more powerful to handle increasing requests)
   a. vertical scaling: 
      - Pros: In case of scaling, at initial stage, we go for vertical scaling (as it does not involve software architecture change, pros of reducing latency, as the communication is done locally, and does not involve it to occur through network).
      - Cons: It has hardware limitation, can not upgrade hardware specifications after certain limit. There is risk of single system creating the single point of failure.
   b. Horizontal scaling:
      - Types: database scaling and compute scaling.
        i. Database scaling: I
           - In order to improve the performance, we can add database replicas. To shard the data into different databases is known as database sharding. It is more like dividing the large database /data, into smaller parts/databases.
           - Here, we use hash function to determine, which database stores which data, also for storing the data into one of the databases.
           - As the amount of data to be handled increases further, we can add additional level of sharding to make queries more efficient.
        ii. Compute scaling:  
           -  Processing large amount of data on a single computer might be slow. Here, we divide the data into pieces, and designate each piece as a job in a queue, so that multiple computers can work together in parallel.
    c. Horizontal and vertical scaling together:
       - Horizontal scaling is not the right approach always.
       - Keeping certain processes and services in a single machine can reduce the latency between the calls. 
       - And in order to avoid the single point of failure, we make use of duplicate/ backup nodes, that is horizontal scaling. By making use of load balancer, we can distribute the request to the respective nodes.
4. CAP Theorem:
   - CAP: Consistency, Availability, Partition Tolerance
   - It is the most fundamental theorem for distributed system design.
   - In general, we scale to multiple servers from single servers, to avoid the single point of failure.
     a. Consistency: Every node in the network will have access to the same data.
     b. Availability: System is always available to the users.
     c. Partition tolerance: In case of fault in the network or communication, the system will still work.
   - According to CAP theorem, distributed system needs to make a choice between consistency, availability, and partition tolerance. It can not achieve all three, so, system should be designed to achieve 2 out of these 3.
5. Web authentication and basic security:
   - Authentication: It refers to identifying the identity of service's users.
   - Authorization: It is the concept of determining which users have permissions to take which actions.
   - Software often relies on password for authentication.  The most typical way of authentication is with username and password combination. Here, we can store the password text in a data store alongside the username, and we can compare the password with user's submission. It has the flaw of single point of failure, i.e., attackers might have access to the passwords. To resolve this problem, we go for: Hashing
   a. Hashing:
      - So, for secure password storage, we can make use of cryptographic hash.
      - A hash function is a function which accepts an input, and maps it to smaller value. 
      - cryptographic hash functions are designed in such a way to make it mathematically difficult to derive the output from the input.
      - So, when a user sign ups, we run the password through secure hash function and store the output. The output is referred as the password's hash.
      - Now, we want to authenticate a user. We get the hash of the password provided by the user, and check if it is present in our database. If present, user gets successfully logged in.
   b. Salting:
      - We can encounter a issue, that the password might be easily guessable.
      - In such case, we salt the password by adding some random words that aren't obvious.
      - Salting the passwords make it harder for the attackers to uncover the password. 
      - A rainbow table is an enormous lookup table containing millions of common passwords and their variations, along with their hashes for a common hash algorithm. 
      - For an attacker, with hands on rainbow table, it can easily crack user's passwords.
      - Normally, users tend to use common passwords. To defend the attack by hackers, we go for salting. 
      - The concept behind salting is: Rather than hashing the password, we hash the password concatenated to a random value, which we can store in a plain text along with hash.
      - We apply same salting procedure for the user-provided passwords for login, to authenticate them.
      - Also, to make it difficult for attackers to get password, we use use slower hash function, which will slow down the guess approach for them.
      - In real world, avoid writing code for salt, hash and storing password. Instead, use libraries, for hash generation and password comparison.
   
   Web Sign-in:
   - Classic way for sign-in is with a form submission. Recently, async requests are used.
   - It is sensitive to send user's username and password in the authentication flow. So, login handler should use HTTPS, to ensure that credentials don't pass as plain text. Better encrypt all web traffic with HTTPS, in particular protect the password.
   - Avoid running automatic logging on login endpoints, as it will write user's passwords in plain text to the logs. It compromises the personal information. So, avoid automatically logging POST bodies and GET parameters.  Instead, keep automatic logging for URL paths, response codes, log parameters, after verifying they don't include sensitive information.
   - When a user logs in, we need a way to prove the authentication of the user, on all the subsequent requests, so that, user does not need to log in on every single page request. Simply saying, we need a way to track the authentication.
   
   Session Token:
   - Onw way to track the authentication is to generate a token for a user, which will be submitted with eh subsequent requests to track/ prove their authentication. 
   - This token should be randomly generated, and use a secure random number generator, and it should be long enough to be infeasible to brute force.
   - This token represents this particular sing-in session for this user. We can check for this session token in a cookie on subsequent requests, verifying for same logged-in user.
   - Session token is equivalent to password. We should not store the session token in database, rather, salt and hash it the same way as password.
   - Session token should have an expiration date, as short a possible.
   - The expiration of session token does not mean that user has to log in again, we can rotate them silently in background between one request and the next. 
   - Setting the expiration, will limit the amount of time the user can remain logged in without visiting the site. We can set the expiration time accordingly.
   - The shorter the expiration time, the less opportunity an attacker who steals user's session token will have to exploit it.
   - This is the reason, we should encrypt all traffic with HTTPS. 
   - If we secure only the login endpoints, the user;s subsequent requests will be vulnerable to interception and each of them will include the session token.

   JSON Web Token:
   - Instead of plain session tokens, we can also opt to use JSON Web Tokens, or JWTs.
   - The session token is an opaque string, which means nothing without access to the session database.
   - A JWT encodes the user's access. It may list the logged in user's username or ID, as well as explicit capabilities or permission that have been authorized for that specific login.
   - JWT standard defined a canonical way to encode this data along with expiration and other metadata. 
   - We can use the same concept for our own custom payloads if needed.
   - A JSON payload listing the user's ID and permissions would be easy to falsify, so, we can't use the JSON alone to verify a logged in user. To render a JWT valid, the server must do one of the following:
   i. Sign the payload:
   - Attaching a signature from a private key held only by our service verifies the token's legitimacy.
   - This approach has 2 advantages. The first is that the token is fully transparent. We can use it on the client side to tell who's logged in and optionally what permission they have. The second and most important one is that, we can use the token with other services.
   - As long as we publish the corresponding public key to the private key used to sign the token, not only our own services, but the services from entirely different providers can validate the token, allowing the user to authenticate  elsewhere on the web.

   ii. Encrypt the payload:
   - We can encrypt the payload, so that only our service can read it.
   - This is less commonly used. Ans it turns the token into something closer to simple session token, neither the client nor the third party services can derive any information from it.
   - But, if we distribute the decryption key to different services, they will be able to validate the encrypted token without needing access to centralized session database.
   -  we can also store the small amounts of data for the duration of the session without writing them to database or making them visible to user, We can just insert the data we need into the encrypted token, and we will have access every time the user presents their token to verify session.

Cookies:
- Session token, or JWTs, whatever method we use to track the authentication, we will need a mechanism to store the token on the client side and send it back to the server with each request.
- The standard way to do this in web applications is to use the browser cookies.
- A cookie is simply a text string that a browser associates with a given key and sends back to the server whenever it makes a request.
- The server may include the cookie, in any response, or one or more "Set-Cookie" headers.
- The header indicates the key and value to store, along with some metadata regarding expiration and security.
- Subsequently, the browser automatically includes those cookie values in every request made to the same domain (potentially limited by path, if one was specified while setting the cookie).
- By storing session token or JWT in a cookie, we can ensure that all subsequent requests will include it and allow the server to validate the current user session.
- Since auth tokens- whether session token or JWT - are equivalent to passwords, we need to be very careful about who can access these cookies and how.
- Setting the "Secure" flag on cookies tells the browser only to include it in HTTPS requests, keeping it out of unencrypted traffic, where it could be intercepted.
- The "HttpOnly" flag tells the browser not to allow access to the cookies through JavaScript. The cookies will still be sent in request header, but snooping frontend code won;t have direct access to it.
- The "SameSite" flag allows to specify the degree of caution the browser should take when sending requests that originate from different site, to help prevent cross-site request forgery (CSRF) attacks.

Process:
- The user signs up. At this point, we need to salt and hash their password and store those values.
-  The user logs in with username and password. The password is verified by hashing it with stored salt and checking to see if it matches the stored hash. W then send identifying token, either session token or JWT, back to client in cookie set header.
- On subsequent requests, browser sends the cookie back to the server, where we can verify the session token, or check the signature on/ decrypt a JWT.
- Periodically, session token or JWT has to be expired and new one is generated and sent down to client with cookie set header.
- eventually, user's session may expire from inactivity. Then, we go back to creating another token in step 2.

6. LoadBalancer:
- Load balancers are important component of any distributed system and helps to distribute traffic across the machines.
- As distributed systems are designed to scale, the basic requirement is to add or remove the machines in case of increased load or in case of failures. Load balancers help to manage this.
- Algorithms used are:
  a. Round Robin:
     - In this technique, if there are "N" machines, then the load balancer will send requests to each of them one by one. Although, this should mostly work, in the case where machine is loaded (not able to process requests in timely fashion), the load balancer keeps bombarding the machine.
  b. Least connection/ response time:
     - We send requests to the machine with least connections or the minimum response time. This option is useful for incoming requests that have varying connection times and a set of servers that are relatively similar in terms of available compute and other resources.
  c. Hashing:
     - Hashing is based on an arbitrarily chosen key. 
     - The key for hashing can be request id for a given user or an IP address. The hash function returns a numeric result for each key. 
     - Using the hashing result, we can obtain the server id to which the request should be sent:             machineID = H(requestID)%n
     - Here, the addition and subtraction of machines leads to the same request being sent to multiple machines.

7. Caching:
   - It stores the expensive computation, so that we don't have to compute it again.
   - We use caching to reduce the latency of an expensive network computation or network calls or database queries or asset fetching.
   - Generally, caching is used at the expense of more storage (or more memory). Normally, we store it in memory (RAM), but it can be stored on disk too.
   - Generally, caching is used for read-heavy systems. Popular read-heavy systems are twitter and youtube. Their most common users are reading tweets and watching videos.
   
   The 80/20 rule:
   - We want to store 80% of read requests in 20% of storage. Generally, they are the most popular requests.
   - For example, we cache the tweet from a famous person, on device, on CDN, on the application. 
   - CDN (Content Delivery Network) is commonly used to deliver the cached data.
   - Here, the 80/20 rule is used, which says that 80% of the queries should be served by 20% of popular data which is stored.
   -  Caches store data in local regions.
   - Popular caching patterns:

   a. Cache aside pattern:
      - It is the most popular cache pattern.
      - Here, we have an application which will try to fetch data from the cache, and if the data is not found (also known as ""cache miss"), it will fetch data from the database/ will do an expensive computation.
      - And it will put the data back to the cache before returning the query back to the user.
      - Advantage: we only cache the data that we need.
      - Disadvantage:
        i. The data can become stale, if there are lots of updates to the database. This cons can be mitigated by having "Time To Live", or any other expiry pattern.
        ii. If there are a lot og cache miss in our application, then the application has to do a lot more work than in regular flow of fetching data solely from the database. Here, the application will first go to the cache, then there will be a cache miss, then it will go back to the database, and then write the data back to the cache before going back to user. Cache is used to reduce latency. But, here, it is making the latency worse.

   b. Write-through and Write-back patterns:
      - Here, the application directly writes the data to the cache. 
      - Here, the cache synchronously (or asynchronously) writes the data to the database. 
      - When we write it synchronously, it is called "write-through", and when we write it asynchronously, it is called "write-back" (or "write-behind"). 
      - In the asynchronous process, we put the data in queue, which writes the data back to the database and improves the latency of writes of application.
      - If we are experiencing slow writes, a quick fix is async writes to the database.
      - In both the ways, we are writing all the data to the cache, which is not even being read. So, we are just overloading the cache (or cache memory) with expensive calls which might not even be required. 
      - Also, if the database goes down before the data is written to the database, this will cause inconsistency.
   
   Caching on the client side:
   - Caching can be done for database queries, expensive computations, or to save network costs.
   
   Cache invalidation:
   - Problem with caching is that data can become stale if there are lots of updates to the database.
   - So, it is important to expire or invalidate data from the cache. So, it will not get stale.
   - There are many policies to invalidate data from the cache. The most important or commonly used one is "Least Recently Used" (or LRU).
   - For most systems 20% of the data accounts for 80% of the reads. So, by using LRU, will result in fewer cache misses. 
   - Here, we can throw stuff in the cache, which reduces latency for 80% of your requests.

8. Message Queues:
   - An asynchronous system is one where the client sends the request and does not wait for the message to be delivered. 
   - Fo the client, sending the message is like any other function or an API call, but in contrast to the APIs, the clients do not expect a response. Such functions or API calls are also known as "fire and forget" calls. 
   - Here, the client's message is not immediately sent over to the processing system but instead sent ot the intermediary. This intermediary is the message queue or the broker.
     Advantages:
     - A queue stores messages that need to be stored in a database. This is used if there's the possibility that traffic will spike, causing the CPU of the database to go up like crazy and kill the server. That would cause the database to be down and probably lose data. So, instead throw it in the queue.
     - If a message has to be processes by some very expensive code, we can hold it in queue while previous messages are being processed. So, we don't overload (or potentially kill) the servers.
     - Queues can deliver messages to multiple systems, instead of the client having to send them to all the required systems.
     - Queues decouple the client from the server by eliminating the need to know the server address.
   
   -  In an asynchronous system, there are producers and consumers/subscribers.
   - Message is the data that flows through an asynchronous system. There is usually no particular data model enforced.
   - Producers are the systems that produce the messages.
   - Consumers are the systems that process those messages.
   - Message queue or topic is the link between the producers and the consumers.
   - Producers publish the messages to a named queue, and the queue delivers these messages to the consumer of the queue.
   - There can be different message queues for different needs and consumers subscribe to them based on their requirements.
   - On the contrary, there can be a single message queue delivering all messages to all the consumers and filtering out the messages that they can process. 
   - Or else, there can also be a single message queue, smart enough to selectively deliver messages to the appropriate subscribers.
   - Based on the different implementations of the message queues, there can be different combinations of the following properties:
     i. Guaranteed delivery
     ii. No duplicate messages are delivered
     iii. Ensure that the order of the messages is maintained
     iv. At least once delivery with idempotent consumers.
   
   - Products for message queues: Kafka, RabbitMQ

9. Indexing:
   - Sometimes we experience a slow website or customer portal that takes too long to fetch the data. A pervasive issue with such systems is the lack of proper indices. As a result, database queries take much longer than expected to find the information.
   - Indexing is a mechanism by which the underlying data is mapper for faster retrieval.
   - For a system to process an instruction involving data access, these are the certain steps involved:
     a. Fetch the block of data from the hard disk (secondary/ permanent storage) to primary memory (RAM)
     b. Look for the desired data in the RAM.
     c. Repeat above steps till the desired data is fetched.
   - In hard disk, there might be GBs or TBs of data stored. If we try to look at each record in the disk to search for a specific record, this would take a long time.
   - Instead, if we have the index of records on the disk, the process would be much faster.
   - This index for records is more like a map or key-value pair, where the key identified a record, and the value is the location of the record on the disk.
   - When the index maintains a key-value mapping for each of the records in the database, it is called a dense index.
   - On the other hand, when the index maintains a mapping for a subset of the records, then it is called a sparse index.
   - If we search a record in hard disk, index would first be loaded in the RAM, and then based on the key, we will retrieve the record/block address. Once we have the block address, the specific block from hard disk would be loaded instead of loading all the blocks. And from the block, we can fetch desired record.
   - By maintaining an index, we could reduce the I/O calls to disk substantially.

   Multilevel Index:
   - As the size of database grows, the size of the index will also grow. A large index requires multiple I/O calls to read the index itself.
   - In case of larger database size, the scenario without index is preferred to the indexing. In such cases, we go for multilevel indices.
   - Multilevel indices maintain an index of indices. This means that to fetch a record, first an index is referenced to get the address for the block storing the correct index.
   - These multiple level of indices reduce the I/O calls. 
     i. One call to load the level 1 index
     ii. The second call to load the level 2 index as determined from level 1 index.
   - The levels of indices can keep on increasing as the data grows to keep the indexing efficient,

  B-Trees:
  - B-Trees are self-adjusting trees that can achieve multilevel indexing.
  - They are the generalized form of binary search tree. The data is stored in sorted order in B-trees. B-tree achieves the efficient utilization of spaces in nodes, along with keeping the height of tree small.
  - For a B-tree of order n, following are the properties of B-tree:
    i. Each node can have at most n-1 keys.
    ii. Each node can have max of n children.
    iii. All the keys of the left subtree are smaller than the keys of subtree on the right.
    iv. Each internal node except for the root node muts have at least n/2 children, x represents the ceiling function.
    v. All leaves are at the same depth.
    
10. Failover
11. Replication
12. Consistent Hashing
