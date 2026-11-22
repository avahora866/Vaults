---
tags:
  - FORMAT
  - SystemDesign
---
**Rate limiting** restricts how many requests a client can make to a server in a given time window.

- Protects APIs from abuse or accidental overload
- Ensures fair use among clients
- Controls resource usage

---

# Common Strategies:

1. **Fixed Window**
    
    - Example: Max 100 requests per minute
    - Resets every fixed period
    - Simple but can allow burst usage at window edges
2. **Sliding Window**
    
    - Uses rolling time windows (e.g., 60 seconds from the current request)
    - Smoother and fairer than fixed window
    - Requires tracking timestamps
3. **Token Bucket**
    
    - Bucket holds tokens (e.g., 100)
    - Each request uses a token
    - Tokens refill at a set rate
    - Allows short bursts while controlling sustained rates
4. **Leaky Bucket**
    
    - Like token bucket, but rate of processing is fixed
    - Helps smooth spikes; excess requests are delayed or dropped

---

# Response Headers (standard practice):

APIs often return headers like:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1682783200
```

This tells the client how many requests remain and when limits reset.

---

# Handling Rate Limits:

Clients should:

- Check headers
- Back off or retry after `Retry-After` header (if provided)
- Use exponential backoff for retries

---

# Example:

```
GET /api/data
HTTP/1.1 429 Too Many Requests
Retry-After: 60
```