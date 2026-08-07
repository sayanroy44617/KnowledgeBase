# 100 Micro Learning Topics for Backend Development

## Fundamentals
1. **How HTTP request-response cycle works**
   -  Client sends a formatted text message over TCP → server reads it, does work, sends a formatted text message back.
2. HTTP vs HTTPS — what TLS actually does
   - TTP is like sending a postcard where anyone can read the text; HTTPS is like sending a locked safe that only the recipient can open.
   - The "S" adds TLS encryption, which scrambles your data and verifies that the website is actually who it claims to be.
3. Status codes — when to use 400 vs 422 vs 409
   - **400 Bad Request**: Syntax Error — The JSON is malformed or data types are wrong (e.g., sending a string instead of an integer).
   - **422 Unprocessable Entity**: Semantic Error — The JSON is valid, but the data fails business rules (e.g., using an ArtifactType not in your Enum). 
   - **409 Conflict**: State Error — The request is valid, but it clashes with data already in the DB (e.g., trying to POST a duplicate URI).
4. Idempotency — what it means and why it matters in APIs
   - **Idempotency**: A property where performing an operation multiple times has the same result as performing it once.
   - **Why it matters**: It prevents duplicate data and allows you to safely restart or retry a failed sync script without side effects.
5. REST constraints — the 6 principles explained simply
   - **Client-Server**: the client handles the ui and user / the server handles the business logic.
   - **Stateless**: the server does not store any state.
   - **Cacheable**: responses must define themselves as cacheable or not.
   - **Uniform Interface**: the server must provide a consistent interface for all clients.
   - **Layered System**: the server must be composed of loosely coupled layers.
   - **Code on Demand (optional)**: the server can dynamically generate code on demand.
6. Stateless vs stateful services
   - **stateful**: The server stores information about the client's session (e.g., shopping cart contents, user login state). This can lead to scalability issues because the server has to manage and remember each client's state.
   - **stateless**: The server does not store any information about the client's session. Each request from the client must contain all the information needed to process it (e.g., using JWT tokens for authentication).
7. Synchronous vs asynchronous communication
   - Most real systems use hybrid:
     - Sync for user-facing request
     - - Async for background processing
8. What happens when you type a URL and hit Enter
9. TCP vs UDP — when each is used in backend systems
   - Need guaranteed delivery / transactions → TCP 
   - Can tolerate loss + need low latency → UDP
10. DNS resolution — how hostnames become IP addresses

## APIs
1. REST vs GraphQL vs gRPC — when to use each
    - REST : 
      - **Best for:** Public APIs and simple CRUD apps.
         * **Pros:** Highly cacheable, stateless, and universally compatible.
         * **Cons:** Over-fetching or under-fetching of data. 
    - GraphQL :
      - **Best for:** Complex Frontends and Mobile Apps.
        * **Pros:** Client defines exact data shape; single request for nested data.
        * **Cons:** Server-side complexity; harder to cache.
    - gRPC :
      - **Best for:** Internal Microservices and high-performance systems.
        * **Pros:** Extremely fast (Binary/HTTP2); strict contracts; supports streaming.
        * **Cons:** Poor browser support; not human-readable.
        
    - **Comparison Example (Fetching User & Posts)**

      - **REST:** - `GET /users/1`
          - `GET /users/1/posts`

      - **GraphQL:**
      ```graphql
      {
        user(id: "1") {
          name
          posts { title }
        }
      }
      ```
2. API versioning strategies — URI vs header vs query param
   - API versioning helps you evolve your API **without breaking existing clients**.

   - **URI Versioning**
     - Example: `/api/v1/users`
     - **Pros:**
       - Clear and explicit
       - Easy to test and document
       - Commonly used
     - **Cons:**
       - Version becomes part of the URL
       - Can clutter endpoints
     - **Best for:** Public APIs

   - **Header Versioning**
     - Example: `Accept: application/vnd.myapi.v2+json`
     - **Pros:**
       - Keeps URLs clean
       - More REST-oriented
       - Good separation of concerns
     - **Cons:**
       - Harder to test/debug manually
       - Less obvious to API consumers
     - **Best for:** Mature or enterprise APIs

   - **Query Parameter Versioning**
     - Example: `/api/users?version=2`
     - **Pros:**
       - Easy to implement
       - Simple for internal use
     - **Cons:**
       - Less clean and structured
       - Not ideal for long-term public APIs
     - **Best for:** Internal tools / lightweight APIs

   - **Rule of thumb:**
     - Public API → URI versioning
     - Enterprise / cleaner design → Header versioning
     - Quick internal use → Query param versioning

   - **Key principle:** Good API versioning is about **backward compatibility** and **not breaking consumers**.
