---
tags:
  - FORMAT
  - SystemDesign
---
# Consistency in Distributed Systems

In distributed databases or systems, **consistency** refers to how and when changes made to one node become visible to others.

---

# Strong Consistency

## Definition  
Strong consistency guarantees that **after a write completes, any subsequent read will return that written value or a newer one**, no matter which node is queried.

## Characteristics
- Always sees the most recent data
- Writes are **atomic and serialized**
- Guarantees **linearizability** (operations appear to happen in a total order)

## Pros
- **Predictable behavior** for reads
- Easier to reason about correctness
- Good for systems where correctness is critical (e.g., banking)

## Cons
- **Higher latency** due to coordination across nodes
- **Reduced availability** under network partitions (CAP trade-off)
- Often slower for globally distributed systems

## Example Systems
- Zookeeper
- HBase
- Traditional SQL databases with synchronous replication

---

# Eventual Consistency

## Definition  
Eventual consistency guarantees that **if no new updates are made**, all reads will eventually return the **same latest value**, **but not immediately**.

## Characteristics
- Allows stale reads temporarily
- Focuses on **availability** and **partition tolerance**
- Replicas sync asynchronously

## Pros
- **Low latency reads/writes**
- **High availability** even under network partitions
- Scales easily across many nodes and regions

## Cons
- Stale or conflicting reads possible
- Requires **conflict resolution logic** (either automatic or app-side)
- Harder to reason about correctness

## Example Systems
- Cassandra
- DynamoDB
- Couchbase
- Amazon S3 (in eventual mode)

---

# Comparison Table

| Feature               | Strong Consistency               | Eventual Consistency             |
|------------------------|----------------------------------|----------------------------------|
| Read-after-write       | Guaranteed                       | Not guaranteed                   |
| Latency                | High                             | Low                              |
| Availability           | Lower under partitions           | High                             |
| Partition Tolerance    | Typically sacrificed             | Supported                        |
| Conflict Resolution    | Not needed                       | Required                         |
| Common Use Cases       | Financial systems, inventory     | Social feeds, IoT data, logs     |

---

# Intermediate Consistency Models

Most real-world systems offer tunable or intermediate levels between strong and eventual:

| Model               | Description |
|---------------------|-------------|
| **Read-your-writes** | Guarantees you always see your own writes |
| **Monotonic reads**  | Reads reflect an increasing set of writes |
| **Causal consistency** | Operations that are causally related appear in order |
| **Quorum consistency** | Requires a read and write quorum to overlap (e.g., Cassandra with `R + W > N`) |

---

# Choosing Between Them

| Choose Strong Consistency When…             |
|---------------------------------------------|
| Data correctness is non-negotiable          |
| Writes must be immediately visible          |
| You can't tolerate stale reads              |

| Choose Eventual Consistency When…           |
|---------------------------------------------|
| You prioritize uptime, performance, and scale|
| Slightly stale data is acceptable            |
| The system can handle temporary conflicts   |
