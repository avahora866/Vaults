---
tags:
  - FORMAT
  - SystemDesign
---
# Scalability

## Definition
Scalability is a system's ability to **handle increased load** by **adding resources** (hardware, servers, etc.).

## Types
| Type               | Description                             | Example                                  |
|--------------------|------------------------------------------|-------------------------------------------|
| Vertical Scaling    | Add more power (CPU, RAM) to one node   | Upgrade server from 4-core to 32-core     |
| Horizontal Scaling  | Add more nodes to distribute the load   | Add more servers behind a load balancer   |

## Characteristics
- Performance increases with load
- Must maintain acceptable latency and throughput
- Often involves stateless services and distributed architectures

## Key Strategies
- Load balancing (e.g. NGINX, HAProxy)
- Partitioning (e.g. DB sharding)
- Caching (Redis, Memcached, CDN)
- Asynchronous processing (message queues, task queues)

---

# Reliability

## Definition
Reliability is the system’s ability to **perform correctly and consistently** over time, even in the face of faults.

## Characteristics
- No data corruption
- Accurate output
- Consistent performance over time
- Tolerates hardware/software faults

## Reliability ≠ Uptime  
A system may be available (online) but not reliable (returns incorrect results).

## Key Techniques
- Input validation and error handling
- Retry logic with backoff
- Idempotent operations
- Redundant systems (failover mechanisms)
- Data replication and checksums

## Real-World Examples
- Payment system that ensures no double charge (idempotency)
- Cloud storage that verifies data integrity with checksums

---

# Availability

## Definition
Availability is the **percentage of time a system is operational** and able to serve requests.

## Formula
```
Availability = (Uptime) / (Uptime + Downtime)
```

| Availability % | Downtime/year       |
|----------------|---------------------|
| 99%            | ~3.65 days           |
| 99.9%          | ~8.76 hours          |
| 99.99%         | ~52.6 minutes        |
| 99.999%        | ~5.26 minutes        |

## Key Techniques
- Redundancy: Replicate services across zones or regions
- Health checks and automated failover
- Load balancers with node awareness
- Graceful degradation (serve partial functionality)

## Example
- Multi-region DNS with automatic routing to healthy regions
- Active-active replication for critical services

---

# How They Relate

| Aspect       | Question Answered                                      |
|--------------|--------------------------------------------------------|
| Scalability  | Can the system grow with increased demand?             |
| Reliability  | Does the system function correctly under stress/failure?|
| Availability | Is the system usable when needed?                      |

- **Scalability** supports availability under high load.
- **Reliability** ensures availability doesn’t mean broken behavior.
- High **availability** requires reliable and scalable architecture.

---

# Design Considerations & Tradeoffs

## CAP Theorem
In distributed systems, you can only pick 2 of 3:
- **Consistency** (same data everywhere)
- **Availability** (always responds)
- **Partition Tolerance** (works if network splits)

e.g.,  
- CP systems prioritize reliability (e.g. Zookeeper)  
- AP systems prioritize availability (e.g. DynamoDB)

## Tradeoffs
- Increasing availability may reduce consistency (eventual consistency)
- Scaling horizontally may reduce reliability unless carefully architected
- Achieving all three requires cost and complexity

---

# Practical Tools & Patterns

| Tool/Service     | Helps With                  | Example Use                                      |
|------------------|-----------------------------|--------------------------------------------------|
| Load Balancer    | Scalability, Availability   | Distributes traffic across servers               |
| Auto-scaling     | Scalability                 | Adds/removes servers based on load               |
| Caches (Redis)   | Scalability, Availability   | Reduces DB load and latency                      |
| Queues (Kafka)   | Scalability, Reliability    | Decouples services, enables retry                |
| CDN              | Availability, Scalability   | Offloads static content, reduces latency         |
| Circuit Breakers | Reliability                 | Prevents system-wide failure during outages      |
| Monitoring (Prometheus, Grafana) | All 3         | Alerting and metrics collection                  |

---

# Summary Table

| Concept       | Focus                    | Goal                             | Metric Examples               |
|---------------|--------------------------|----------------------------------|-------------------------------|
| Scalability   | Growth under load        | Handle more users/data/workload | Latency, QPS, throughput       |
| Reliability   | Correctness under stress | Avoid bugs, data corruption     | Error rate, retries, SLA bugs |
| Availability  | Uptime                   | Be up when needed               | % uptime, incident duration   |