3. Pagination — offset vs cursor-based
4. Rate limiting — token bucket vs leaky bucket algorithms
   - **Token Bucket** allows requests as long as tokens are available, so it supports **short bursts** while still controlling the average rate.
   - **Leaky Bucket** processes requests at a **fixed rate**, smoothing traffic and preventing sudden spikes from overwhelming the system.
5. API Gateway — what it does and when you need one
   - Client shouldn;t be exposed to all the complexity of your microservices. An API Gateway acts as a single entry point that can handle:
     - **Routing**: Directing requests to the appropriate service.
     - **Authentication**: Verifying client credentials before forwarding requests.
     - **Rate Limiting**: Controlling traffic to prevent abuse.
     - **Load Balancing**: Distributing requests across multiple instances of a service.
     - **Response Aggregation**: Combining responses from multiple services into one.
6. Webhooks vs polling — tradeoffs
   - **Polling** means a client repeatedly checks another system for updates. It is **simple** but can be **wasteful and delayed**.
   - **Webhooks** mean the server pushes an update to your system when an event happens. They are **more efficient and near real-time**, but harder to implement reliably.
   - **Rule of thumb:**  
     - Use **Polling** for simplicity.  
     - Use **Webhooks** for event-driven, timely updates.
7. HATEOAS — what it is and why most people skip it
8. OpenAPI/Swagger — writing good API specs
9. Request validation — where to do it and why
   - Request Validation
        - **API layer:** Validate structure, types, required fields (fail fast)
        - **Service layer:** Validate business rules (domain logic)
        - **DB layer:** Enforce integrity (constraints, uniqueness)
