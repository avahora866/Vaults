---
tags:
  - SystemDesign
---
Change Data Capture (CDC) is a method for **tracking and capturing changes in a database** so they can be propagated to other systems. Unlike batch processes that repeatedly read entire tables, CDC focuses only on **what has changed**: inserts, updates, and deletes.

# How it works:

  * **Log-based CDC** reads the database’s transaction or write-ahead log to detect changes.
  * **Trigger-based CDC** uses database triggers to record changes in a separate table.
  * **Polling-based CDC** periodically queries for rows that have changed using timestamps or version columns.
# Why it’s used:

  * Keeps other systems (data warehouses, search indexes, caches, analytics pipelines) **up to date in near real-time**.
  * Reduces the need to repeatedly scan full tables, improving efficiency.
# Use cases

  * Real-time ETL pipelines.
  * Keeping search-optimized databases like Elasticsearch in sync with the primary DB.
  * Event-driven microservices responding to data changes.

---
**Common implementations**: Debezium (for MySQL, PostgreSQL, MongoDB), AWS DMS, Kafka Connect.

In short, CDC is about **capturing incremental changes in a database efficiently** and delivering them to systems that need up-to-date data.

# Example
Here’s a simple example of implementing CDC using **PostgreSQL and Debezium** to stream changes into **Kafka** and then into another system:

1. **Set up PostgreSQL**
    - Enable logical replication:
        ```sql
        ALTER SYSTEM SET wal_level = logical;
        ALTER SYSTEM SET max_replication_slots = 4;
        ALTER SYSTEM SET max_wal_senders = 4;
        ```
    - Restart PostgreSQL.
    - Create a replication slot for Debezium: Debezium will handle this automatically when configured.
2. **Set up Debezium**
    - Run Debezium as a Kafka Connect connector.
    - Configure a connector for PostgreSQL with:
        - Host, port, database name, user, password
        - Tables to monitor
        - Slot name for logical replication
    - Debezium reads PostgreSQL’s WAL and produces change events into Kafka topics.
3. **Consume changes**
    - Downstream systems (search index, cache, data warehouse) consume the Kafka topic.
    - Each message represents a row change: insert, update, delete.
4. **Example use case**
    - You have a `products` table in PostgreSQL.
    - Debezium captures every insert/update/delete in `products`.
    - A consumer service reads these events and updates an Elasticsearch index in real-time.

Result: the search index is always in sync with the database **without scanning the full table repeatedly**.