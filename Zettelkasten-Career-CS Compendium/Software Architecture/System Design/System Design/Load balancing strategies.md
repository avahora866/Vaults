---
tags:
  - FORMAT
  - SystemDesign
---
Load balancing distributes incoming network or application traffic across multiple servers to:
- Prevent any one server from being overloaded
- Improve responsiveness
- Increase availability and fault tolerance

It’s a key component in scalable and highly available systems.

---

# Where Load Balancers Operate

| Layer        | Type               | Example Protocols           |
| --------------------- | --------------------- | --------------------------- |
| Layer 4 (Transport) | TCP/UDP-level routing | TCP, UDP, TLS               |
| Layer 7 (Application) | Content-aware routing | HTTP, HTTPS, [[gRPC\|gRPC]] |

---

# Core Load Balancing Strategies

## Round Robin
- Sends requests to servers in a fixed, circular order
- Works best when all servers have equal capacity and load

**Pros**: Simple, no need for complex logic  
**Cons**: Doesn’t account for server load

---

## Weighted Round Robin
- Like round robin, but assigns a weight to each server based on capacity
- A server with a weight of 2 gets twice as many requests as one with weight 1

**Use case**: Mixed-capacity servers  
**Con**: Needs manual tuning or monitoring feedback

---

## Least Connections
- Routes new requests to the server with the fewest active connections

**Best for**: Long-lived or variable-length requests (e.g., WebSocket, video stream)  
**Con**: Doesn’t consider server capacity unless weighted

---

## Weighted Least Connections
- Adds weight/capacity awareness to Least Connections strategy

**Use case**: Heterogeneous server environments with uneven traffic  
**Con**: Higher complexity

---

## IP Hash
- Hashes the client IP to determine which server handles the request

**Pros**: Sticky sessions without cookies  
**Cons**: Poor distribution if IPs aren’t diverse; problematic if number of servers changes

---

## Consistent Hashing
- Maps requests to servers based on a hash ring
- Minimizes disruption when servers are added/removed

**Use case**: Distributed caches (e.g. Memcached), sharded databases  
**Con**: Needs well-designed hash function

---

## Random (with or without weights)
- Selects a server randomly for each request
- May use weights to prefer higher-capacity servers

**Pros**: Simple and stateless  
**Cons**: Can be unbalanced without weights

---

## Geolocation-Based Routing
- Directs traffic to the closest data center or edge node based on the user's location

**Use case**: CDNs, global applications  
**Con**: May need DNS support and geo-aware infrastructure

---

## Latency-Based Routing
- Routes requests to the server with the lowest observed latency

**Used in**: AWS Route 53, Azure Traffic Manager  
**Con**: Needs real-time measurement infrastructure

---

## Least Response Time
- Chooses the server with the lowest average response time

**Good for**: Systems where latency matters more than connection count  
**Con**: Needs active monitoring

---

# Load Balancer Types

| Type                 | Description                                  | Examples                         |
|----------------------|----------------------------------------------|----------------------------------|
| Hardware LB          | Dedicated appliance                          | F5, Citrix                       |
| Software LB          | Runs on general-purpose servers              | NGINX, HAProxy, Envoy            |
| DNS-based LB         | DNS returns different IPs per request        | AWS Route 53, NS1                |
| Cloud LB (managed)   | Provided as a service                        | AWS ELB/ALB/NLB, Azure Load Balancer, GCP L7 LB |

---

# Load Balancing and Health Checks

All good load balancers periodically check the health of backend servers:
- HTTP/HTTPS probes
- TCP connection attempts
- Custom scripts

If a backend fails, it’s removed from the rotation until healthy again.

---

# Load Balancing + Failover

You can combine:
- Primary data center with local load balancing
- Global DNS or BGP failover to route across data centers

Also use **active-passive** or **active-active** architectures for reliability.

---

# Summary Table

| Strategy               | Stateful? | Load-Aware? | Use Case Example                              |
|------------------------|-----------|-------------|------------------------------------------------|
| Round Robin            | No        | No          | Evenly loaded web servers                     |
| Weighted Round Robin   | No        | Manual      | Unequal server capacities                     |
| Least Connections      | Yes       | Yes         | WebSocket server farm                         |
| IP Hash                | No        | No          | Sticky sessions without cookies               |
| Consistent Hashing     | No        | No          | Distributed caching or sharding               |
| Geolocation Routing    | No        | No          | Global CDN                                    |
| Latency-Based Routing  | No        | Yes         | Dynamic multi-region API                      |
