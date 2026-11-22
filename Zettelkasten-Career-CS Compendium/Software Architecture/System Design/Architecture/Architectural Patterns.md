---
tags:
  - Architecture
---
# Layered Architecture
[[Layered Architecture|Layered Architecture (e.g., 3-layer architecture)]] organizes an application into separate layers, typically Presentation (UI), Business Logic, and Data Access, to enhance maintainability, scalability, and separation of concerns. Each layer interacts only with adjacent layers, making it easier to modify or replace components independently.

# Microservices Architecture
[[Microservices Architecture|Microservices Architecture]] decomposes applications into small, independently deployable services, each handling a specific business function and communicating via APIs. This approach improves scalability, resilience, and development speed but increases complexity in service orchestration #NOTES   and data consistency #NOTES  .

# Event-Driven Architecture
[[Event-Driven Architecture|Event-Driven Architecture]] is based on asynchronous communication where components produce and consume events, allowing systems to be loosely coupled and highly scalable. This is useful for real-time processing, but it requires careful event handling and coordination to maintain consistency.

# Client-Server Architecture
[[Client-Server Architecture|Client-Server Architecture]] separates systems into clients (which request services) and servers (which provide them), enabling distributed computing . This model is fundamental to web applications, ensuring centralized control and scalability, though it can create bottlenecks if the server is overwhelmed.
# Monolithic vs Distributed Systems
[[Monolithic Architecture|Monolithic Architecture]] vs. [[Distributed Systems]] compares two approaches: monolithic systems bundle all functionality into a single codebase, making deployment simpler but limiting scalability, while distributed systems break functionality across multiple nodes or services, improving scalability and resilience but introducing complexity in communication and coordination.

