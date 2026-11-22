---
tags:
  - FORMAT
  - SystemDesign
---
A load balancer is a system (hardware or software) that **distributes incoming network traffic across multiple servers** to improve performance, reliability, and scalability.

Instead of one server handling all requests, a load balancer spreads the load so no single server is overwhelmed.

---

# What a load balancer does

* **Distributes traffic**: Sends requests to multiple servers (backend instances or nodes).
* **Monitors health**: Detects when a server is down and reroutes traffic to healthy servers.
* **Improves availability**: If one server fails, others keep serving requests.
* **Optimizes performance**: Balances requests to avoid bottlenecks.

---

# Load balancing algorithms

* **Round Robin**: Sends requests to servers in order.
* **Least Connections**: Sends requests to the server with the fewest active connections.
* **IP Hash**: Routes a client’s requests to the same server based on IP.
* **Weighted**: Gives more traffic to higher-capacity servers.

---

# Types of load balancers

* **Layer 4 (Transport level)**
	- Works with IP and TCP/UDP.
	- Distributes based on IP address and port, without looking into the content.

* **Layer 7 (Application level)**
	- Works with HTTP/HTTPS.
	- Can make routing decisions based on content (URL path, headers, cookies, etc.).

---

# Benefits

* **Scalability**: Add more servers as traffic grows.
* **High availability**: Handles server failures automatically.
* **Efficiency**: Prevents overloading a single server.
* **Flexibility**: Supports zero-downtime deployments (rolling updates).

---

# Examples

* AWS Elastic Load Balancer (ELB)
* NGINX, HAProxy
* F5 Big-IP
* Google Cloud Load Balancing

---

So:

* A **load balancer** = spreads traffic across servers.
* An **API gateway** = can include load balancing, but adds features like authentication, caching, and request transformation.
