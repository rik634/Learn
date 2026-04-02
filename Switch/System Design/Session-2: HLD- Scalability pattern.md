# Horizontal vs Vertical scaling

## 1. Scaling Fundamentals (5 minutes)
 
### What is Scaling?
 
**Scaling** = Increasing system capacity to handle more load (users, requests, data)
 
**Why Scale?**
- More users → More traffic
- Business growth
- Seasonal spikes (Black Friday, IPL matches)
- Prevent downtime
- Maintain performance (latency < 200ms)
 
**Two Dimensions of Scaling:**
1. **Vertical Scaling** (Scale UP): Make server bigger
2. **Horizontal Scaling** (Scale OUT): Add more servers
 
---

## 2. Vertical Scaling (Scale UP) - 10 minutes
 
### Definition
**Increase resources of a single server**: More CPU, RAM, Disk, Network bandwidth
 
### Example
```
Before: AWS EC2 t3.medium
- 2 vCPU
- 4 GB RAM
- Handles 500 req/s
 
After: AWS EC2 m5.2xlarge
- 8 vCPU
- 32 GB RAM
- Handles 2000 req/s
```
 
### When to Use Vertical Scaling
 
✅ **Good for:**
- **Early stage / MVP**: Simple, no code changes needed
- **Databases**: MySQL, PostgreSQL (hard to shard)
- **Monolithic applications**: Legacy systems
- **Stateful applications**: In-memory caches, sessions
- **Single-threaded workloads**: Can't parallelize
- **Quick fix**: Need scale NOW, no time for architecture changes
 
❌ **Bad for:**
- **High availability requirements**: Single point of failure
- **Extreme scale**: Hardware limits (max 448 vCPU, 24 TB RAM on AWS)
- **Cost optimization**: Exponential cost increase
- **Microservices**: Better to scale horizontally
 
### Pros and Cons
 
| Pros | Cons |
|------|------|
| **Simple**: No code changes | **Single Point of Failure**: Server down = service down |
| **No distributed complexity**: No network latency | **Hardware Limits**: Can't scale infinitely |
| **Data consistency**: Single database, no sync issues | **Downtime during upgrade**: Must restart server |
| **Fast to implement**: Just upgrade instance | **Expensive**: Doubling capacity costs 4x |
| **No inter-process communication**: Fast | **No redundancy**: Maintenance = downtime |
 
### Cost Analysis (AWS EC2 Example)
 
```
t3.medium:   2 vCPU, 4 GB RAM   → $30/month   (baseline)
t3.large:    2 vCPU, 8 GB RAM   → $60/month   (2x cost, 2x RAM)
m5.xlarge:   4 vCPU, 16 GB RAM  → $140/month  (4.6x cost, 4x capacity)
m5.4xlarge:  16 vCPU, 64 GB RAM → $560/month  (18x cost, 16x capacity)
```
 
**Observation**: Cost grows **faster** than capacity (non-linear)
 
### Real-World Example: Stack Overflow
 
**Stack Overflow (early days):**
- Ran on a single server for years
- Vertically scaled to massive specs
- Eventually added read replicas (minimal horizontal scaling)
- Still runs on surprisingly few servers (<10)
 
**Why it worked:**
- Read-heavy workload (90% reads, 10% writes)
- Highly optimized code
- Aggressive caching
- Business model allowed downtime during upgrades
 
---

## 3. Horizontal Scaling (Scale OUT) - 15 minutes
 
### Definition
**Add more servers** of the same size to distribute load
 
### Example
```
Before: 1 server × 8 vCPU = 8 total vCPU
        Handles 2000 req/s
 
After:  10 servers × 8 vCPU = 80 total vCPU
        Handles 20,000 req/s
```
 
### When to Use Horizontal Scaling
 
✅ **Good for:**
- **High availability**: No single point of failure
- **Extreme scale**: Add servers infinitely
- **Stateless applications**: Web servers, API servers
- **Microservices architecture**: Independent scaling
- **Cost optimization**: Linear cost growth
- **Cloud-native apps**: Auto-scaling, elastic
- **Read-heavy workloads**: Add read replicas
 
❌ **Bad for:**
- **Stateful applications**: Session data, caches (need external store)
- **Databases** (without sharding): Write bottleneck remains
- **Complex data consistency**: Distributed transactions hard
- **Small scale**: Overhead not worth it (<10K requests/day)
 
### Pros and Cons
 
| Pros | Cons |
|------|------|
| **No single point of failure**: HA + fault tolerance | **Complexity**: Distributed systems are hard |
| **Infinite scalability**: Add servers as needed | **Data consistency**: CAP theorem trade-offs |
| **Linear cost**: 2x servers = 2x capacity = 2x cost | **Network latency**: Inter-server communication |
| **Zero downtime deployments**: Rolling updates | **State management**: Sessions, caches need external store |
| **Geographic distribution**: Servers in multiple regions | **Load balancing**: Need LB (single point of failure?) |
| **Elastic scaling**: Auto-scale based on load | **Debugging**: Harder to trace requests |
 
### Cost Analysis (AWS EC2 Example)
 
```
1 server  × t3.xlarge (4 vCPU, 16 GB)  → $120/month  → 4K req/s
5 servers × t3.xlarge (20 vCPU, 80 GB) → $600/month  → 20K req/s
10 servers × t3.xlarge (40 vCPU, 160 GB) → $1200/month → 40K req/s
```
 
**Observation**: Cost grows **linearly** with capacity
 
### Real-World Example: Netflix
 
**Netflix:**
- Runs on 1000s of AWS EC2 instances
- Horizontally scaled microservices
- Each service scales independently
- Can lose 100 servers with zero impact
- Auto-scales based on traffic
 
**Why it worked:**
- Microservices architecture
- Stateless services
- External session store (distributed cache)
- Cloud-native from day 1
 
---
 
## 4. Vertical vs Horizontal: Decision Framework (5 minutes)
 
### Quick Decision Matrix
 
| Factor | Vertical | Horizontal |
|--------|----------|-----------|
| **Current load** | < 10K req/day | > 100K req/day |
| **Availability requirement** | 95% (some downtime OK) | 99.9% (no downtime) |
| **Budget** | Limited (startup) | Flexible (enterprise) |
| **Application type** | Monolith, stateful | Microservices, stateless |
| **Team size** | 1-5 engineers | 10+ engineers |
| **Timeline** | Need scale NOW | Can invest in architecture |
| **Data consistency** | Strong consistency needed | Eventual consistency OK |
 
### The Typical Journey (Most Companies)
 
```
Stage 1: Single Server (Vertical Only)
  - MVP, 0-1K users
  - 1 server runs everything (app + DB)
  
Stage 2: Separate DB (Vertical for both)
  - 1K-10K users
  - 1 app server + 1 DB server
  
Stage 3: Add Load Balancer + Horizontal App Servers
  - 10K-100K users
  - 1 LB + 3-5 app servers + 1 DB server
  - DB still vertical (largest bottleneck)
  
Stage 4: DB Read Replicas (Partial Horizontal)
  - 100K-1M users
  - 1 LB + 10-20 app servers + 1 master DB + 3-5 read replicas
  
Stage 5: Full Horizontal + Sharding
  - 1M+ users
  - Auto-scaling app servers
  - Sharded databases
  - Microservices
```
 
### Interview Answer Template
 
**"When would you choose vertical vs horizontal scaling?"**
 
**"I'd consider three factors:**
 
