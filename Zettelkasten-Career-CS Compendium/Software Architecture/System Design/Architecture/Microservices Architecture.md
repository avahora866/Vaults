---
tags:
  - Architecture
---
Microservices architecture is a design approach where an application is structured as a collection of small, loosely coupled, independently deployable services. Each service:

- Focuses on a single business capability
- Owns its own data and logic
- Communicates with other services via lightweight protocols (usually HTTP/REST, [[gRPC|gRPC]], or messaging queues)

This architecture contrasts with the **monolithic architecture**, where all features are bundled into one large codebase and deployed together.

# Key Characteristics

- **Decentralized**: Each service can be developed, deployed, and scaled independently.
- **Domain-driven**: Each microservice focuses on a specific domain or business capability.
- **Technology agnostic**: Different services can use different languages, frameworks, and databases.
- **Fault isolation**: A failure in one service doesn’t necessarily bring down the whole system.
- **Independent deployments**: Teams can work on and release features without coordination overhead.

# Core Components

### Services

Each microservice encapsulates:

- Business logic
- Its own database
- APIs for interaction

### API Gateway

Acts as a single entry point for clients. It:

- Routes requests to appropriate services
- Handles authentication, logging, rate limiting

### Service Registry and Discovery

Keeps track of which services are available and how to reach them. Tools: Consul, Eureka, etc.

### Inter-service Communication

- **Synchronous**: REST APIs, [[gRPC|gRPC]]
- **Asynchronous**: Messaging queues (Kafka, RabbitMQ)

### Monitoring and Logging

Because services are distributed, centralized logging and monitoring (e.g., with ELK stack or Prometheus/Grafana) is critical.

# Example Scenario: E-Commerce Platform

Let’s break a simple e-commerce site into microservices:

### Microservices

**User Service** – Handles registration, login, profiles.
1. **Product Service** – Manages product listings and inventory.
**Order Service** – Handles order creation, tracking.
2. **Payment Service** – Processes payments and refunds.
**Notification Service** – Sends emails, SMS, push notifications.

### Architecture Flow

**User logs in** → Request goes to **API Gateway** → routed to **User Service**
1. **User browses products** → Gateway routes to **Product Service**
**User places an order**:
    - API Gateway sends order to **Order Service**
    - **Order Service** calls **Product Service** to check stock
    - If stock is sufficient, **Order Service** requests **Payment Service** to charge the user
    - Upon success, **Notification Service** sends confirmation email

### Database Per Service

Each service manages its own database:

- **User Service** → `users_db`
- **Product Service** → `products_db`
- **Order Service** → `orders_db`

This enforces **loose coupling** and **data encapsulation**.

# Advantages

- **Scalability**: Scale only the services that need it (e.g., Payment Service under load).
- **Fault isolation**: If Product Service fails, login and payment still work.
- **Faster deployments**: Independent services mean faster CI/CD pipelines.
- **Technology diversity**: Each team chooses best tech for their service.

# Disadvantages

- **Operational complexity**: Managing many services, deployments, and versions is harder.
- **Distributed system challenges**: Network latency, message reliability, consistency.
- **Data management**: Transactions across services are difficult (may need eventual consistency, Sagas).
- **DevOps maturity**: Requires good infrastructure, monitoring, and CI/CD tooling.

# Comparison to Monolithic Architecture

# When to Use Microservices

**Good for:**

- Large, complex applications
- Multiple independent teams
- Systems needing high scalability and resilience

**Avoid if:**

- The app is small and unlikely to grow
- You lack the DevOps and monitoring infrastructure
- Your team is small or inexperienced in distributed systems

# Code Example (Simplified)

Here's a Java Spring Boot REST controller for a **Product Service**:

```java
@RestController
@RequestMapping("/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    @GetMapping("/{id}")
    public ResponseEntity<Product> getProduct(@PathVariable Long id) {
        Product product = productService.findById(id);
        return ResponseEntity.ok(product);
    }

    @PostMapping
    public ResponseEntity<Product> createProduct(@RequestBody Product product) {
        Product created = productService.save(product);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
}
```

Each microservice would have similar endpoints and be deployed separately.

# Summary

Microservices offer great flexibility, scalability, and speed for large systems with complex domain logic and many contributors. But they come at the cost of increased architectural complexity and require mature DevOps practices. Many companies adopt **microservices gradually**, starting with a monolith and breaking it into services over time.