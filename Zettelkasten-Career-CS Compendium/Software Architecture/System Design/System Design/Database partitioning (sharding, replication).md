---
tags:
  - FORMAT
  - SystemDesign
---
Database partitioning is the process of **splitting data** across multiple storage units (partitions) to improve:
- **Performance**
- **Scalability**
- **Availability**

There are two main forms:
- **Sharding**: Horizontal partitioning
- **Replication**: Copying data across nodes

---

# Sharding (Horizontal Partitioning)

## What It Is  
Sharding distributes different **rows** across multiple databases or nodes. Each shard holds a **subset of the total data**.

For example, users with `user_id % 4 == 0` go to `shard0`, `% 4 == 1` to `shard1`, and so on.

## Why Use It
- Handle **large datasets** beyond a single machine’s capacity
- Improve **query throughput** by parallelizing traffic
- Localize reads/writes per shard

## Sharding Strategies

| Strategy           | How It Works                                    | Pros                                      | Cons                                           |
|--------------------|--------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Hash-based         | Hash a key (e.g., `user_id`) to select a shard   | Even distribution                         | Hard to re-shard; no locality                 |
| Range-based        | Split data by value range (`id < 1M`, etc.)      | Range queries are fast                    | Uneven distribution (hot shards)             |
| Directory-based    | Use a lookup table to map keys to shards         | Flexible                                  | Lookup table is a SPOF unless replicated     |
| Geo-based          | Split by region (EU, US, APAC)                   | Local latency improvements                | Cross-region joins are complex               |

## Challenges of Sharding
- Cross-shard joins and transactions are hard
- Rebalancing shards requires data migration
- Increased operational complexity
- Requires application logic to route to the correct shard

---

# Replication

## What It Is  
Replication involves **copying the same data** across multiple machines for:
- **Redundancy**
- **Read scalability**
- **High availability**

## Replication Types

| Type             | Description                                                 | Use Case                         |
|------------------|-------------------------------------------------------------|----------------------------------|
| Master-Slave     | One primary node handles writes; replicas handle reads      | Read-heavy applications          |
| Master-Master    | Multiple nodes accept writes; replicate to each other       | Multi-region or high-write apps  |
| Synchronous      | Write is acknowledged only after all replicas confirm       | Strong consistency               |
| Asynchronous     | Write is acknowledged after the master writes                | Better latency, risk of stale reads |

## Benefits of Replication
- **High availability**: One node down doesn’t block access
- **Horizontal scaling for reads**
- **Disaster recovery** (backup replication across regions)

## Challenges of Replication
- **Eventual consistency** with async replication
- **Write conflicts** in master-master setups
- **Lag** between master and replicas
- **Split-brain** risk if leader election fails

---

# Combining Sharding + Replication

A robust production-grade system often uses **both**:
- **Shards** to scale horizontally by splitting data
- **Replicas** to scale reads and improve fault tolerance

Each shard typically has a **replication group** (1 master + N replicas).

## Example:
- `Shard 1` → Master A, Replica A1, A2
- `Shard 2` → Master B, Replica B1, B2

Total nodes: 6

---

# Trade-Off Matrix

| Goal             | Sharding             | Replication           |
|------------------|----------------------|------------------------|
| Write scaling     | ✅                    | ❌ (writes go to master)|
| Read scaling      | ✅ (per shard)        | ✅ (via replicas)       |
| Fault tolerance   | ❌ (unless replicated)| ✅                     |
| Data locality     | ✅                    | ❌                     |
| Query complexity  | ❌ (joins are hard)   | ✅ (single dataset)    |
| Operational cost  | High (complex logic) | Medium (coordination) |

---

# Real-World Examples

| System       | Sharding             | Replication         |
|--------------|----------------------|----------------------|
| MongoDB      | Automatic with config | Replica sets         |
| Cassandra    | Hash-based sharding   | Quorum-based replication |
| PostgreSQL   | Manual sharding       | Streaming replication |
| MySQL        | Application-driven    | Master-slave         |
| Redis        | Cluster sharding      | Replication groups   |

---

# Best Practices

- **Pick sharding key carefully**: Aim for even distribution and minimize cross-shard operations
- **Use health checks and leader election** for replication
- **Monitor replication lag**
- **Automate failover and backups**
- **Avoid over-sharding too early**—adds unnecessary complexity