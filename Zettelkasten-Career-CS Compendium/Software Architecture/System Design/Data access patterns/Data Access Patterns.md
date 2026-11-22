---
tags:
  - SystemDesign
---
Data access patterns describe the **common ways applications read and write data**. They’re important in system design because different patterns affect **performance, scalability, and cost** depending on how data is stored and retrieved.

---

# Common data access patterns

- **Read-heavy vs write-heavy**
	- Some applications mostly read data (e.g., news sites).
	- Others mostly write/update data (e.g., IoT sensors streaming logs).
- **Read-through cache**
	- Data is first requested from a cache.
	- If it’s not there, the system fetches it from the database, stores it in cache, and returns it.
- **Write-through cache**
	- Writes go to both the cache and the database at the same time.
	- Ensures cache consistency but adds write latency.
- **Write-behind (write-back)**
	- Writes go to the cache first, and the database is updated later asynchronously.
	- Improves write speed but risks data loss if cache fails.
- **Event sourcing**
	- Instead of storing the latest state, you store all events that led to the state.
	- The current state is reconstructed by replaying events.
- [[CQRS (Command Query Responsibility Segregation)]]
	- Separate models for reads and writes.
	- Reads can be optimized for queries (denormalized, cached), while writes focus on consistency.
- **Append-only / time-series**
	- Data is never updated, only appended (logs, metrics, financial transactions).
	- Optimized for sequential writes and time-based queries.
- **Key-value access**
	- Direct lookups by key (e.g., Redis, DynamoDB).
	- Fast and simple, but no complex queries.
- **Document access**
	- Retrieval of semi-structured objects (e.g., JSON in MongoDB).
	- Flexible schema, good for hierarchical data.
- **Relational access**
	- Structured queries across normalized tables (SQL).
	- Good for complex joins, constraints, and strong consistency.

---

# Why access patterns matter

* They guide **database choice** (SQL, NoSQL, cache, time-series DB, etc.).
* They affect **schema design** (normalized vs denormalized).
* They influence **scaling strategy** (replication, sharding, caching).

---
