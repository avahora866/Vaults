---
tags:
  - Architecture
  - DevOps
  - FORMAT
  - "#LEAF"
---
**Horizontal vs. Vertical Scaling:** Horizontal scaling (scaling out) involves adding more machines or instances to distribute the load, improving scalability and fault tolerance. Vertical scaling (scaling up) increases the resources (CPU, RAM, storage) of a single machine to handle higher loads, but it has hardware limitations and potential downtime for upgrades.

**Caching Strategies:** Caching improves performance by storing frequently accessed data in a faster storage layer. Common strategies include **write-through** (writes data to cache and database simultaneously), **write-back** (writes to cache first and asynchronously updates the database), **least recently used (LRU)** eviction, and **content delivery networks (CDNs)** for distributing static content.

**Load Balancing:** Load balancers distribute incoming traffic across multiple servers to optimize resource use, reduce latency, and ensure high availability. Strategies include **round-robin** (equal distribution), **least connections** (sends requests to the least busy server), and **IP hash** (routes based on client IP). Load balancing can be implemented at **Layer 4 (TCP/UDP)** or **Layer 7 (HTTP/HTTPS)** in the OSI model.

**Database Sharding and Partitioning:** **Sharding** splits a database into smaller, independent shards, each handling a subset of data, improving performance and scalability but increasing complexity. **Partitioning** divides data within a single database instance, typically by range, hash, or list, improving query efficiency and manageability while keeping a unified system.