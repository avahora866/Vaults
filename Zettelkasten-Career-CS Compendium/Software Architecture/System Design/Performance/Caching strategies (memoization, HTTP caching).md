---
tags:
  - SystemDesign
---
# What is Caching?

Caching stores the results of expensive operations so future requests for the same data can be served faster. Caching improves performance, reduces load, and can improve user experience.

---

# Memoization

## What It Is
Memoization is a **function-level** caching technique that stores the result of a function call for given inputs, avoiding repeated computations.

## How It Works
- When a function is called:
  - Check if the result is already stored for the given input
  - If yes, return the cached result
  - If no, compute it, store it, and return the result

## Characteristics
| Feature       | Detail                             |
|---------------|-------------------------------------|
| Scope         | In-memory, local to application     |
| Granularity   | Per function call and input         |
| Volatility    | Cleared when program restarts       |
| Eviction      | Usually not implemented by default (can use LRU manually) |

## When to Use
- Pure functions (same input → same output)
- Expensive computations (e.g., recursion, I/O-heavy functions)
- Low input cardinality (few unique inputs)

## Examples
## Java
```java
Map<Integer, Integer> cache = new HashMap<>();

int fib(int n) {
    if (cache.containsKey(n)) return cache.get(n);
    if (n <= 1) return n;
    int result = fib(n - 1) + fib(n - 2);
    cache.put(n, result);
    return result;
}
```

## Python
```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)
```

---

# HTTP Caching

## What It Is
HTTP caching is a **network-level** caching technique that stores responses at various layers (browser, CDN, reverse proxy) to reduce server load and improve response time.

## Types
| Type         | Description                                                       |
|--------------|-------------------------------------------------------------------|
| Browser cache | Caches response locally in the browser                           |
| Proxy cache   | Intermediary server caches (e.g. CDN like Cloudflare)            |
| Server cache  | Application or origin server caches responses                    |

## HTTP Headers for Caching
| Header             | Purpose                                                                 |
|--------------------|-------------------------------------------------------------------------|
| `Cache-Control`    | Defines caching policy (`max-age`, `no-cache`, `public`, `private`)    |
| `ETag`             | Validator for conditional requests using a hash/fingerprint            |
| `Last-Modified`    | Indicates when the resource was last changed                           |
| `Expires`          | Legacy way to define expiry time                                        |

## Cache-Control Examples
- `Cache-Control: max-age=3600, public`: Cache for 1 hour
- `Cache-Control: no-store`: Don’t store at all
- `Cache-Control: must-revalidate`: Revalidate before using cached response

## Validation Strategies
| Strategy           | Mechanism                            | Result                                 |
|--------------------|----------------------------------------|----------------------------------------|
| Fresh              | Use cached content                    | Fast, no request                       |
| Stale with revalidation | Use `ETag` or `Last-Modified`     | Return 304 if unchanged                |
| Expired            | Refetch from origin                   | New content fetched                    |

## When to Use
- Static resources (e.g. JS, CSS, images)
- API responses that don’t change often
- Resources behind CDN

---

# Comparison: Memoization vs HTTP Caching

| Feature            | Memoization                       | HTTP Caching                           |
|--------------------|------------------------------------|----------------------------------------|
| Layer              | Application                        | Network/browser/server                 |
| Scope              | In-memory, per process             | Across users, sessions, locations      |
| Input              | Function arguments                 | URL, headers, query parameters         |
| Volatility         | Lost on restart unless persisted   | Can persist via CDNs, proxies          |
| Use Case           | Function optimization              | Response delivery optimization         |

---

# Other Caching Strategies (for context)

| Strategy          | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| In-memory cache   | Local cache (e.g. `ConcurrentHashMap`, Guava, Caffeine)                     |
| Distributed cache | Shared cache across instances (e.g. Redis, Memcached)                       |
| Write-through     | Cache + database update on write                                            |
| Write-back        | Write to cache first, flush to DB later                                     |
| Read-through      | Cache fetches from DB on miss                                               |
| Time-based expiry | Items expire after a set time (TTL)                                         |
| Eviction policies | LRU, LFU, FIFO, etc., to manage cache size                                  |

---

# Best Practices

- Use memoization only for pure, deterministic functions
- Set appropriate `Cache-Control` headers on HTTP responses
- Use ETag and conditional GET to reduce unnecessary bandwidth
- Monitor cache hit/miss rates to fine-tune strategy
- Don’t cache sensitive or user-specific data without controls