1. **Current scale and growth rate**: If we're handling 1K req/s and expect slow growth, vertical scaling is simpler. If we're at 10K req/s and doubling every 6 months, horizontal scaling is necessary for sustainability.
 
2. **Application architecture**: If we have a Spring Boot monolith with session state in memory, vertical scaling requires no code changes. Horizontal scaling would require externalizing sessions to Redis and ensuring stateless design.
 
3. **Availability requirements**: If we can tolerate 1-2 hours of downtime per month for upgrades, vertical is acceptable. If we need 99.9% uptime (< 8 hours/year), we need horizontal scaling with redundancy.
 
**Practical approach**: Start with vertical scaling to validate product-market fit, then transition to horizontal as you hit limits. Most successful companies do both — vertically scale databases while horizontally scaling app servers."
 
---

## 5. Stateless vs Stateful Services (10 minutes)
 
### Definitions
 
**Stateless Service:**
- Doesn't store any client session data on the server
- Each request is independent
- Any server can handle any request
- **Horizontally scalable by default**
 
**Stateful Service:**
- Stores client session data in server memory
- Requests from same client must go to same server (sticky sessions)
- OR session data must be externalized
- **Hard to scale horizontally**
 
### Examples
 
#### Stateless Service Example (Spring Boot)
 
```java
@RestController
@RequestMapping("/api/products")
public class ProductController {
    
    @Autowired
    private ProductService productService;
    
    @GetMapping("/{id}")
    public ResponseEntity<Product> getProduct(@PathVariable Long id) {
        // No session data needed
        // Authenticated via JWT token in request header
        // Any server instance can handle this request
        Product product = productService.findById(id);
        return ResponseEntity.ok(product);
    }
    
    @PostMapping
    public ResponseEntity<Product> createProduct(
            @RequestHeader("Authorization") String token,
            @RequestBody Product product) {
        
        // Extract user from JWT token (stateless auth)
        Long userId = jwtService.extractUserId(token);
        
        // Process request (no session state needed)
        Product created = productService.create(product, userId);
        return ResponseEntity.status(201).body(created);
    }
}
```
 
**Why it's stateless:**
- Authentication via JWT in every request (not stored on server)
- No session object, no HttpSession
- Database stores all state
- Server is interchangeable
 
#### Stateful Service Example (Old School Spring Boot)
 
```java
@RestController
@RequestMapping("/api/cart")
public class CartController {
    
    // BAD: Storing cart in session (server memory)
    @PostMapping("/add")
    public ResponseEntity<String> addToCart(
            @RequestBody CartItem item,
            HttpSession session) {
        
        // Cart stored in server memory
        Cart cart = (Cart) session.getAttribute("cart");
        if (cart == null) {
            cart = new Cart();
        }
        
        cart.addItem(item);
        session.setAttribute("cart", cart);
        
        return ResponseEntity.ok("Added to cart");
    }
    
    @GetMapping
    public ResponseEntity<Cart> getCart(HttpSession session) {
        // Must hit the SAME server that has the session
        Cart cart = (Cart) session.getAttribute("cart");
        return ResponseEntity.ok(cart);
    }
}
```
 
**Why it's stateful:**
- Cart data stored in HttpSession (server memory)
- If request goes to different server → cart is lost
- Requires sticky sessions or session replication
 
### Converting Stateful to Stateless
 
#### Solution 1: Externalize Session to Redis (Recommended)
 
```java
// Spring Session + Redis configuration
@Configuration
@EnableRedisHttpSession
public class SessionConfig {
    
    @Bean
    public LettuceConnectionFactory connectionFactory() {
        return new LettuceConnectionFactory();
    }
}
 
// Controller code stays the same!
@RestController
@RequestMapping("/api/cart")
public class CartController {
    
    @PostMapping("/add")
    public ResponseEntity<String> addToCart(
            @RequestBody CartItem item,
            HttpSession session) {  // Now backed by Redis
        
        Cart cart = (Cart) session.getAttribute("cart");
        if (cart == null) {
            cart = new Cart();
        }
        
        cart.addItem(item);
        session.setAttribute("cart", cart);  // Stored in Redis
        
        return ResponseEntity.ok("Added to cart");
    }
}
```
 
**How it works:**
1. Spring Session intercepts HttpSession calls
2. Stores session data in Redis (distributed cache)
3. Session ID stored in cookie
4. Any server can retrieve session from Redis
5. **Now horizontally scalable!**
 
#### Solution 2: Move State to Client (JWT Approach)
 
```java
@RestController
@RequestMapping("/api/cart")
public class CartController {
    
    @Autowired
    private CartService cartService;
    
    @PostMapping("/add")
    public ResponseEntity<String> addToCart(
            @RequestBody CartItem item,
            @RequestHeader("Authorization") String token) {
        
        Long userId = jwtService.extractUserId(token);
        
        // Store cart in database, not session
        cartService.addToUserCart(userId, item);
        
        return ResponseEntity.ok("Added to cart");
    }
    
    @GetMapping
    public ResponseEntity<Cart> getCart(
            @RequestHeader("Authorization") String token) {
        
        Long userId = jwtService.extractUserId(token);
        
        // Fetch from database
        Cart cart = cartService.getUserCart(userId);
        return ResponseEntity.ok(cart);
    }
}
```
 
**Benefits:**
- Truly stateless
- No Redis dependency
- Horizontally scalable
- Each request is independent
 
### Stateless vs Stateful: Comparison
 
| Aspect | Stateless | Stateful |
|--------|-----------|----------|
| **Scaling** | Easy horizontal scaling | Hard without external store |
| **Load balancing** | Any algorithm works | Needs sticky sessions or shared state |
| **Failure recovery** | Instant (any server) | Session lost or need replication |
| **Performance** | May need DB/Redis call per request | Fast (in-memory) |
| **Complexity** | Simple architecture | Complex session management |
| **Examples** | REST APIs, microservices | Old JSP apps, desktop apps |
 
### Interview Answer
 
**"How would you make a stateful Spring Boot app horizontally scalable?"**
 
**"I'd externalize the session state using Spring Session with Redis:**
 
1. **Add dependency**: Spring Session Data Redis
2. **Configure Redis**: Connection factory bean
3. **Enable**: @EnableRedisHttpSession
4. **No code change needed**: HttpSession automatically backed by Redis
 
**Alternative**: Redesign as stateless using JWT authentication and store user data in database/cache. This is cleaner long-term but requires more code changes.
 
**Trade-off**: Redis adds latency (network call) vs in-memory session (local). But Redis gives us horizontal scalability and high availability."
 
---

## 6. Load Balancing Algorithms (10 minutes)
 
### What is a Load Balancer?
 
**Load Balancer (LB)** = Distributes incoming requests across multiple servers
 
```
Client requests → Load Balancer → Chooses server → Server handles request
```
 
### Common Algorithms
 
#### 1. Round Robin (Most Common)
 
**How it works:**
- Requests distributed in circular order: Server 1 → Server 2 → Server 3 → Server 1 ...
- Simple counter, no intelligence
 
**Pros:**
- Simple to implement
- Fair distribution (assuming equal server capacity)
- Stateless (no tracking needed)
 
**Cons:**
- Doesn't consider server load (one server might be busy)
- Doesn't consider server capacity (if servers are different sizes)
- Bad for long-lived connections
 
**Example:**
```
Request 1 → Server 1
Request 2 → Server 2
Request 3 → Server 3
Request 4 → Server 1
Request 5 → Server 2
...
```
 
