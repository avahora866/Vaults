---
tags:
  - SystemDesign
---
# Scaling

**SQL (Relational Databases)**

* **Vertical scaling (scale-up)**

  * Increase server resources: CPU, RAM, SSDs.
  * Simple but expensive and limited by hardware limits.
* **Horizontal scaling (scale-out)**

  * Harder because relational databases rely on ACID transactions and joins.
  * Usually done via:

    * **Read replicas**: replicate the database for read-heavy workloads.
    * **Sharding**: partition tables across multiple servers by key.
  * More complex; application may need to handle routing queries to the correct shard.

**NoSQL (Non-relational Databases)**

* **Horizontal scaling is natural**

  * Designed for distributed systems; can easily add nodes to handle more data or traffic.
  * Data is often partitioned automatically using key-based hashing or ranges.
* **Replication**

  * Typically supports multiple replicas for high availability and fault tolerance.
* **Consistency trade-offs**

  * Many NoSQL systems use eventual consistency to enable horizontal scaling without slowing writes.

---

# Read/Write Performance

**SQL**

* **Reads**:

  * Fast for structured queries with proper indexing.
  * Can be slower for complex joins or analytics on very large datasets.
  * Can scale reads with replicas.
* **Writes**:

  * Slower compared to NoSQL in high-concurrency environments due to ACID transactions and integrity checks.
  * Scaling writes horizontally is challenging.

**NoSQL**

* **Reads**:

  * Can be extremely fast for key-value or document lookups.
  * Performance may vary if queries require complex aggregations or secondary indexes.
* **Writes**:

  * Often very fast because schema enforcement is minimal and many systems allow eventual consistency.
  * Can handle very high write throughput (logs, sensor data, user events).

---

# Summary Table

| Aspect            | SQL                                               | NoSQL                                                                |
| ----------------- | ------------------------------------------------- | -------------------------------------------------------------------- |
| Scaling           | Vertical (easy), Horizontal (hard)                | Horizontal (easy), vertical optional                                 |
| Read performance  | Good with indexing, moderate with complex joins   | Excellent for simple lookups, variable for complex queries           |
| Write performance | Slower under high concurrency due to ACID         | Fast, designed for high-volume writes                                |
| Consistency       | Strong (ACID)                                     | Often eventual (some support ACID per document)                      |
| Best fit          | Structured data, relational, complex transactions | Large-scale, unstructured/semi-structured, high-throughput workloads |

---

In short:

* **SQL** is better when you need strong consistency and complex queries but is harder to scale horizontally.
* **NoSQL** is better when you need high scalability and throughput, especially for unstructured or semi-structured data, but may sacrifice immediate consistency.
