---
tags:
  - SystemDesign
---
In many projects, we need to move or propagate data to different systems. There are several techniques for doing this, each with its own pros and cons. Some of the common methods are listed below.

# Data replication (keep a copy of the same DB)
Data replication ensures that a copy of a database exists in another location or system. This is primarily done to increase availability, support disaster recovery, and balance read-heavy workloads. By replicating data, organizations can reduce latency for geographically distributed users and maintain a standby database that can take over in case the primary system fails, ensuring business continuity without affecting performance.

Some of the techniques of data replication are below
- **Physical replication** (block-level, storage engine, e.g., PostgreSQL streaming replication)
- **Logical replication** (row-level, statement-level, e.g., MySQL binlog, Postgres logical replication)
- **Synchronous vs. asynchronous replication** (trade-off between latency and safety)
# Data synchronization / change propagation
Data synchronization focuses on keeping multiple systems consistent with each other. This is important when changes in one system need to be reflected in another in near real-time, such as updating search indexes, caches, or microservices. Synchronization ensures downstream systems always have the latest data, supports event-driven architectures, and helps maintain operational consistency across different components of an application.

Some of the techniques of data synchronization are below
- **Change Data Capture (CDC)**
    - Log-based (reads DB transaction logs)
    - Trigger-based (DB triggers writing to side tables)
    - Query-based / polling (checking timestamps/version columns)
- **Dual writes** (application writes to multiple systems at once)
- **Event sourcing** (application records all events, which are replayed into other systems)
# Data integration / ingestion
Data integration involves moving data from one or more sources into a centralized system like a data warehouse or data lake. The main goal is to enable analytics, reporting, and business intelligence by combining data from multiple systems. Integration allows organizations to perform historical analysis, generate insights from aggregated data, and create a unified view of business operations for decision-making.

Some of the techniques of data integration are below
- **Batch ETL/ELT** (nightly/hourly extracts, transform, and load into a warehouse)
- **Streaming ingestion** (Kafka, Kinesis, Pulsar; continuous feed of events into target systems)
- **API-based ingestion** (pulling from external services via APIs)
# Hybrid / derived techniques
Hybrid and derived techniques create precomputed or transformed datasets to improve performance or support specialized workloads. Examples include materialized views, cached results, or search-optimized indexes. These techniques reduce query latency, support complex queries that would otherwise be slow on raw data, and automatically keep derived data in sync with the source, combining the benefits of replication, synchronization, and integration for targeted performance improvements.

Some of the hybrid techniques are below
- **Materialized views** (database-native precomputed copies of queries, auto-refreshed or manual refresh)
- **Search sync** (pushing DB changes into search-optimized systems like Elasticsearch via CDC or dual writes)
- **Cache warming** (pushing data into Redis/Memcached to keep reads fast)

---

Key idea: replication and synchronization focus on **consistency and availability**, integration focuses on **analytics and reporting**, and hybrid/derived focuses on **performance or specialised use cases**.

If you think about it in terms of _purpose_:

- **Replication** = high availability, load balancing, disaster recovery.
- **Synchronization** = keep external systems up to date (search, cache, microservices).
- **Integration** = move data into analytics, BI, or external platforms.
- **Hybrid** = optimise query performance or support special workloads.
    