**Use case:** Stateless APIs with uniform request processing time
 
#### 2. Weighted Round Robin
 
**How it works:**
- Assign weight to each server based on capacity
- Server with higher weight gets more requests
 
**Example:**
```
Server 1: 16 vCPU, Weight = 4
Server 2: 8 vCPU,  Weight = 2
Server 3: 4 vCPU,  Weight = 1
 
Distribution:
S1, S1, S2, S1, S3, S1, S2 (repeats)
Server 1 gets 4/7 requests
Server 2 gets 2/7 requests
Server 3 gets 1/7 requests
```
 
**Use case:** Heterogeneous server fleet (different instance sizes)
 
#### 3. Least Connections
 
**How it works:**
- Track active connections per server
- Route new request to server with fewest active connections
- Requires stateful load balancer
 
**Example:**
```
Server 1: 5 active connections
Server 2: 3 active connections
Server 3: 8 active connections
 
New request → Goes to Server 2 (least connections)
```
 
**Pros:**
- Better distribution for long-lived connections
- Adapts to server load dynamically
 
**Cons:**
- Stateful (LB must track connections)
- More complex
- Overhead of tracking
 
**Use case:** WebSocket connections, long-polling, video streaming
 
#### 4. IP Hash (Source-based)
 
**How it works:**
- Hash client IP address
- Route to server based on hash value
- Same client always goes to same server
 
**Algorithm:**
```
server_index = hash(client_ip) % num_servers
```
 
**Example:**
```
Client IP: 203.0.113.50
Hash: 12345
12345 % 3 = 0 → Server 1
 
Same client always → Server 1
```
 
**Pros:**
- Sticky sessions without session store
- Good for caching per-server (cache hit rate)
 
**Cons:**
- Uneven distribution if few clients
- If server goes down, all its clients remap (cache miss)
- Can't handle client IP changes (mobile users)
 
**Use case:** Applications needing session affinity without external store
 
#### 5. Least Response Time
 
**How it works:**
- Track average response time per server
- Route to server with lowest latency
- Requires health checks
 
**Example:**
```
Server 1: 50ms avg response
Server 2: 120ms avg response (high load)
Server 3: 45ms avg response
 
New request → Goes to Server 3
```
 
**Use case:** Performance-critical APIs, auto-scaling environments
 
#### 6. Consistent Hashing (Advanced)
 
**Problem with IP Hash:**
```
3 servers: hash(IP) % 3
Server 3 goes down → Now 2 servers: hash(IP) % 2
Result: 66% of clients remap to different servers!
```
 
**Consistent Hashing Solution:**
- Map servers to points on a circle (hash ring)
- Map client IPs to points on same circle
- Client routed to nearest server clockwise
- When server fails, only its keys remap
 
**Visual:**
```
Circle (0-360 degrees):
Server A at 60°
Server B at 150°
Server C at 270°
 
Client 1 (hash=80°) → Goes to Server B (next clockwise at 150°)
Client 2 (hash=200°) → Goes to Server C (next clockwise at 270°)
Client 3 (hash=300°) → Goes to Server A (wraps around, 60°)
 
Server B fails:
Client 1 (80°) → Now goes to Server C (270°)
Client 2 and 3 unchanged!
```
 
**Benefits:**
- Minimal remapping when servers added/removed
- Only K/N keys remap (K = total keys, N = num servers)
- Used by: Memcached, Redis Cluster, Cassandra
 
**Implementation (Java):**
```java
public class ConsistentHash {
    
    private TreeMap<Integer, String> circle = new TreeMap<>();
    private int virtualNodes = 150; // Per server
    
    public void addServer(String server) {
        for (int i = 0; i < virtualNodes; i++) {
            int hash = hash(server + "#" + i);
            circle.put(hash, server);
        }
    }
    
    public String getServer(String clientIP) {
        int hash = hash(clientIP);
        
        // Find next server on circle
        Map.Entry<Integer, String> entry = circle.ceilingEntry(hash);
        if (entry == null) {
            entry = circle.firstEntry(); // Wrap around
        }
        
        return entry.getValue();
    }
    
    private int hash(String key) {
        return key.hashCode();
    }
}
```
 
**Use case:** Distributed caches (Redis, Memcached), CDN edge routing
 
### Load Balancing Algorithm Selection Guide
 
| Use Case | Algorithm | Why |
|----------|-----------|-----|
| Stateless REST APIs | Round Robin | Simple, fair, efficient |
| Heterogeneous servers | Weighted Round Robin | Account for capacity differences |
| WebSocket / Long connections | Least Connections | Handles long-lived connections |
| Need sticky sessions | IP Hash | Automatic affinity |
| Distributed cache | Consistent Hashing | Minimal cache invalidation |
| Auto-scaling | Least Response Time | Adapts to server performance |
 
### Interview Answer
 
**"What load balancing algorithm would you use for a Spring Boot API?"**
 
**"For a stateless Spring Boot REST API, I'd use **Round Robin** because:**
1. Requests have similar processing time (50-200ms)
2. Servers are identical (same instance type)
3. No session state to maintain
4. Simple and efficient
 
**If we had WebSocket connections** (e.g., chat app), I'd switch to **Least Connections** since connections are long-lived and vary in duration.
 
**If we needed sticky sessions** (legacy session-based app), I'd use **Consistent Hashing** over IP Hash because it minimizes session loss when servers scale up/down."
 
---

## 7. Session Management Strategies (5 minutes)
 
### The Session Problem
 
**Scenario:**
```
User logs in → Creates session on Server 1
Next request → Load balancer routes to Server 2
Server 2 doesn't have the session → User appears logged out ❌
```
 
### Solution 1: Sticky Sessions (Session Affinity)
 
**How it works:**
- Load balancer remembers which server handled user's first request
- All subsequent requests from that user go to same server
- Uses cookie or IP hash
 
**Implementation:**
```
Load Balancer sets cookie:
Set-Cookie: SERVER_ID=server_1
 
Client includes cookie in subsequent requests:
Cookie: SERVER_ID=server_1
 
Load balancer routes to Server 1
```
 
**Pros:**
- No code changes needed
- Fast (local memory)
- Simple
 
**Cons:**
- If server crashes → all its users lose sessions
- Uneven load distribution (hot users on one server)
- Not truly horizontally scalable
- Can't do zero-downtime deployments easily
 
**Use case:** Legacy apps during migration period
 
### Solution 2: Session Store (Recommended)
 
**How it works:**
- Store sessions in external shared storage (Redis, Memcached)
- Any server can access any session
- Session ID in cookie, data in Redis
 
**Spring Boot Implementation:**
 
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
 
```java
// Configuration
@Configuration
@EnableRedisHttpSession(maxInactiveIntervalInSeconds = 1800)
public class SessionConfig {
    
    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        RedisStandaloneConfiguration config = 
            new RedisStandaloneConfiguration("redis.example.com", 6379);
        return new LettuceConnectionFactory(config);
    }
}
 
// Controller (no changes needed!)
@RestController
public class UserController {
    
    @PostMapping("/login")
    public ResponseEntity<String> login(
            @RequestBody LoginRequest request,
            HttpSession session) {
        
        // Authenticate user
        User user = authService.authenticate(request.getUsername(), 
                                            request.getPassword());
        
        // Store in session (automatically goes to Redis)
        session.setAttribute("user", user);
        
        return ResponseEntity.ok("Logged in");
    }
    
    @GetMapping("/profile")
    public ResponseEntity<User> getProfile(HttpSession session) {
        // Retrieve from session (from Redis)
        User user = (User) session.getAttribute("user");
        
        if (user == null) {
            return ResponseEntity.status(401).build();
        }
        
        return ResponseEntity.ok(user);
    }
}
```
 
