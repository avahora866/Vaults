---
tags:
  - SystemDesign
---
# Rate Limiting
Restricts the **number of operations per user/client/system in a fixed time window**. It's used to:
- Prevent abuse or DoS attacks
- Enforce fair usage
- Control resource load

# Throttling
Slows down or **delays processing** rather than outright rejecting requests. It’s used to:
- Smooth traffic spikes
- Defer or pace expensive operations
- Avoid triggering limits

---

# Differences

| Aspect              | Rate Limiting                             | Throttling                              |
|---------------------|--------------------------------------------|------------------------------------------|
| Purpose             | Enforce maximum request rate               | Manage resource usage over time          |
| Behavior            | Reject excess requests                     | Delay or queue excess requests           |
| User experience     | Can result in HTTP 429 (Too Many Requests) | Slower responses, not immediate failure  |
| Typical Usage       | APIs, login attempts, messaging systems    | Background jobs, email sending, bulk ops |

---

# Common Use Cases

- **APIs**: Limit requests per IP/token to avoid abuse
- **Authentication**: Limit login attempts to prevent brute-force attacks
- **Messaging systems**: Throttle message publishing to protect consumers
- **Resource management**: Cap bandwidth or CPU usage per process
- **Web scraping**: Respect rate limits to avoid IP bans

---

# Rate Limiting Strategies

## Fixed Window
- Count requests in a fixed interval (e.g. 100 requests per minute)
- Simple but can allow burst at window edges

## Sliding Window
- Breaks time into smaller chunks (e.g. per second within a minute)
- Smoother and more accurate than fixed window

## Token Bucket
- Tokens are added at a fixed rate
- Each request consumes a token
- Allows bursts up to a maximum

## Leaky Bucket
- Queue incoming requests
- Processes at a steady rate
- Smooths out bursty traffic

## Examples
| Method         | Allows Bursts? | Complexity | Use Case                          |
|----------------|----------------|------------|-----------------------------------|
| Fixed Window   | Yes (edge burst)| Low        | Simple API rate limits            |
| Sliding Window | Moderate        | Medium     | More fair API handling            |
| Token Bucket   | Yes             | Medium     | User-facing apps (bursts OK)      |
| Leaky Bucket   | No              | Medium     | Bandwidth shaping, smooth pacing  |

---

# Throttling Techniques

## Client-side Throttling
- Backoff strategy (e.g. exponential backoff)
- Sleep or delay requests before retrying

## Server-side Throttling
- Queue requests (with caps)
- Slow down expensive operations (e.g. limit parallel DB queries)

## Examples
- Limit to 10 emails per second per user
- Delay message publishing if rate exceeds safe threshold

---

# Implementation Techniques

## In-Memory (Single Node)
- Use hashmaps or counters (e.g. `ConcurrentHashMap` in Java)
- Fast, but not scalable

## Distributed (Multi-Node)
- Use shared stores: Redis, Memcached
- Atomic counters with expiry (e.g. `INCR` + `EXPIRE` in Redis)
- Use `SETNX` for locks or tokens

## Middleware / API Gateways
- Envoy, NGINX, Kong, AWS API Gateway offer built-in rate limiting
- Can configure per IP, token, or route

## Libraries and Tools
| Language | Libraries/Tools                                |
|----------|------------------------------------------------|
| Java     | Bucket4j, Resilience4j, Guava RateLimiter      |
| Python   | `ratelimit`, Django Ratelimit, Flask-Limiter   |
| Node.js  | `express-rate-limit`, `rate-limiter-flexible`  |
| Redis    | Lua scripts, RedisRateLimiter pattern          |

---

# HTTP Response Standards

When rate limiting is enforced:
- Use **HTTP 429 Too Many Requests**
- Include headers:
  - `Retry-After`: seconds or date to retry
  - `X-RateLimit-Limit`: max requests allowed
  - `X-RateLimit-Remaining`: how many left
  - `X-RateLimit-Reset`: time when rate limit resets

---

# Best Practices

- **Granularity**: Use per-user, per-IP, per-token limits based on context
- **Differentiation**: Apply stricter limits for unauthenticated users
- **Graceful degradation**: Combine throttling with retries or queues
- **Transparency**: Return clear errors with headers so clients can adjust
- **Monitoring**: Track hits, misses, and abuse attempts
- **Override**: Allow bypasses for internal systems or trusted clients