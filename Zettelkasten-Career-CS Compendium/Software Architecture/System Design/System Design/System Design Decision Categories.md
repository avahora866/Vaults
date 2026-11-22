---
tags:
  - FORMAT
  - SoftwareDesign
---
# Data & Storage

This category focuses on how and where data is stored and managed. It includes choices around databases (relational or NoSQL), data modeling, schema design, caching strategies, and how data is persisted, indexed, queried, and expired. It also includes decisions about consistency, availability, and how data is backed up or migrated.

---

# Code & Business Logic

This covers the structure and behavior of your core application logic. It includes decisions around how to organize code, where logic lives (e.g., services, controllers, domain layers), and how you separate concerns. It also deals with error handling strategies, code reuse patterns, and application-level abstractions.

---

# API & External Interfaces

Focuses on how your system communicates with users or other systems. This includes designing RESTful or GraphQL APIs, defining routes, shaping request/response payloads, choosing protocols (HTTP, [[gRPC|gRPC]]), handling authentication, and managing versioning and documentation. It also covers rate limiting, input validation, and interface contracts.

---

# Infrastructure & Deployment

This category involves everything that supports the app operationally: where and how it's hosted, how it's deployed, and the tools and environments used. It includes containerization (e.g., Docker), cloud services (e.g., AWS, GCP), CI/CD pipelines, secrets management, configuration, and how you scale infrastructure as demand grows.

---

# Performance & Scalability

Covers how your system performs under load and how it grows to meet demand. This includes decisions on caching strategies (in-memory or distributed), database indexing, load balancing, batching or queuing operations, and optimizing for latency or throughput. It also deals with resource limits, response time targets, and capacity planning.

---

# Observability & Monitoring

Concerns visibility into the system’s behavior in production. This includes logging strategies, metrics collection, health checks, tracing (e.g., OpenTelemetry), dashboards, and alerting mechanisms. The goal is to make sure the system can be understood, debugged, and improved based on live data.

---

# Security & Access Control

Deals with protecting your system from misuse or compromise. This includes authentication and authorization (who can do what), input validation, data encryption (at rest and in transit), API rate limits, abuse protection (e.g., spam detection), and data privacy. It also includes auditing and permissions systems.

---

# Product/UX Behavior

Covers decisions that affect how users experience the system. This includes how features behave, what defaults are used, timeouts, UI-driven constraints, and anything shaped by business logic or user expectations. It often connects to non-functional requirements like reliability, feedback, or accessibility.

---

