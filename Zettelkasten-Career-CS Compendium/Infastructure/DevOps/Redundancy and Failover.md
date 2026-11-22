---
tags:
  - Architecture
  - DevOps
  - "#LEAF"
---
**Redundancy and Failover:**  
 Redundancy involves having backup components or systems in place to prevent a single point of failure. This can include duplicating servers, databases, or network paths. Failover is the automatic switching to a standby system or component in the event of a failure in the primary one. These measures are designed to ensure that the system remains operational, even if some parts fail, providing continuous service and minimizing downtime.

**Circuit Breaker Pattern:**  
 The circuit breaker pattern is a fault tolerance mechanism that detects failures in a system and temporarily "breaks" the connection to the failing service. When a failure threshold is reached, the circuit breaker prevents further requests from being made, allowing the system to recover. After a predefined time, the circuit breaker allows limited traffic to test if the system has recovered, promoting stability and preventing cascading failures that could affect other parts of the system.

**Graceful Degradation:**  
 Graceful degradation refers to the practice of designing a system to continue operating at a reduced level of functionality when some components fail. Instead of a total system shutdown, certain features or services may be temporarily disabled, while core functionality remains intact. This ensures that users can still interact with the system, albeit with reduced features, improving the overall user experience during outages or partial failures.