**How it works internally:**
1. Client makes request with session cookie
2. Spring Session intercepts
3. Fetches session data from Redis using session ID
4. Populates HttpSession object
5. Application code uses HttpSession normally
6. On session.setAttribute(), Spring Session writes to Redis
 
**Pros:**
- Truly horizontally scalable
- No sticky sessions needed
- Server can crash → session survives
- Zero-downtime deployments
- Load balancer can use any algorithm
 
**Cons:**
- Network latency (Redis call per request)
- Redis becomes single point of failure (mitigated with Redis Sentinel/Cluster)
- Slightly more complex setup
 
### Solution 3: JWT Tokens (Stateless)
 
**How it works:**
- No server-side session at all
- All user data in JWT token (client-side)
- Token sent with every request
 
```java
@RestController
public class UserController {
    
    @Autowired
    private JwtService jwtService;
    
    @PostMapping("/login")
    public ResponseEntity<LoginResponse> login(
            @RequestBody LoginRequest request) {
        
        // Authenticate
        User user = authService.authenticate(request.getUsername(), 
                                            request.getPassword());
        
        // Generate JWT
        String token = jwtService.generateToken(user);
        
        return ResponseEntity.ok(new LoginResponse(token));
    }
    
    @GetMapping("/profile")
    public ResponseEntity<User> getProfile(
            @RequestHeader("Authorization") String authHeader) {
        
        // Extract token: "Bearer <token>"
        String token = authHeader.substring(7);
        
        // Validate and extract user
        User user = jwtService.validateAndExtractUser(token);
        
        return ResponseEntity.ok(user);
    }
}
 
// JWT Service
@Service
public class JwtService {
    
    private String SECRET_KEY = "your-secret-key";
    
    public String generateToken(User user) {
        return Jwts.builder()
                .setSubject(user.getUsername())
                .claim("userId", user.getId())
                .claim("role", user.getRole())
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 3600000))
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
                .compact();
    }
    
    public User validateAndExtractUser(String token) {
        Claims claims = Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody();
        
        User user = new User();
        user.setId(claims.get("userId", Long.class));
        user.setUsername(claims.getSubject());
        user.setRole(claims.get("role", String.class));
        
        return user;
    }
}
```
 
**Pros:**
- Truly stateless (no session store needed)
- Horizontally scalable by default
- No Redis/database dependency for auth
- Works across domains (mobile app, web app)
 
**Cons:**
- Can't revoke tokens (until expiry)
- Token size (sent with every request)
- Security: token theft risk (use HTTPS, short expiry)
 
### Session Management Comparison
 
| Strategy | Scalability | Complexity | Performance | Failure Handling |
|----------|-------------|------------|-------------|------------------|
| **Sticky Sessions** | Limited | Low | Fast (local) | Session lost on crash |
| **Session Store (Redis)** | Excellent | Medium | Good (1-2ms latency) | Survives crashes |
| **JWT (Stateless)** | Excellent | Medium | Fast (no external call) | Always works |
 
### Interview Answer
 
**"How would you handle sessions in a horizontally scaled Spring Boot app?"**
 
**"I'd use Spring Session with Redis:**
 
1. **Why Redis**: Fast (in-memory), distributed, persistent
2. **How**: Add Spring Session dependency, configure Redis connection, enable @EnableRedisHttpSession
3. **Result**: HttpSession calls automatically stored in Redis, any server can access any session
4. **Trade-off**: 1-2ms latency per request vs local memory, but we gain horizontal scalability and fault tolerance
 
**Alternative for new projects**: JWT tokens for truly stateless design. No session store needed, but lose ability to revoke tokens immediately.
 
**I'd avoid sticky sessions** as they create single points of failure and complicate deployments."
 
---

## 8. Practical Exercise: Scale Spring Boot Monolith from 1 to 10 Servers

### UML:
<img width="533" height="646" alt="image" src="https://github.com/user-attachments/assets/96396b1e-f5ce-47a0-a2e9-f9ee9988f86b" />
<img width="539" height="113" alt="image" src="https://github.com/user-attachments/assets/9aa75ce0-59ad-4bc5-9e78-af0dd69ab82f" />

### Scenario
 
**You have:**
- 1 Spring Boot application (monolith)
- 1 MySQL database
- Handles 1000 req/s currently
- Need to scale to 10,000 req/s
 
**Requirements:**
- No downtime
- Session-based authentication (HttpSession)
- Some endpoints are read-heavy (product catalog)
- Some are write-heavy (order placement)
 
### Step-by-Step Solution
 
#### **Current Architecture (1 Server)**
 
```
Internet → Server 1 (Spring Boot + Tomcat)
              ↓
           MySQL Database
```
 
**Problems at scale:**
- Single point of failure
- CPU/Memory bottleneck
- No redundancy
 
---
 
#### **Step 1: Separate Database (Still 1 App Server)**
 
```
Internet → Server 1 (Spring Boot)
              ↓
           Server 2 (MySQL)
```
 
**Changes:**
- Move MySQL to dedicated server
- Update application.properties:
  ```properties
  spring.datasource.url=jdbc:mysql://db-server:3306/myapp
  ```
 
**Benefits:**
- App server has more resources (no DB competition)
- Can scale app and DB independently
- Can optimize DB server (more RAM, faster disk)
 
**Still limited:** Single app server bottleneck
 
---
 
#### **Step 2: Add Load Balancer + 2 App Servers (But Sessions Break!)**
 
```
                 ┌→ Server 1 (Spring Boot)
Internet → LB ──┤
                 └→ Server 2 (Spring Boot)
                       ↓
                   MySQL DB
```
 
**Problem:**
```
User logs in → Server 1 creates session
Next request → LB routes to Server 2
Server 2 doesn't have session → User logged out ❌
```
 
**Temporary Fix:** Sticky sessions on LB
- Not recommended (see Session Management section)
 
---
 
#### **Step 3: Externalize Sessions to Redis**
 
```
                 ┌→ Server 1 ────┐
Internet → LB ──┤                 ├→ Redis (sessions)
                 └→ Server 2 ────┘
                       ↓
                   MySQL DB
```
 
**Code Changes:**
 
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
```
 
```java
// SessionConfig.java
@Configuration
@EnableRedisHttpSession
public class SessionConfig {
    @Bean
    public LettuceConnectionFactory connectionFactory() {
        return new LettuceConnectionFactory("redis-host", 6379);
    }
}
```
 
**No controller changes needed!** Spring Session intercepts HttpSession.
 
**Benefits:**
- Sessions work across all servers
- Can scale to 10 servers with zero additional changes
- Servers are now stateless
 
---
 
#### **Step 4: Scale to 10 App Servers**
 
```
                 ┌→ Server 1 ────┐
                 ├→ Server 2 ─────┤
                 ├→ Server 3 ─────┤
Internet → LB ──┤  ...            ├→ Redis
                 ├→ Server 9 ─────┤
                 └→ Server 10 ────┘
                       ↓
                   MySQL DB
