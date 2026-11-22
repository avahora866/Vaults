---
tags:
  - SystemDesign
---
The **read/write ratio** is a metric that compares the number of read operations to the number of write operations in a system over a period of time.

It helps determine whether a system is **read-heavy**, **write-heavy**, or balanced, which in turn guides **database design, caching, replication, and scaling strategies**.

---

# How it’s expressed

* Usually as **R\:W** (reads : writes).
* Examples:

  * 10:1 → 10 reads for every write (read-heavy)
  * 1:1 → balanced reads and writes
  * 1:5 → 1 read for every 5 writes (write-heavy)

---

# Why it matters

* **Read-heavy systems** (high read/write ratio) benefit from:
	- Caching
	- Read replicas
	- Denormalization for faster reads

* **Write-heavy systems** (low read/write ratio) benefit from:
	- Optimized write paths
	- Batch processing
	- Append-only or log-based storage

* **Balanced systems** may require a mix of read and write optimizations.

---

# Example

A social media feed system:
* Users mostly view posts, occasionally create new ones.
* Read/write ratio might be 20:1.
* Optimizations: caching feeds, read replicas, CDN for static content.

A logging system:
* Every action generates a new log entry; reads happen rarely.
* Read/write ratio might be 1:50.
* Optimizations: write-optimized storage, batch indexing, sequential append-only writes.

---
