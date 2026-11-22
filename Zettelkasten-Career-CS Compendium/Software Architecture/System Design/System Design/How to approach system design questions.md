---
tags:
  - FORMAT
  - SystemDesign
---
# Understand the Goal

Before designing anything, clarify:
- What the system needs to do (functionality)
- Who the users are
- Key constraints (latency, scale, consistency, deadlines)

Start with a clear **problem statement**:
- “Design a URL shortening service”
- “Design a scalable chat app”

---

# Clarify Requirements

## a. Functional Requirements
Ask:
- What are the core features?
- What should the system *definitely* do?

Example (URL Shortener):
- Shorten URLs
- Redirect to original URL
- Track click stats (optional)

## b. Non-Functional Requirements
Ask about:
- Scalability
- Latency expectations
- Uptime/availability (e.g. 99.9%)
- Throughput (requests per second)
- Consistency vs availability

## c. Constraints and Assumptions
Define:
- Number of users
- Read vs write ratio
- Data size growth
- External integrations (APIs, SDKs)

---

# Define High-Level Architecture

Sketch a basic **component diagram** with:
- Clients
- Load balancers
- Web/application servers
- Databases
- Caches
- External services (queues, CDN, object stores)

This shows how components interact at a system level.

---

# Break Into Subsystems

Decompose the problem:
- Authentication service
- API gateway
- Search indexer
- Messaging queue
- Worker service

Explain how each piece contributes to system goals.

---

# Address Key Design Challenges

## a. Data Modeling
- What data is stored?
- Which DB fits the access pattern? (SQL vs NoSQL vs key-value store)
- Do you need ACID or eventual consistency?

## b. Caching
- What to cache (pages, DB queries, objects)
- Where (CDN, Redis, in-memory)
- Eviction and expiration strategies

## c. Rate Limiting
- To prevent abuse or DoS
- Per-IP or per-user limits
- Use Redis, API gateway, or token buckets

## d. Load Balancing
- Round-robin, consistent hashing, L7 vs L4
- Failover handling
- Sticky sessions?

## e. Scalability
- Horizontal vs vertical scaling
- Partitioning (sharding, hashing)
- Asynchronous processing with queues

## f. Availability
- Redundancy (multi-region)
- Health checks and failover
- Backups and disaster recovery

## g. Security
- AuthN/AuthZ (OAuth2, JWT)
- Data encryption (in transit, at rest)
- Rate limiting, input sanitization

---

# Tradeoffs

Discuss real architectural tradeoffs:
| Choice                    | Pros                                | Cons                                  |
|---------------------------|--------------------------------------|---------------------------------------|
| SQL vs NoSQL              | Structured queries vs flexibility   | Complexity vs performance             |
| Cache before DB           | Fast reads                          | Stale data risk                       |
| Async queue for writes    | Decoupling, better throughput       | Delayed consistency                   |
| Denormalization           | Faster reads                        | Harder updates                        |

---

# Bottlenecks and Scaling Strategy

- Identify single points of failure
- Project future load
- Use back-of-envelope math:
  - Requests/sec × bytes/request = bandwidth needs
  - Read:write ratio
  - DB IOPS requirements

Plan:
- What happens at 10× scale?
- Which services need to scale first?

---

# Final Touches

## Monitoring
- Metrics (latency, error rate, throughput)
- Logs (structured, searchable)
- Alerts (SLO/SLI/SLA violations)

## Deployment
- CI/CD pipelines
- Blue-green or canary deployments
- Rollback strategy

## Testing
- Load tests
- Fault injection
- Chaos engineering (advanced)

---

# Communication Style in Interviews

- **Think aloud**—show your reasoning
- **Ask questions** to clarify and validate assumptions
- **Make tradeoffs explicit**
- **Draw diagrams** (even rough ones) to illustrate architecture
- **Adapt** if the interviewer adds complexity (e.g. “now support 10× traffic”)

---

# Practice Scenarios

| System                          | Concepts to Focus On                                      |
|---------------------------------|------------------------------------------------------------|
| URL shortener                   | Hashing, DB design, cache, redirect latency                |
| Social media feed               | Feed generation, fan-out, ranking, caching, pagination     |
| Chat system                     | Real-time delivery, message queues, storage, presence      |
| File storage (e.g. Dropbox)     | Chunking, deduplication, object storage, sync conflicts    |
| Ride-sharing app backend        | Geo indexing, matching, updates, load balancing            |

---