```
 
**Deployment:**
- Use AWS Auto Scaling Group or Kubernetes
- Add/remove servers based on CPU usage
- Round Robin load balancing
 
**Current capacity:** 10 × 1000 req/s = 10,000 req/s ✅
 
**Remaining bottleneck:** MySQL writes
 
---
 
#### **Step 5: Add Database Read Replicas (Read-heavy optimization)**
 
```
                 ┌→ Server 1 ────┐
                 ├→ Server 2 ─────┤
Internet → LB ──┤  ...            ├→ Redis
                 └→ Server 10 ────┘
                       ↓
                 MySQL Master (writes)
                       ↓
              ┌────────┴────────┐
              ↓                 ↓
        Read Replica 1    Read Replica 2
```
 
**Code Changes:**
 
```java
// application.properties
spring.datasource.hikari.maximum-pool-size=20
 
# Master (writes)
spring.datasource.master.url=jdbc:mysql://master-db:3306/myapp
 
# Replicas (reads)
spring.datasource.replica1.url=jdbc:mysql://replica1-db:3306/myapp
spring.datasource.replica2.url=jdbc:mysql://replica2-db:3306/myapp
```
 
```java
// DataSourceConfig.java
@Configuration
public class DataSourceConfig {
    
    @Bean
    @Primary
    public DataSource routingDataSource() {
        RoutingDataSource routing = new RoutingDataSource();
        
        Map<Object, Object> sources = new HashMap<>();
        sources.put("master", masterDataSource());
        sources.put("replica1", replica1DataSource());
        sources.put("replica2", replica2DataSource());
        
        routing.setTargetDataSources(sources);
        routing.setDefaultTargetDataSource(masterDataSource());
        
        return routing;
    }
    
    // Master, Replica1, Replica2 DataSource beans...
}
 
// Custom annotation for read-only queries
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface ReadOnlyQuery {
}
 
// AOP to route queries
@Aspect
@Component
public class DataSourceRoutingAspect {
    
    @Around("@annotation(ReadOnlyQuery)")
    public Object route(ProceedingJoinPoint joinPoint) throws Throwable {
        // Set thread-local to route to replica
        DataSourceContextHolder.set("replica" + (random() % 2 + 1));
        try {
            return joinPoint.proceed();
        } finally {
            DataSourceContextHolder.clear();
        }
    }
}
```
 
**Usage in Service:**
 
```java
@Service
public class ProductService {
    
    @Autowired
    private ProductRepository productRepository;
    
    // Writes go to master (default)
    public Product save(Product product) {
        return productRepository.save(product);
    }
    
    // Reads go to replicas
    @ReadOnlyQuery
    public List<Product> findAll() {
        return productRepository.findAll();
    }
    
    @ReadOnlyQuery
    public Product findById(Long id) {
        return productRepository.findById(id).orElse(null);
    }
}
```
 
**Benefits:**
- Read queries (90% of traffic) distributed across replicas
- Master only handles writes (10% of traffic)
- Read capacity tripled (1 master + 2 replicas)
 
**Bottleneck now:** Master writes (if >5K writes/s, need sharding)
 
---
 
#### **Step 6: Add Caching Layer (Redis for data, not just sessions)**
 
```
                 ┌→ Server 1 ────┐
                 ├→ Server 2 ─────┤
Internet → LB ──┤  ...            ├→ Redis (sessions + cache)
                 └→ Server 10 ────┘
                       ↓
                 MySQL Master
                       ↓
              ┌────────┴────────┐
              ↓                 ↓
        Read Replica 1    Read Replica 2
```
 
**Code Changes:**
 
```java
// Enable caching
@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory factory) {
        RedisCacheConfiguration config = RedisCacheConfiguration
                .defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(10));
        
        return RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
    }
}
 
// Service with caching
@Service
public class ProductService {
    
    @Cacheable(value = "products", key = "#id")
    public Product findById(Long id) {
        // Only hits DB if not in cache
        return productRepository.findById(id).orElse(null);
    }
    
    @CacheEvict(value = "products", key = "#product.id")
    public Product update(Product product) {
        // Invalidates cache on update
        return productRepository.save(product);
    }
}
```
 
**Benefits:**
- Hot products cached (Pareto: 20% products = 80% traffic)
- 95%+ cache hit rate
- DB load reduced dramatically
- Response time: 2ms (cache) vs 50ms (DB)
 
---
 
### Final Architecture (10 Servers, Scaled)
 
```
                            ┌→ App Server 1 ─┐
                            ├→ App Server 2 ──┤
                            ├→ App Server 3 ──┤
                            ├→ App Server 4 ──┤
Internet → Load Balancer ──┤  App Server 5 ──├→ Redis Cluster
                            ├→ App Server 6 ──┤  (sessions + cache)
                            ├→ App Server 7 ──┤
                            ├→ App Server 8 ──┤
                            ├→ App Server 9 ──┤
                            └→ App Server 10 ─┘
                                    ↓
                              MySQL Master
                              (writes only)
                                    ↓
                      ┌─────────────┴─────────────┐
                      ↓                           ↓
              Read Replica 1              Read Replica 2
              (reads only)                (reads only)
```
 
**Capacity:**
- **App layer**: 10 × 1000 req/s = 10,000 req/s
- **Cache layer**: 95% hit rate → only 500 req/s hit DB
- **DB layer**: 500 writes/s + 250 reads/s (5% miss rate) = 750 DB req/s (well within limits)
 
**Achieved:**
- ✅ 10,000 req/s capacity
- ✅ No single point of failure (redundant app servers, Redis Sentinel, DB replicas)
- ✅ Zero downtime deployments (rolling restart)
- ✅ Auto-scaling ready (add/remove app servers dynamically)
 
---
 
### Cost Breakdown (AWS Example)
 
```
Load Balancer (ALB):     $20/month
App Servers (10 × t3.medium): $300/month
Redis Cluster (r6g.large):    $100/month
MySQL Master (db.r5.xlarge):  $180/month
Read Replicas (2 × db.r5.large): $240/month
 
