---
tags:
  - FORMAT
  - SystemDesign
---
**CQRS** is an architectural pattern that separates **read operations (queries)** from **write operations (commands)** in a system.  
Rather than using the same data model and service methods for reading and writing data (as is typical in CRUD-based systems), CQRS splits them into **distinct models** and often **distinct paths** through the application.

The goal is to **optimize** and **specialize** parts of your system for their specific responsibilities:

- **Commands** modify state.
- **Queries** return data but do not change state.

# Why use CQRS?

CQRS offers several advantages:

- **Scalability**: Read and write workloads can be scaled independently.
- **Performance Optimization**: You can optimize queries separately from how you handle updates.
- **Security**: Different models and services for commands and queries can have different security rules.
- **Separation of Concerns**: Command-side models (domain models) focus on business logic and invariants, while query-side models focus on efficiently retrieving data.

It is especially useful in:

- Complex domains (where business logic and validation are intricate).
- Systems where **read load** is much higher than **write load**.
- Event-driven architectures.

# Core Concepts of CQRS

## Command

A **command** represents an **intention to change the system's state**.  
A command:

- Is a **request** to perform an action.
- Should **not** return any data except maybe a confirmation (like success/failure).
- Can be validated and rejected (e.g., "you cannot delete an order that is already shipped").

Example:

```java
public class CreateOrderCommand {
    private String customerId;
    private List<String> itemIds;
    private Address shippingAddress;
}
```

## Query

A **query** is a **request to retrieve data**.  
It:

- Does **not** change system state.
- Should be **side-effect free**.
- Can be optimized for fast reads (e.g., denormalized views, caching).

Example:

```java
public class GetOrderDetailsQuery {
    private String orderId;
}
```

## Responsibility Segregation

In CQRS:

- The **command model** contains the logic to **validate**, **execute**, and **persist** changes.
- The **query model** is designed to **fetch** data efficiently and may even use a different data store or denormalized structures for performance.

# CQRS and Event Sourcing

CQRS often goes hand-in-hand with **Event Sourcing** but **they are not the same**.

**Event Sourcing** means you **store the history of changes** (events) rather than just the current state.

When combined:

- A command results in events being produced.
- The event is stored.
- The current state is reconstructed by replaying events.

This strengthens auditability and complex state reconstruction, but adds complexity.

# High-Level Architecture Diagram (Simple)

```
Client
  |
[Command Handler]  ->  [Write Model (Domain Model)]  ->  [Write Database]
  |
[Query Handler]    ->  [Read Model (Optimized Views)] -> [Read Database]
```

# Example Flow

**Command Flow**:

- Client sends a `CreateOrderCommand`.
- The `CommandHandler` validates and applies domain rules.
- If valid, it persists the new order in the database.

**Query Flow**:

- Client sends a `GetOrderQuery`.
- The `QueryHandler` fetches the order information from the read database or read-optimized view.
- Returns data without modifying anything.

# CQRS vs CRUD

# Challenges and Trade-offs

While CQRS offers powerful benefits, it comes with trade-offs:

- **Increased Complexity**: More layers, more models.
- **Eventual Consistency**: In distributed systems, write and read models might not be immediately synchronized.
- **Higher Maintenance**: Keeping two models (command and query) synchronized can require additional development effort.
- **Tooling and Frameworks**: May need special libraries or frameworks if adopting event sourcing.

Thus, CQRS is typically recommended **only for systems that have complexity justifying the extra effort**.

# When to use CQRS

**Good use cases**:

- High-scale applications (millions of reads vs relatively few writes).
- Collaborative domains (e.g., financial systems, order management, inventory management).
- Systems where audit trails are critical.
- Applications needing extreme performance optimization on queries.

**Bad use cases**:

- Simple CRUD applications.
- Early-stage startups (where complexity costs outweigh scalability needs).

# Summary

**CQRS** is about **splitting command and query responsibilities** into different models to **optimize, secure, and scale** complex systems better.  
It is often used in combination with **event sourcing** but can be used without it.  
While powerful, it should be applied judiciously, as it introduces complexity.