**Rule of thumb:**  
Validate early, validate deeply, enforce at the database.
10. API authentication — API keys vs OAuth vs JWT
    - **API Keys:** Simple token to identify the calling application. Easy but limited security and no user context.
    - **JWT (JSON Web Token):** Signed token carrying user/service identity and claims. Stateless and scalable.
    - **OAuth:** Authorization framework for delegated access (e.g., "Login with Google"). Issues tokens (often JWTs).
    - **Rule of thumb:**
      - API Keys → simple app access (Internal service-to-service)
      - JWT → user/service authentication (User login system , Microservices with user context)
      - OAuth → delegated authorization (asking an app access to a user's data)
## Databases
21. **ACID properties** — what each one means with examples
    - **atomicity** : everything that is part of the transaction must succeed or fail together
    - **consistency** : if any part of the transaction fails, the whole transaction should fail and the database should be left unchanged
    - **isolation** : no other process should influence any other process 
    - **durability** : once a transaction is committed, it should be permanent even in the case of a crash
22. **CAP theorem** — why you can only pick two
    - Partition Tolerance is inavoidable in distributed systems, so you have to choose between Consistency and Availability.
    - a x b (suppose partition tolerance is a given)
    - Write happens on a and make it 100 from 50.
    - Now read happens in b  
      - now if you choose consistency b will reject the request because it doesn't know A (this is not available)
      - but if you choose availability, b will return 50 (but this is not consistent).
23. Indexing — how B-tree indexes work
    - B-tree = wide, shallow, sorted tree optimized for fast disk-based lookups and range scans
    - It's self sorted , grows in width not height , held multiple keys 
    - [10,15] -> a node , if anything searchable is there , it goes in the middle
24. Query optimization — reading EXPLAIN output
    - EXPLAIN shows how the database plans to execute your query. You read it to detect inefficiencies before running the query on large data.
      - Core idea 
        - You are not reading results. You are reading the execution plan:
        - which table is read first 
        - how rows are filtered 
        - whether indexes are used and estimated cost / rows scanned
      
        - Avoid Breaking Index 
          - Bad: 
                ```YEAR(date) = 2024 LIKE '%abc'```
          - Good:
          ```date BETWEEN ... LIKE 'abc%'```

25. N+1 query problem — how to detect and fix it
    - n+1 query is a query that fetches the same data for multiple things rather than one fat big query.
    - rather than having ```select * from users```, you have ```select * from users where id = $id```
    - it calls the database n times for n users, which is inefficient.
26. Database normalization — 1NF, 2NF, 3NF explained simply
27. **Transactions** — isolation levels and what dirty reads are
    - **Transaction**: A group of DB operations where all succeed or all fail (ACID).
    - **Isolation**: Controls how transactions see each other and prevents inconsistent reads.
    - **Isolation Levels (low → high):**
      - Read Uncommitted
      - Read Committed
      - Repeatable Read
      - Serializable
    - **Dirty Read**: Reading uncommitted data from another transaction. If the other transaction rolls back → data was invalid.
    - **Key**: Dirty reads allowed only in Read Uncommitted isolation level.
28. Connection pooling — why you need it and how it works
    #### Problem
    - Creating DB connection is expensive (time + resources)
      - Doing it per request → slow + high overhead
    #### Solution: Connection Pool
    - Pre-create a pool of connections
      - Reuse them instead of creating new ones

    #### How it works
    1. App asks for connection
       2. Pool gives an **available connection**
       3. App uses it
       4. App returns it to pool (not closed)

    #### Benefits
    - Faster (no creation cost each time)
      - Limits max connections (protect DB)
      - Better performance under load

    #### Key Params
    - max connections
      - idle timeout
      - connection timeout

    #### One-line
    Reuse DB connections instead of creating every time
29. SQL vs NoSQL — choosing the right tool
30. Database migrations — best practices and rollback strategies
    - expand the table add columns but dont remove anything
    - make the app talk and write to both the new and the old table
    - backfill the new table with data from the old table
    - make the app read only from the new table
    - remove the old table

## Caching
31. Cache-aside vs write-through vs write-behind patterns
    - **cache aside** : talk to redis first, if not found then talk to db and update redis (lazy loading) , cache is there as a diff layer
    - **Read through** : redis keeps the responisbility of talking to db and updating itself, app only talks to redis (redis is the source of truth)
    - **write behind** : write operation is done in redis first and then redis updates the db asynchronously (redis is the source of truth)
32. Cache invalidation — why it's hard and common strategies
    - Challenges:
      - Stale data: Cached data may become outdated if the underlying data changes.
      - Concurrency: Multiple processes may update the same data, leading to inconsistencies between cache and database.
      - cache miss penalty: If the cache is invalidated too frequently, it can lead to increased load on the database.
    - Solutions:
      - explicit invalidation: When the underlying data changes, explicitly remove or update the corresponding cache entry.
      - write through caching: Update the cache immediately when the underlying data changes, ensuring consistency.
      - cache versioning: Use version numbers or timestamps to track changes in the underlying data, allowing the cache to determine if it is still valid.
      - event driven invalidation: Use events or notifications to trigger cache invalidation when the underlying data changes.
33. Redis data structures — strings, hashes, sets, sorted sets

    **Strings**
    
    * Single value (string/number/binary)
      * Use: cache, counters
      * Ops: `SET`, `GET`, `INCR`
      * Time: O(1)
    
    **Hashes**
    
    * Field → value map
      * Use: objects (user, config)
      * Ops: `HSET`, `HGET`, `HGETALL`
      * Time: O(1) per field
    
    **Sets**
    
    * Unique, unordered elements
      * Use: dedup, membership
      * Ops: `SADD`, `SISMEMBER`, `SINTER`
      * Time: O(1)
    
    **Sorted Sets (ZSET)**
    
    * Unique elements + score (ordered)
      * Use: leaderboard, ranking, timeline
      * Ops: `ZADD`, `ZRANGE`, `ZINCRBY`
      * Time: O(log N)
    
    **Selection**
    
    * Value → String
      * Object → Hash
      * Unique no order → Set
      * Ordered/ranked → Sorted Set

34. TTL strategies — how to set expiry times
35. Cache stampede — what it is and how to prevent it
    - when an important cache expires and when a lot of requests come in at the same time, it can cause a thundering herd problem. To avoid this, you can use a **randomized TTL** strategy:
    - Instead of setting a fixed TTL (e.g., 60 seconds), add a random offset (e.g., 60-120 seconds). This spreads out the expiration times and reduces the chance of many clients hitting the database simultaneously.
36. CDN caching — how edge caching works
    - technique to cache content at edge (closest node to the user) to reduce latency and improve performance. CDNs store copies of static assets (images, videos, scripts) in multiple locations worldwide. When a user requests content, the CDN serves it from the nearest edge server, reducing load on the origin server and speeding up delivery.
    - rather than every request going to the origin server, the CDN caches the content at edge servers. When a user requests content, the CDN serves it from the nearest edge server, reducing latency and improving performance.
    - request -> cdn server --> hit cache? yes -> return content, no -> fetch from origin server, cache it, return content
37. HTTP caching headers — Cache-Control, ETag, Last-Modified
38. Memoization vs caching — the difference
    - memorization is a technique to optimize function calls by storing the results of expensive function calls and returning the cached result when the same inputs occur again. It is typically used for pure functions (functions that always produce the same output for the same input and have no side effects). Memoization is usually implemented in-memory and is specific to a single process or instance of an application.
39. Distributed cache vs local cache tradeoffs
    - **Local Cache**: Stored in the memory of a single application instance. Fast access, but not shared across instances. Good for small-scale apps or when data is specific to a single instance.
    - **Distributed Cache**: Shared across multiple application instances, often using external systems like Redis or Memcached. Slower than local cache due to network latency, but allows for data sharing and consistency across instances. Suitable for large-scale applications with multiple servers.
40. When NOT to cache — cases where caching hurts

## Authentication & Security
41. JWT structure — header, payload, signature decoded
42. OAuth 2.0 flows — authorization code vs client credentials
    - auth code -> user logs in, gets code, exchanges for token
    - client credentials -> service logs in with its own credentials, gets token
43. Session vs token authentication — tradeoffs
    - Session: Server stores authentication state; client sends a session ID/cookie. Easy revocation, but requires shared session storage when scaling.
    - Token: Client sends an access token (often JWT); server can validate it without session state. Scales easily, but revocation is harder.
44. Password hashing — bcrypt vs Argon2 vs PBKDF2
    - Password hashing is used because passwords should never be stored as plaintext.
        bcrypt: Proven, widely supported, intentionally slow; good default for many systems.
        Argon2: Modern choice, designed to resist GPU/memory-based attacks; generally preferred for new systems.
        PBKDF2: Older, standardized, widely available; still useful where compatibility/compliance requires it.
    Preference: Argon2 > bcrypt > PBKDF2 for a new application, assuming your platform supports them properly.
45. SQL injection — how it works and how parameterized queries prevent it
    - Think of SQL injection as the application accidentally allowing the user to write part of the SQL command.
    - **Unsafe example**```sqlquery = "SELECT * FROM users WHERE username = '" + username + "'"```→ input ```sql''OR '1'='1``` can turn it into ```WHERE username = '' OR '1'='1'.```which is always true
    - **Safe example**```query = "SELECT * FROM users WHERE username = ?" with execute(query, (username,))``` → the same input is treated as data, not SQL
46. CORS — why it exists and how to configure it properly
47. CSRF attacks — how they work and prevention strategies
48. Rate limiting for auth endpoints — brute force prevention
49. Refresh token rotation — why and how
50. Principle of least privilege — applying it to backend services

## Concurrency & Performance
51. Thread vs process vs coroutine — when to use each
52. Race conditions — how they happen and how to prevent them
53. Deadlocks — detection and prevention strategies
54. Optimistic vs pessimistic locking
55. Event loop — how Node.js/async Python handles concurrency
56. Thread pools — sizing them correctly
57. Backpressure — what it is and why it matters
58. Connection timeouts vs read timeouts — the difference
59. Profiling — how to find bottlenecks in your code
60. Memory leaks — common causes in backend services

## Distributed Systems
61. Eventual consistency — what it means in practice
62. Two-phase commit — how distributed transactions work
63. Saga pattern — managing distributed transactions without 2PC
64. Circuit breaker pattern — preventing cascade failures
65. Service discovery — how microservices find each other
66. Load balancing algorithms — round robin vs least connections vs consistent hashing
67. Distributed tracing — how to trace a request across services
68. Consensus algorithms — what Raft does simply explained
69. Leader election — why distributed systems need it
70. Partition tolerance — what happens when network splits occur

## Message Queues & Events
71. Message queue vs event streaming — Kafka vs RabbitMQ
72. At-least-once vs exactly-once delivery — tradeoffs
73. Dead letter queues — what they are and when to use them
74. Consumer groups — how Kafka partitions work
75. Event sourcing — storing state as a sequence of events
76. CQRS — separating reads and writes
77. Pub/sub pattern — how it differs from point-to-point messaging
78. Message ordering guarantees — when you need them
79. Idempotent consumers — handling duplicate messages
80. Outbox pattern — reliably publishing events from a database

## DevOps & Infrastructure
81. Docker layers — how image caching works
82. Container vs VM — the actual difference
83. Kubernetes pods vs deployments vs services
84. Health checks — liveness vs readiness probes
85. Environment variables vs secrets — how to manage config
86. Blue-green deployment — zero downtime releases
87. Feature flags — how to use them safely
88. Observability — logs vs metrics vs traces
89. SLI, SLO, SLA — what each means and how to set them
90. Graceful shutdown — how to handle SIGTERM properly

## Design Patterns
91. Repository pattern — abstracting data access
92. Dependency injection — what problem it solves
93. Middleware pattern — how Express/FastAPI middleware chains work
94. Retry with exponential backoff — implementing it correctly
95. Bulkhead pattern — isolating failures between services
96. Strangler fig pattern — migrating legacy systems safely
97. Sidecar pattern — extending services without modifying them
98. Factory pattern — when and why to use it in backend code
99. Observer pattern — decoupling event producers and consumers
100. Twelve-Factor App — the methodology behind cloud-native backends