Total: $840/month for 10,000 req/s
Cost per request: $0.0000034
```
 
Compare to vertical scaling:
```
Single server (m5.8xlarge): $1,100/month
Capacity: ~3,000 req/s (CPU/memory bound)
More expensive AND lower capacity!
```
 
---
 
### Deployment Checklist
 
**Infrastructure:**
- [ ] Provision 10 EC2 instances (or Kubernetes pods)
- [ ] Set up Application Load Balancer
- [ ] Configure Redis Cluster with Sentinel (HA)
- [ ] Set up MySQL master + 2 read replicas
- [ ] Configure auto-scaling policies
 
**Application Changes:**
- [ ] Add Spring Session + Redis dependencies
- [ ] Externalize session configuration
- [ ] Add caching annotations
- [ ] Configure read/write data source routing
- [ ] Update connection pool sizes (HikariCP)
- [ ] Add health check endpoint (/actuator/health)
 
**Testing:**
- [ ] Load test with 10,000 req/s (use JMeter, Gatling)
- [ ] Verify session persistence across servers
- [ ] Test cache hit rates
- [ ] Simulate server failure (kill 1 server, verify no impact)
- [ ] Test rolling deployments
 
**Monitoring:**
- [ ] Set up CloudWatch / Prometheus metrics
- [ ] Alert on high CPU (>80%)
- [ ] Alert on high DB connections
- [ ] Monitor cache hit rate (should be >90%)
- [ ] Track response time P95, P99
 
---
 
## Summary: Key Takeaways
 
✅ **Vertical scaling**: Simple but limited. Good for early stage.  
✅ **Horizontal scaling**: Complex but infinite. Required at scale.  
✅ **Stateless services**: Essential for horizontal scaling.  
✅ **Session externalization**: Redis or JWT tokens.  
✅ **Load balancing**: Round Robin for stateless APIs.  
✅ **Consistent hashing**: For distributed caches.  
✅ **Read replicas**: Optimize read-heavy workloads.  
✅ **Caching**: Redis for hot data, 95%+ hit rate ideal.  
 
**Interview Tip**: When discussing scaling, always mention:
1. Current numbers (X req/s today)
2. Target numbers (Y req/s needed)
3. Bottleneck identification (CPU? DB? Memory?)
4. Solution with trade-offs
5. Cost implications
 
# Microservice Architecture

## 1. Microservices Fundamentals (5 minutes)
 
### What are Microservices?
 
**Microservices** = Architectural style where an application is built as a collection of small, independent services that:
- Run in their own processes
- Communicate via lightweight protocols (HTTP/REST, gRPC, message queues)
- Are independently deployable
- Are organized around business capabilities
- Can use different tech stacks
 
### Monolith vs Microservices
 
#### **Monolith Architecture**
```
┌─────────────────────────────────┐
│    Single Spring Boot App       │
│                                  │
│  ┌──────────────────────────┐  │
│  │   User Management        │  │
│  ├──────────────────────────┤  │
│  │   Product Catalog        │  │
│  ├──────────────────────────┤  │
│  │   Order Management       │  │
│  ├──────────────────────────┤  │
│  │   Payment Processing     │  │
│  ├──────────────────────────┤  │
│  │   Inventory              │  │
│  └──────────────────────────┘  │
│                                  │
│        Single Database           │
└─────────────────────────────────┘
```
 
**Characteristics:**
- One codebase, one deployment
- Shared database
- In-process function calls (fast)
- Simple to develop initially
 
#### **Microservices Architecture**
```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   User       │  │   Product    │  │   Order      │
│   Service    │  │   Service    │  │   Service    │
│              │  │              │  │              │
│   User DB    │  │  Product DB  │  │   Order DB   │
└──────────────┘  └──────────────┘  └──────────────┘
 
┌──────────────┐  ┌──────────────┐
│   Payment    │  │  Inventory   │
│   Service    │  │   Service    │
│              │  │              │
│  Payment DB  │  │ Inventory DB │
└──────────────┘  └──────────────┘
```
 
**Characteristics:**
- Multiple codebases, independent deployments
- Database per service
- Network calls (HTTP/gRPC/messaging)
- Complex to develop, easier to scale
 
### When to Use Microservices
 
✅ **Good for:**
- **Large teams** (50+ engineers): Teams can work independently
- **Complex domains**: Different business capabilities need different tech
- **Rapid scaling requirements**: Scale services independently
- **Frequent deployments**: Deploy one service without touching others
- **Different SLA requirements**: Critical services get more resources
- **Long-term product**: Will evolve over years
 
❌ **Bad for (Use Monolith Instead):**
- **Small teams** (<10 engineers): Overhead not worth it
- **MVPs / Startups**: Need speed, not complexity
- **Simple domains**: CRUD apps don't need microservices
- **Tight coupling**: If services call each other constantly, monolith is better
- **No DevOps expertise**: Need strong ops team for microservices
 
### The Monolith-First Strategy
 
**Martin Fowler's Advice:**
> "Almost all the successful microservice stories have started with a monolith that got too big and was broken up."
 
**Recommended Path:**
1. Start with well-designed modular monolith
2. Identify seams (module boundaries)
3. Extract services when:
   - Team size grows (>10 people)
   - Deployment becomes painful (>1 hour)
   - Parts need independent scaling
   - Clear service boundaries emerge
 
**Anti-Pattern:** "Microservices from day 1" for a new product
- No idea where boundaries should be
- Premature optimization
- Distributed monolith (worst of both worlds)
 
---

## 2. Service Decomposition Strategies (8 minutes)
 
### How to Break a Monolith?
 
**Three Main Strategies:**
 
#### **Strategy 1: Decompose by Business Capability**
 
**Definition:** Group related business functions together
 
**Example: E-commerce**
```
Business Capabilities:
- User Management (authentication, profiles)
- Product Catalog (browse, search)
- Order Management (cart, checkout)
- Payment Processing (transactions)
- Inventory Management (stock, warehousing)
- Shipping (tracking, delivery)
- Notification (email, SMS)
```
 
**Benefits:**
- Aligns with organization structure (Conway's Law)
- Stable boundaries (business capabilities change slowly)
- Clear ownership (one team per capability)
 
**How to Identify:**
- Talk to business stakeholders
- Map business processes
- Look for nouns in domain language
 
#### **Strategy 2: Decompose by Subdomain (Domain-Driven Design)**
 
**Definition:** Use DDD bounded contexts
 
**DDD Concepts:**
- **Bounded Context**: A boundary within which a model is defined
- **Ubiquitous Language**: Shared vocabulary within a context
- **Aggregate**: Cluster of objects treated as a unit
 
**Example: E-commerce**
```
Core Domains (differentiators):
- Product Recommendation Engine
- Pricing & Promotions
- Order Fulfillment
 
Supporting Domains:
- User Authentication
- Payment Processing (use Stripe)
 
Generic Domains:
- Notification (use SendGrid)
```
 
**Benefits:**
- Focus on core business value
- Buy vs Build decisions clear
- Boundaries based on domain expertise
 
#### **Strategy 3: Decompose by Transaction / Use Case**
 
**Definition:** Extract services based on user journeys
 
**Example: E-commerce Checkout**
```
Checkout Transaction involves:
1. Validate cart items (Product Service)
2. Check inventory (Inventory Service)
3. Calculate price (Pricing Service)
4. Process payment (Payment Service)
5. Create order (Order Service)
6. Reserve inventory (Inventory Service)
7. Send confirmation (Notification Service)
```
 
**This helps identify:**
- Service interactions
- Transaction boundaries
- Potential bottlenecks
 
### Decomposition Principles
 
#### **1. Single Responsibility Principle (SRP)**
Each service should have one reason to change.
 
```
❌ Bad: "User Service" handles auth, profiles, preferences, avatars, activity logs
✅ Good: 
   - Auth Service: authentication, tokens
   - Profile Service: user data, preferences
   - Activity Service: user actions, logs
```
 
#### **2. Loose Coupling**
Services should be independent.
 
```
❌ Bad: Order Service calls Product Service for every field
       → 10 API calls to display one order
 
✅ Good: Order Service stores product snapshot
       → Self-contained, only 1 API call
```
 
#### **3. High Cohesion**
Related functionality stays together.
 
```
❌ Bad: Payment validation in Order Service
       Payment processing in Payment Service
       → Split responsibility
 
✅ Good: All payment logic in Payment Service
```
 
#### **4. Database per Service**
Each service owns its data.
 
```
❌ Bad: Order Service and Inventory Service share same database
       → Tight coupling via schema
 
✅ Good: Order DB separate from Inventory DB
       → Communicate via APIs
```
 
### Common Decomposition Mistakes
 
**Mistake 1: Too Granular (Nano-services)**
```
❌ UserService, UserProfileService, UserPreferenceService, UserAvatarService
   → Too chatty, high latency
 
✅ Single User Service with all user-related functionality
```
 
**Mistake 2: Too Coarse (Distributed Monolith)**
```
❌ Frontend Service (calls Backend Service for everything)
   Backend Service (calls Database Service)
   → Still a monolith, just distributed
 
