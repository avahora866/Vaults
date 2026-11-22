---
tags:
  - Architecture
---
# RESTFull
[[Rest|RESTful API design]]: RESTful API design is a set of principles and constraints for building web services that communicate over HTTP. It uses standard HTTP methods like GET, POST, PUT, and DELETE to perform operations on resources, which are represented by URLs. RESTful APIs are stateless, meaning each request from a client to a server contains all the necessary information for processing. They are widely adopted due to their simplicity, scalability, and ease of integration with web applications.

# GraphQL
[[GraphQL]]: GraphqQL is a query language for APIs and a runtime for executing those queries. Unlike REST, which exposes multiple endpoints for different resources, GraphQL provides a single endpoint where clients can request specific data in a flexible way. Clients can define the structure of the response, asking for only the fields they need, reducing over-fetching and under-fetching of data. It also allows for real-time data fetching through subscriptions.

# Messaging Queues
**Messaging queues (e.g., RabbitMQ, Kafka)**: Messaging queues, such as RabbitMQ and Kafka, enable asynchronous communication between systems by decoupling producers and consumers of messages. Producers send messages to a queue, and consumers process them in the order they arrive. RabbitMQ is a traditional message broker that supports various messaging patterns, while Kafka is designed for high-throughput, distributed event streaming and log-based architectures. Both technologies are used to handle heavy loads, improve system reliability, and enable scalable, [[Event-Driven Architecture|event-driven systems]].

# API Gateways
**API Gateways**: An API Gateway is an intermediary layer between clients and backend services, providing a unified entry point for multiple APIs. It is responsible for tasks such as routing requests to the appropriate service, load balancing, authentication, rate limiting, and logging. By using an API Gateway, organizations can simplify their infrastructure, centralize cross-cutting concerns, and improve the maintainability of their [[Microservices Architecture|microservices]] architecture. It also enhances security by acting as a single point for enforcing policies.

