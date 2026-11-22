---
tags:
  - FORMAT
  - SystemDesign
---
Caching stores **frequently accessed data** closer to where it’s used to reduce:
- Latency
- Backend load
- Cost (e.g. database or API usage)

It trades **stale data risk** for **speed and scale**.

---

# Key Cache Types

| Type          | Location                    | Example Use                                 |
|---------------|-----------------------------|---------------------------------------------|
| Client-side   | In browser or app           | Images, static HTML                         |
| Edge/Network  | CDN nodes near users        | Images, videos, JS, CSS                     |
| Server-side   | Between app and DB/API      | Query results, API responses, sessions      |
| In-process    | Inside the app runtime      | Local object/memory caching (e.g. LRU cache)|

---

# CDN (Content Delivery Network)

## What It Is
A [[Content Delivery Networks (CDNs)|CDN]] is a geographically distributed network of edge servers that cache static or semi-static content like:
- Images
- CSS/JS files
- Videos
- HTML pages
- API responses (with proper headers)

## Key Features
- Caches are stored **close to the user** to minimize latency
- Works mostly with **HTTP caching** headers
- Offloads origin server and reduces bandwidth

## Popular CDNs
Cloudflare, Akamai, Fastly, AWS CloudFront, Google Cloud CDN

## How It Works
User requests a file (e.g., image.jpg)
1. If the CDN edge node has it (cache hit), it returns the file
If not (cache miss), it fetches from origin, stores it, then serves

## Cache Control Headers
- `Cache-Control`: `public, max-age=3600`
- `ETag`: Entity tag for validation
- `Expires`: HTTP/1.0 compatibility
- `Vary`: Allows for content negotiation (e.g., language)

## Pros
- Reduces latency globally
- Offloads traffic from your servers
- Built-in DDoS protection (in many CDNs)

## Cons
- Not suitable for rapidly changing data
- Cache invalidation can be hard (but CDNs offer purge APIs)

---

# Redis (In-Memory Cache)

## What It Is
[[Redis]] is an **in-memory key-value store** often used for fast server-side caching.

## Key Use Cases
- Database query results
- Computation results
- API rate limiting
- Sessions and auth tokens
- Pub/sub and queues

## Caching Patterns Using Redis

## Cache-Aside (Lazy Loading)
- App checks Redis first.
- If not found (cache miss), it queries the DB and stores the result in Redis.

```java
String user = redis.get("user:123");
if (user == null) {
    user = db.query("SELECT * FROM users WHERE id = 123");
    redis.set("user:123", user, 3600); // 1-hour TTL
}
```

**Pros**: Simple, avoids stale data  
**Cons**: Cold-start latency

---

## Write-Through
- Write goes to both DB and cache simultaneously.

```java
db.insert(user);
redis.set("user:123", user);
```

**Pros**: Cache always fresh  
**Cons**: Slower writes, more complex error handling

---

## Write-Behind (Write-Back)
- Write goes to cache first; DB write is deferred/asynchronous.

**Pros**: Fast writes  
**Cons**: Risk of data loss on crash, needs queue or log

---

## Read-Through
- App only talks to Redis. Redis loads from DB automatically on miss (needs custom code or Redis modules).

**Pros**: Centralized caching logic  
**Cons**: Tight Redis coupling

---

# Redis Expiration and Eviction

- Redis supports **TTL (Time To Live)** for each key
- Eviction policies for memory pressure:
  - `noeviction` (default)
  - `allkeys-lru`
  - `volatile-lru`
  - `allkeys-random`

---

# General Cache Design Considerations

| Factor              | Questions to Ask                           |
|---------------------|--------------------------------------------|
| Freshness           | How long can data be stale?                |
| Eviction policy     | What happens when cache fills up?          |
| Invalidation        | How do we remove/refresh stale data?       |
| Write consistency   | Does cache update on writes?               |
| Cold start          | How do we handle cache misses?             |

---

# Cache Invalidation Techniques

- TTL-based expiration
- Manual invalidation (purge by key or pattern)
- Versioned keys (`user:123:v2`)
- Event-driven (invalidate on DB update)

---

# Summary Table

| Strategy        | Tool      | Good For                                | Notes                                    |
|------------------|-----------|------------------------------------------|-------------------------------------------|
| HTTP CDN Cache   | CDN       | Static assets, large public files        | Best with proper headers and TTL          |
| Cache-Aside      | Redis     | DB query results                         | Simple and commonly used                  |
| Write-Through    | Redis     | Read-heavy data with strict consistency  | Adds write latency                        |
| Write-Behind     | Redis     | High write throughput, async persistence | Needs durability safeguards               |
| Read-Through     | Redis     | Centralized access, integrated caching   | Higher complexity                         |