✅ Properly decomposed by business capability
```
 
**Mistake 3: Ignoring Data Consistency**
```
❌ Extract Inventory Service but Order Service still updates inventory table directly
   → Broken encapsulation
 
✅ Order Service calls Inventory API to reserve stock
```
 
---
 
## 3. API Gateway Pattern (5 minutes)
 
### The Problem
 
**Without API Gateway:**
```
Mobile App ──┬→ User Service (auth)
             ├→ Product Service (catalog)
             ├→ Order Service (cart)
             ├→ Payment Service (checkout)
             └→ Shipping Service (tracking)
```
 
**Issues:**
- Client must know all service URLs
- Many network calls from client (slow on mobile)
- Authentication logic duplicated in every service
- CORS, rate limiting in every service
- Tight coupling (client knows internal architecture)
 
### The Solution: API Gateway
 
**With API Gateway:**
```
Mobile App → API Gateway ──┬→ User Service
                           ├→ Product Service
                           ├→ Order Service
                           ├→ Payment Service
                           └→ Shipping Service
```
 
**Gateway handles:**
- **Routing**: Maps client requests to backend services
- **Aggregation**: Combines responses from multiple services
- **Authentication**: JWT validation, OAuth
- **Rate Limiting**: Throttle requests
- **Caching**: Cache responses
- **Load Balancing**: Distribute load
- **Protocol Translation**: REST to gRPC
 
### Implementation: Spring Cloud Gateway
 
**Dependencies:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```
 
**Configuration:**
```yaml
# application.yml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service  # Load balanced via Eureka
          predicates:
            - Path=/api/users/**
          filters:
            - RewritePath=/api/users/(?<segment>.*), /${segment}
            - name: RequestRateLimiter
              args:
                redis-rate-limiter.replenishRate: 10
                redis-rate-limiter.burstCapacity: 20
        
        - id: product-service
          uri: lb://product-service
          predicates:
            - Path=/api/products/**
          filters:
            - RewritePath=/api/products/(?<segment>.*), /${segment}
        
        - id: order-service
          uri: lb://order-service
          predicates:
            - Path=/api/orders/**
          filters:
            - RewritePath=/api/orders/(?<segment>.*), /${segment}
 
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```
 
**Custom Filter (JWT Authentication):**
```java
@Component
public class AuthenticationFilter implements GatewayFilter {
    
    @Autowired
    private JwtUtil jwtUtil;
    
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, 
                             GatewayFilterChain chain) {
        
        ServerHttpRequest request = exchange.getRequest();
        
        // Skip auth for login/register
        if (isAuthMissing(request)) {
            return onError(exchange, "Authorization header missing", 
                          HttpStatus.UNAUTHORIZED);
        }
        
        String token = getAuthHeader(request);
        
        if (!jwtUtil.isValid(token)) {
            return onError(exchange, "Invalid token", 
                          HttpStatus.UNAUTHORIZED);
        }
        
        // Add user info to headers for downstream services
        populateHeaders(exchange, token);
        
        return chain.filter(exchange);
    }
    
    private boolean isAuthMissing(ServerHttpRequest request) {
        return !request.getHeaders().containsKey("Authorization");
    }
    
    private String getAuthHeader(ServerHttpRequest request) {
        return request.getHeaders()
                .getOrEmpty("Authorization")
                .get(0)
                .substring(7); // Remove "Bearer "
    }
    
    private void populateHeaders(ServerWebExchange exchange, String token) {
        String userId = jwtUtil.extractUserId(token);
        exchange.getRequest().mutate()
                .header("X-User-Id", userId)
                .build();
    }
}
```
 
**Response Aggregation:**
```java
@RestController
@RequestMapping("/api/aggregated")
public class AggregationController {
    
    @Autowired
    private WebClient.Builder webClient;
    
    @GetMapping("/dashboard")
    public Mono<DashboardResponse> getDashboard(
            @RequestHeader("X-User-Id") String userId) {
        
        // Call multiple services in parallel
        Mono<User> userMono = webClient.build()
                .get()
                .uri("http://user-service/users/" + userId)
                .retrieve()
                .bodyToMono(User.class);
        
        Mono<List<Order>> ordersMono = webClient.build()
                .get()
                .uri("http://order-service/orders?userId=" + userId)
                .retrieve()
                .bodyToFlux(Order.class)
                .collectList();
        
        Mono<Cart> cartMono = webClient.build()
                .get()
                .uri("http://cart-service/cart?userId=" + userId)
                .retrieve()
                .bodyToMono(Cart.class);
        
        // Combine responses
        return Mono.zip(userMono, ordersMono, cartMono)
                .map(tuple -> new DashboardResponse(
                    tuple.getT1(), // user
                    tuple.getT2(), // orders
                    tuple.getT3()  // cart
                ));
    }
}
```
 
### Gateway Patterns
 
**Pattern 1: Backends for Frontends (BFF)**
```
Mobile App    → Mobile Gateway    → Services
Web App       → Web Gateway       → Services
Partner API   → Partner Gateway   → Services
```
 
Each client type gets optimized gateway.
 
**Pattern 2: Service Mesh (Advanced)**
```
Services communicate via sidecar proxies (Istio, Linkerd)
- Service-to-service auth
- Traffic management
- Observability
```
 
---

## 4. Service Discovery (Eureka) (4 minutes)
 
### The Problem
 
**Hardcoded URLs:**
```java
// Order Service needs to call Inventory Service
String inventoryUrl = "http://192.168.1.50:8082/api/inventory/check";
RestTemplate rest = new RestTemplate();
rest.getForObject(inventoryUrl, InventoryResponse.class);
```
 
**Issues:**
- IP changes → Code change needed
- Multiple instances → How to load balance?
- Instance crashes → Need to update URLs
- Dynamic scaling → New instances not discovered
 
### The Solution: Service Discovery
 
**Service Registry (Eureka Server):**
```
1. Services register themselves on startup
2. Send heartbeats every 30 seconds
3. Registry removes dead services
4. Clients query registry to find services
```
 
### Implementation: Netflix Eureka
 
#### **Eureka Server**
 
**Dependencies:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```
 
**Application:**
```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```
 
**Configuration:**
```yaml
# application.yml
server:
  port: 8761
 
eureka:
  client:
    register-with-eureka: false  # Don't register itself
    fetch-registry: false
  server:
    enable-self-preservation: false  # For dev only
```
 
#### **Eureka Client (Service)**
 
**Dependencies:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```
 
**Application:**
```java
@SpringBootApplication
@EnableDiscoveryClient
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}
```
 
**Configuration:**
```yaml
# application.yml
spring:
  application:
    name: order-service  # Service name in registry
 
server:
  port: 8080
 
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true
    lease-renewal-interval-in-seconds: 30  # Heartbeat
```
 
#### **Service-to-Service Communication**
 
**Option 1: RestTemplate with @LoadBalanced**
```java
@Configuration
public class RestTemplateConfig {
    
    @Bean
    @LoadBalanced  // Enable load balancing via Eureka
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
 
@Service
public class OrderService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    public void createOrder(Order order) {
        // Use service name, not IP
        String url = "http://inventory-service/api/inventory/reserve";
        
        InventoryRequest request = new InventoryRequest(
            order.getProductId(), order.getQuantity()
        );
        
        InventoryResponse response = restTemplate.postForObject(
            url, request, InventoryResponse.class
        );
        
        // Process response...
    }
}
```
 
