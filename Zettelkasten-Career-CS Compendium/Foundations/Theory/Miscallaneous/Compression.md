---
tags:
  - Theory
---
Compression is the process of **reducing the size of data** so it takes up less storage space or bandwidth when transmitted. It works by **removing redundancy or representing data more efficiently**.

---

# Types of Compression

1. **Lossless Compression**

   * No data is lost; original data can be perfectly reconstructed.
   * Used for text, code, JSON, CSV, databases, or any critical data.
   * Examples:

     * **ZIP**
     * **Gzip**
     * **PNG (images)**
     * **LZ77, LZW algorithms**

2. **Lossy Compression**

   * Some data is discarded to achieve higher compression.
   * Used for images, audio, and video where exact fidelity is not required.
   * Examples:

     * **JPEG** (images)
     * **MP3** (audio)
     * **H.264/H.265** (video)

---

# How Compression works (conceptually)

* **Find patterns**: Repeated sequences, redundant data, predictable patterns.
* **Replace with shorter representations**: e.g., `"aaaaa"` → `"5a"`.
* **Encode efficiently**: Use fewer bits for common patterns and more bits for rare patterns.

---

# Benefits

* **Reduced storage**: Store more data in the same space.
* **Reduced network bandwidth**: Faster file transfers or API responses.
* **Improved performance**: Less data to read/write or transmit (though decompression adds CPU cost).

---

# Trade-offs

* **CPU overhead**: Compressing and decompressing data requires processing.
* **Latency**: Real-time systems may need faster (less compressed) options.
* **Lossy risk**: In lossy compression, some data is permanently lost.

---

# Common use cases

* File storage and archiving (ZIP, Gzip)
* Database storage (compressed columns in PostgreSQL, MySQL)
* Web traffic (HTTP gzip/deflate for JSON, HTML, CSS)
* Media streaming (MP3, H.264 video)
