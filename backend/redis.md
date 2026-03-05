# Redis — Interview Ready Notes

## 1. What is Redis
**Redis (Remote Dictionary Server)** is an **in-memory data structure store** used as:
- Cache
- Database
- Message broker
- Streaming engine

It stores data **in RAM**, making it extremely fast (microseconds latency). Redis supports multiple **data structures**, not just key–value.

**Typical performance:**
- **Reads/Writes:** ~100k+ ops/sec
- **Latency:** < 1 ms

---

## 2. Why Redis is Used
Primary reasons:
1. **Caching** (Reduce DB load)
2. **Session storage** (User login states)
3. **Rate limiting** (API protection)
4. **Real-time analytics** (Counting events)
5. **Pub/Sub messaging** (Chat/Notifications)
6. **Distributed locking** (Preventing race conditions)
7. **Queues** (Task processing)

---

## 3. Key Characteristics
- **In-Memory:** Data stored in RAM
- **Persistence:** Optional disk persistence (RDB/AOF)
- **Data Structures:** Strings, Lists, Sets, Sorted Sets, Hashes
- **Atomic Operations:** Commands execute atomically
- **Single Threaded:** Avoids lock overhead (uses I/O multiplexing)
- **Extremely Fast:** Memory + optimized engine

---

## 4. Redis Data Structures

### 1. String
Basic key-value.
`SET user:1 "Sayan"`
`GET user:1`
- **Use cases:** caching, counters, flags

### 2. Hash
Object-like structure.
`HSET user:1 name "Sayan" age 25`
`HGET user:1 name`
- **Use case:** storing objects

### 3. List
Ordered sequence.
`LPUSH queue job1`
`RPUSH queue job2`
- **Use case:** queues, task processing

### 4. Set
Unique unordered elements.
`SADD users "A" "B"`
`SMEMBERS users`
- **Use case:** unique collections, tags

### 5. Sorted Set (ZSET)
Ordered by score.
`ZADD leaderboard 100 user1`
`ZADD leaderboard 200 user2`
- **Use case:** leaderboards, ranking systems

---

## 5. Redis Persistence

### RDB (Snapshot)
Periodically saves full dataset.
- **Pros:** fast, compact
- **Cons:** possible data loss between snapshots

### AOF (Append Only File)
Logs every write operation.
- **Pros:** safer, more durable
- **Cons:** larger files, slower recovery

---

## 6. Redis Expiration (TTL)
Redis supports automatic expiration.
`SET session:123 data EX 60`
- **Meaning:** Key expires after 60 seconds
- **Use Case:** Heavily used in caching and session management.

---

## 7. Redis Caching Pattern (Cache-Aside)

1. **Client** requests data.
2. Check **Redis Cache**.
3. **Cache Hit:** Return data.
4. **Cache Miss:** Fetch from **DB** → Store in **Redis** → Return data.

---

## 8. Implementation Examples

### Java (Jedis)
```xml
<dependency>
 <groupId>redis.clients</groupId>
 <artifactId>jedis</artifactId>
 <version>5.0.0</version>
</dependency>
```


```java
import redis.clients.jedis.Jedis;

public class RedisExample {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost", 6379);
        jedis.set("user:1", "Sayan");
        String value = jedis.get("user:1");
        jedis.setex("session:1", 60, "active");
        jedis.hset("user:2", "name", "Batman");
        jedis.close();
    }
}
```

### Python (redis-py)
```python
import redis

r = redis.Redis(host='localhost', port=6379, db=0)
r.set("user:1", "Sayan")
value = r.get("user:1")
r.setex("session:1", 60, "active")
r.hset("user:2", mapping={"name": "Batman", "age": 25})
```

---

## 9. Redis Pub/Sub
**Publisher:**
```
PUBLISH channel1 "hello"
```

**Subscriber:**
```
SUBSCRIBE channel1
```

**Use in:** real-time notifications, chat systems.

---

## 10. Redis Distributed Lock
```
SET lock_key value NX EX 10
```
- **NX:** Only set if not exists
- **EX 10:** Expire in 10s (prevents deadlocks)
- **Use in:** cron jobs, distributed workers.

---

## 11. Redis vs Memcached
- **Data structures:** Redis ✓ | Memcached ✗
- **Persistence:** Redis ✓ | Memcached ✗
- **Pub/Sub:** Redis ✓ | Memcached ✗
- **Performance:** Redis (Fast) | Memcached (Slightly faster for small strings)

---

## 12. When NOT to Use Redis
- Dataset significantly larger than memory.
- Strict relational queries or complex joins required.
- Critical data that cannot risk even 1 second of loss (unless heavily tuned).

---

## 13. Common Interview Questions

**Q1: Why is Redis fast?**
In-memory, single-threaded (no context switching), and optimized data structures.

**Q2: What is Redis eviction policy?**
When memory is full, Redis removes keys based on policy: `allkeys-lru` (Least Recently Used) is most common.

**Q3: What is Redis Cluster?**
A way to run Redis where data is automatically sharded across multiple nodes for horizontal scaling and high availability.

---

## 14. One Sentence Summary
Redis is a high-performance in-memory data structure store used primarily as a cache to dramatically reduce database load and improve application latency.