**Option 2: WebClient (Reactive)**
```java
@Configuration
public class WebClientConfig {
    
    @Bean
    @LoadBalanced
    public WebClient.Builder webClientBuilder() {
        return WebClient.builder();
    }
}
 
@Service
public class OrderService {
    
    @Autowired
    private WebClient.Builder webClientBuilder;
    
    public Mono<OrderResponse> createOrder(Order order) {
        return webClientBuilder.build()
                .post()
                .uri("http://inventory-service/api/inventory/reserve")
                .bodyValue(new InventoryRequest(order.getProductId(), 
                                               order.getQuantity()))
                .retrieve()
                .bodyToMono(InventoryResponse.class)
                .map(inventoryResponse -> {
                    // Process and create order
                    return new OrderResponse(order.getId(), "CREATED");
                });
    }
}
```
 
**Option 3: Feign Client (Declarative)**
```java
@FeignClient(name = "inventory-service")
public interface InventoryClient {
    
    @PostMapping("/api/inventory/reserve")
    InventoryResponse reserveStock(@RequestBody InventoryRequest request);
    
    @GetMapping("/api/inventory/check/{productId}")
    InventoryResponse checkStock(@PathVariable Long productId);
}
 
@Service
public class OrderService {
    
    @Autowired
    private InventoryClient inventoryClient;
    
    public void createOrder(Order order) {
        InventoryRequest request = new InventoryRequest(
            order.getProductId(), order.getQuantity()
        );
        
        InventoryResponse response = inventoryClient.reserveStock(request);
        
        if (response.isAvailable()) {
            // Create order
        }
    }
}
```
 
### Service Discovery Alternatives
 
| Tool | Type | Features |
|------|------|----------|
| **Eureka** | Application-level | Spring ecosystem, easy setup |
| **Consul** | Platform-level | Health checks, KV store, multi-DC |
| **Zookeeper** | Platform-level | Used by Kafka, HBase |
| **Kubernetes** | Platform-level | Built-in (Service + DNS) |
 
---

## 5. Circuit Breaker (Resilience4j) (5 minutes)
 
### The Problem
 
**Cascading Failures:**
```
Order Service → Payment Service (down/slow)
                ↓
Order Service threads blocked waiting
                ↓
Order Service becomes slow
                ↓
API Gateway times out
                ↓
Entire system appears down
```
 
One failing service brings down the whole system!
 
### The Solution: Circuit Breaker
 
**Circuit Breaker States:**
```
CLOSED (Normal)
   ↓ (errors > threshold)
OPEN (Reject requests immediately)
   ↓ (after wait duration)
HALF_OPEN (Try a few requests)
   ↓ (if success)
CLOSED
```
 
**Visual:**
```
CLOSED → All requests flow through
         If 50% fail in 10 seconds → OPEN
 
OPEN → Reject immediately (fail fast)
       Wait 60 seconds → HALF_OPEN
 
HALF_OPEN → Allow 3 test requests
            If success → CLOSED
            If fail → OPEN
```
 
### Implementation: Resilience4j
 
**Dependencies:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
 
**Configuration:**
```yaml
# application.yml
resilience4j:
  circuitbreaker:
    instances:
      paymentService:
        register-health-indicator: true
        sliding-window-size: 10  # Last 10 calls
        minimum-number-of-calls: 5  # Need 5 calls before decision
        permitted-number-of-calls-in-half-open-state: 3
        wait-duration-in-open-state: 60s
        failure-rate-threshold: 50  # 50% failure rate → OPEN
        slow-call-duration-threshold: 2s
        slow-call-rate-threshold: 50  # 50% slow calls → OPEN
 
management:
  endpoints:
    web:
      exposure:
        include: health,circuitbreakers
  health:
    circuitbreakers:
      enabled: true
```
 
**Service Implementation:**
```java
@Service
public class OrderService {
    
    @Autowired
    private PaymentClient paymentClient;
    
    @Autowired
    private CircuitBreakerFactory circuitBreakerFactory;
    
    public OrderResponse createOrder(Order order) {
        CircuitBreaker circuitBreaker = 
            circuitBreakerFactory.create("paymentService");
        
        // Try payment with circuit breaker
        PaymentResponse payment = circuitBreaker.run(
            () -> paymentClient.processPayment(order.getPayment()),
            throwable -> handlePaymentFailure(order, throwable)
        );
        
        if (payment.isSuccess()) {
            return new OrderResponse(order.getId(), "CONFIRMED");
        } else {
            return new OrderResponse(order.getId(), "PENDING_PAYMENT");
        }
    }
    
    private PaymentResponse handlePaymentFailure(Order order, 
                                                 Throwable error) {
        // Fallback logic
        log.error("Payment failed for order: " + order.getId(), error);
        
        // Option 1: Queue for retry
        paymentRetryQueue.add(order);
        
        // Option 2: Return partial success
        return new PaymentResponse(false, "Payment service unavailable");
    }
}
```
 
**Using Annotations:**
```java
@Service
public class OrderService {
    
    @Autowired
    private PaymentClient paymentClient;
    
    @CircuitBreaker(name = "paymentService", fallbackMethod = "paymentFallback")
    @Retry(name = "paymentService", fallbackMethod = "paymentFallback")
    @RateLimiter(name = "paymentService")
    public PaymentResponse processPayment(PaymentRequest request) {
        return paymentClient.processPayment(request);
    }
    
    // Fallback method (same signature + Throwable)
    private PaymentResponse paymentFallback(PaymentRequest request, 
                                           Throwable throwable) {
        log.error("Payment circuit breaker activated", throwable);
        return new PaymentResponse(false, "Service temporarily unavailable");
    }
}
```
 
**Retry Configuration:**
```yaml
resilience4j:
  retry:
    instances:
      paymentService:
        max-attempts: 3
        wait-duration: 1s
        retry-exceptions:
          - java.net.ConnectException
          - java.io.IOException
        ignore-exceptions:
          - com.example.BusinessException
```
 
### Other Resilience Patterns
 
**1. Bulkhead (Isolation)**
```java
@Bulkhead(name = "paymentService", type = Bulkhead.Type.THREADPOOL)
public PaymentResponse processPayment(PaymentRequest request) {
    // Isolated thread pool (doesn't block other services)
    return paymentClient.processPayment(request);
}
```
 
**2. Rate Limiter**
```java
@RateLimiter(name = "paymentService")
public PaymentResponse processPayment(PaymentRequest request) {
    // Max 10 requests per second
    return paymentClient.processPayment(request);
}
```
 
**3. Timeout**
```java
@TimeLimiter(name = "paymentService")
public CompletableFuture<PaymentResponse> processPayment(
        PaymentRequest request) {
    return CompletableFuture.supplyAsync(() -> 
        paymentClient.processPayment(request)
    );
}
```
 
### Interview Answer
 
**"How do you handle service failures in microservices?"**
 
**"I use the Circuit Breaker pattern with Resilience4j:**
 
1. **Detect failures**: Monitor error rate (e.g., 50% failures in 10 calls)
2. **Open circuit**: Stop calling failing service, fail fast
3. **Fallback**: Return cached data or degraded response
4. **Half-open**: After cooldown, try test requests
5. **Close circuit**: If recovered, resume normal flow
 
**Example**: If Payment Service is down, Order Service:
- Opens circuit (stops calling)
- Returns "Payment pending" to user
- Queues order for retry
- User experience: Slightly degraded but system still works
 
**Benefits**: Prevents cascading failures, improves system resilience."
 
---

