---
tags:
  - FORMAT
  - Database
---
Redis is a high-performance, in-memory data store, often used as a **database, cache, and message broker**. It’s designed for speed and flexibility, supporting multiple data structures and advanced features.

---

# Key Characteristics

* **In-memory**: Stores data primarily in RAM, making read and write operations extremely fast (microsecond range).
* **Persistent**: Can persist data to disk (snapshotting or append-only file) to survive restarts.
* **Single-threaded core**: Handles commands sequentially for simplicity and speed, but supports multiple clients concurrently.
* **Open-source**: Widely used with a large ecosystem.

---

# Data Structures Supported

Redis is not just key-value; it supports rich data types:

1. **Strings** – simple key-value pairs.
2. **Hashes** – maps/dictionaries (field-value pairs).
3. **Lists** – ordered collections (like queues).
4. **Sets** – unordered collections of unique elements.
5. **Sorted Sets** – sets ordered by a score (useful for leaderboards).
6. **Bitmaps** – for compact binary data.
7. **HyperLogLogs** – probabilistic data structures for counting unique elements efficiently.
8. **Streams** – append-only logs for messaging and event streaming.

---

# Common Use Cases

* **Caching**

  * Reduce database load by storing frequently accessed data.
  * Example: session data, user profiles, product catalogs.
* **Pub/Sub messaging**

  * Real-time communication between services.
  * Example: chat apps, notifications.
* **Leaderboards & analytics**

  * Sorted sets make ranking easy.
* **Queues & task management**

  * Lists or streams can implement FIFO queues.
* **Rate limiting & counters**

  * HyperLogLogs, counters, or expirations enforce limits per user/IP.

---

# Features

* **Persistence options**:

  * RDB snapshots (periodic dumps).
  * AOF (Append-Only File) for more durable, incremental persistence.
* **Replication**:

  * Master-slave replication for high availability.
* **Clustering**:

  * Redis Cluster distributes data across multiple nodes for horizontal scalability.
* **Transactions**:

  * Supports simple transactions with `MULTI/EXEC` commands.
* **Lua scripting**:

  * Run atomic scripts on the server for complex operations.
* **Eviction policies**:

  * Can automatically remove old keys when memory is full (LRU, LFU, TTL-based).

---

# Performance

* **Reads**: Extremely fast due to in-memory storage.
* **Writes**: Also very fast, especially for simple key-value operations.
* **Persistence trade-off**: Enabling AOF can slow writes slightly, but increases durability.

---

# Strengths

* Blazing-fast reads and writes.
* Flexible data structures.
* Can be used as a cache, database, or messaging system.
* Simple to deploy and scale.

# Weaknesses / Considerations

* Memory-bound: storing everything in RAM can be expensive for huge datasets.
* Single-threaded: can be CPU-bound under extreme write loads.
* Durability is optional; if not configured carefully, data can be lost in crashes.

---

# Example Usage

**Caching a user profile**:

```java
Jedis jedis = new Jedis("localhost");
String key = "user:1001";
jedis.set(key, "{'name':'Alice','age':30}");  // store data
String profile = jedis.get(key);              // fetch data
```

**Leaderboard using sorted sets**:

```java
jedis.zadd("game:leaderboard", 500, "player1");
jedis.zadd("game:leaderboard", 800, "player2");
Set<String> topPlayers = jedis.zrevrange("game:leaderboard", 0, 2);
```

---

In short: Redis is a **fast, versatile, in-memory data store** used for caching, real-time analytics, messaging, and high-speed data operations.
