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
7. Synchronous vs asynchronous communication
8. What happens when you type a URL and hit Enter
9. TCP vs UDP — when each is used in backend systems
10. DNS resolution — how hostnames become IP addresses

## APIs
11. REST vs GraphQL vs gRPC — when to use each
12. API versioning strategies — URI vs header vs query param
13. Pagination — offset vs cursor-based
14. Rate limiting — token bucket vs leaky bucket algorithms
15. API Gateway — what it does and when you need one
16. Webhooks vs polling — tradeoffs
17. HATEOAS — what it is and why most people skip it
18. OpenAPI/Swagger — writing good API specs
19. Request validation — where to do it and why
20. API authentication — API keys vs OAuth vs JWT

## Databases
21. ACID properties — what each one means with examples
22. CAP theorem — why you can only pick two
23. Indexing — how B-tree indexes work
24. Query optimization — reading EXPLAIN output
25. N+1 query problem — how to detect and fix it
26. Database normalization — 1NF, 2NF, 3NF explained simply
27. Transactions — isolation levels and what dirty reads are
28. Connection pooling — why you need it and how it works
29. SQL vs NoSQL — choosing the right tool
30. Database migrations — best practices and rollback strategies

## Caching
31. Cache-aside vs write-through vs write-behind patterns
32. Cache invalidation — why it's hard and common strategies
33. Redis data structures — strings, hashes, sets, sorted sets
34. TTL strategies — how to set expiry times
35. Cache stampede — what it is and how to prevent it
36. CDN caching — how edge caching works
37. HTTP caching headers — Cache-Control, ETag, Last-Modified
38. Memoization vs caching — the difference
39. Distributed cache vs local cache tradeoffs
40. When NOT to cache — cases where caching hurts

## Authentication & Security
41. JWT structure — header, payload, signature decoded
42. OAuth 2.0 flows — authorization code vs client credentials
43. Session vs token authentication — tradeoffs
44. Password hashing — bcrypt vs Argon2 vs PBKDF2
45. SQL injection — how it works and how parameterized queries prevent it
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