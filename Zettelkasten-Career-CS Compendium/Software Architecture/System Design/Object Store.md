---
tags:
  - SystemDesign
---
An **object store** (or object storage) is a type of storage system designed to manage **data as objects**, rather than as files in a hierarchy (file system) or rows/columns in a database.

Each object contains:

1. **Data** – the actual content (file, image, video, log, etc.)
2. **Metadata** – information about the object (creation date, size, type, custom tags)
3. **Unique identifier** – a key or ID used to retrieve the object

---

# How object storage works

* Objects are stored in a **flat address space**, not a folder hierarchy.
* Clients access objects via **APIs** (usually HTTP/REST or S3-compatible APIs).
* Designed for **scalability**: can store petabytes of data across multiple servers or data centers.
* Optimized for **large, unstructured data** rather than transactional data.

---

# Advantages

* **Massive scalability** – can handle huge amounts of data without worrying about file systems.
* **Durability** – data is often replicated across multiple nodes or locations.
* **Metadata-rich** – allows advanced search, classification, or tagging.
* **Cost-effective** – good for archives, backups, media, and big data.

---

# Disadvantages

* **Slower for small, frequent updates** – not ideal for transactional data.
* **No traditional filesystem features** – can’t rely on directories, in-place edits, or complex queries.

---

# Use cases

* Cloud storage (AWS S3, Google Cloud Storage, Azure Blob Storage)
* Backup and archival systems
* Media storage (videos, images, audio)
* Big data and analytics platforms
* Static content hosting for websites

---

In short: **object storage = storing data as objects with metadata and IDs, optimized for large-scale, unstructured data**, unlike block or file storage.
