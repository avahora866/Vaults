---
tags:
  - SystemDesign
---
A CDN (Content Delivery Network) is a distributed system of servers designed to deliver content (web pages, images, videos, files, APIs, etc.) to users more quickly and reliably.

Instead of every user pulling data from a single origin server, a CDN stores cached copies of content on multiple **edge servers** located around the world. When a user makes a request, they are routed to the nearest or fastest edge server, reducing distance and latency.

# How a CDN works

1. You request a resource (e.g., a website image).
2. DNS or routing directs you to the nearest CDN edge server.
3. If the edge server has the cached resource, it serves it immediately.
4. If not, it fetches it from the origin server, caches it, and serves it to you.

# What CDNs improve

* **Lower latency**: Data comes from a nearby server instead of a distant origin.
* **Higher throughput**: Load is spread across many servers instead of one.
* **Scalability**: Can handle large traffic spikes (e.g., live events, product launches).
* **Reliability**: If one server goes down, others can take over.
* **Security**: CDNs often provide DDoS protection, rate limiting, and TLS termination.

# Examples of CDN providers

* Cloudflare
* Akamai
* Amazon CloudFront
* Google Cloud CDN
* Fastly
