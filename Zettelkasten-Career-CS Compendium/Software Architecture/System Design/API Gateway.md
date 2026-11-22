---
tags:
  - FORMAT
  - SystemDesign
---
An API gateway is a **single entry point** for client applications to access backend services. Instead of clients calling each microservice or backend API directly, they send requests to the gateway, which then routes, transforms, or aggregates the requests before passing them to the correct service.

Think of it as a **reverse proxy with extra intelligence**.

---

# What an API gateway does
- **Request routing**
    - Directs incoming API calls to the correct backend service.
- **Aggregation**
    - Combines responses from multiple services into one (avoiding multiple client calls).
- **Protocol translation**
    - Converts between protocols (e.g., HTTP ↔ WebSocket, REST ↔ gRPC).
- **Security**
    - Handles authentication, authorization, TLS termination, rate limiting, and IP whitelisting.
- **Traffic management**
    - Load balances requests, applies throttling, and manages retries/failover.
- **Monitoring and logging**
    - Centralizes logging, tracing, and metrics for all requests.

---

# Why use an API gateway

- **Simplifies clients**: Clients only talk to one endpoint instead of many services.
- **Decouples clients from services**: Backend services can evolve without breaking clients.
- **Centralized policy enforcement**: Security and rate limits are managed in one place.
- **Improves performance**: By caching, aggregating requests, and reducing round trips.

---

# Common use cases

- **Microservices architectures**: Where many small services need a single access point.
- **Mobile/IoT apps**: Where clients benefit from simplified endpoints.
- **Hybrid/multi-cloud**: Where backends are spread across different environments.

---

# Examples of API gateways

- AWS API Gateway
- Kong
- NGINX (with gateway modules)
- Apigee
- Istio (service mesh with gateway features)

---

In short:

- A **load balancer** just distributes traffic.
- An **API gateway** distributes traffic _and_ handles authentication, transformation, aggregation, monitoring, and security policies.