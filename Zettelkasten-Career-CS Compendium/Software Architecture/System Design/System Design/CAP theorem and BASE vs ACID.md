---
tags:
  - FORMAT
  - SystemDesign
---
# CAP Theorem

## Definition
CAP theorem states that **in any distributed data system**, you can only **guarantee two of the following three** at the same time:

| Property                    | Description                                                                                       |
| --------------------------- | ------------------------------------------------------------------------------------------------- |
| **Consistency (C)**         | Every read gets the most recent write or an error                                                 |
| **Availability (A)**        | Every request gets a non-error response (but not guaranteed to be up-to-date)                     |
| **Partition Tolerance (P)** | The system continues to operate despite network partitions (message loss or delays between nodes) |

---

## Implication
In the presence of a **network partition (P)**, you must choose between:
- **Consistency** (but risk rejecting reads/writes)
- **Availability** (but risk serving stale data)

You **cannot guarantee all three**.

---

## Examples of CAP Tradeoffs

| System Type      | Prioritized Properties | Examples                    |
|------------------|------------------------|-----------------------------|
| CP               | Consistency + Partition Tolerance | HBase, MongoDB (with majority writes), Zookeeper |
| AP               | Availability + Partition Tolerance | Cassandra, Couchbase, DynamoDB |
| CA *(theoretical only)* | Consistency + Availability (no partition tolerance) | Single-node relational DBs (no network split) |

---

# BASE vs ACID

These are two **data consistency models** often tied to the CAP choices:

---

## ACID (Traditional Databases)

| Property         | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **Atomicity**     | Transactions are all-or-nothing                                              |
| **Consistency**   | Transactions take the DB from one valid state to another                    |
| **Isolation**     | Transactions appear to run sequentially (no interference)                   |
| **Durability**    | Once a transaction commits, it persists even after crashes                  |

**Used by**: SQL databases like PostgreSQL, MySQL, Oracle

**Guarantees**:
- Strong consistency
- Safe, predictable behavior
- Slower under distributed load

---

## BASE (Distributed Systems)

| Property         | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **Basically Available** | System is always available (non-error responses)                    |
| **Soft state**   | State may change over time even without new input (due to eventual syncs)  |
| **Eventual consistency** | Data will become consistent over time if no new updates occur      |

**Used by**: NoSQL systems like Cassandra, DynamoDB, Couchbase

**Trade-offs**:
- Prioritizes **availability and partition tolerance**
- Accepts temporary inconsistency
- Faster response times, high fault tolerance

---

## Comparison Table

| Feature           | ACID (SQL)               | BASE (NoSQL)                  |
|------------------|--------------------------|-------------------------------|
| Consistency       | Strong                   | Eventual                      |
| Availability      | Lower under partitions   | High                          |
| Tolerance to Partition | Low                 | High                          |
| Use Case          | Banking, accounting      | Social media, analytics, IoT  |
| Latency           | Higher                   | Lower                         |
| Transactions      | Yes                      | Rare or eventually consistent |

---

# When to Use What

## Use ACID:
- When **data integrity is critical**
- Banking, payments, inventory systems
- Schema is well-defined and relatively static

## Use BASE:
- When **scale and availability** are more important
- Real-time feeds, analytics, user activity logs
- Data is large and partitionable

---

# Practical Reality

In practice:
- Systems **mix models** using hybrid approaches
- E.g. MongoDB offers tunable consistency
- SQL DBs now support **eventual consistency** modes (e.g., PostgreSQL async replication)