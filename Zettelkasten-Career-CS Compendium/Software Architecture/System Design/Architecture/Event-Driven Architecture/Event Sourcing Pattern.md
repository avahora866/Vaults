---
tags:
  - SoftwareDesign
---
**Event Sourcing** is an architectural pattern where **state changes** in a system are **not stored as the final state**, but instead as a **sequence of events** that led to that state.

Instead of persisting the current state of an object directly (like saving a database row representing a User or an Order), **you record every change** (event) that has happened to the object, in the order they occurred.  
The **current state** is then reconstructed **by replaying those events**.

# Key Concepts of Event Sourcing

## Event

An **event** is a **fact** that something happened in the system.  
An event:

- Describes **what happened** (not what someone _wants_ to happen, that's a command).
- Is **immutable** (it cannot be changed once recorded).
- Is **persisted** to an event store (a database optimized for storing and retrieving events).

Example events:

- `UserRegistered`
- `OrderPlaced`
- `ProductShipped`
- `PaymentReceived`

Each event typically contains:

- A **timestamp**
- A **type** (what kind of event it is)
- **Data** about the event (payload)

Example Java event:

```java
public class OrderPlacedEvent {
    private final String orderId;
    private final String customerId;
    private final List<String> productIds;
    private final LocalDateTime occurredAt;

    public OrderPlacedEvent(String orderId, String customerId, List<String> productIds, LocalDateTime occurredAt) {
        this.orderId = orderId;
        this.customerId = customerId;
        this.productIds = productIds;
        this.occurredAt = occurredAt;
    }

    // Getters...
}
```

## Aggregate

An **aggregate** is the core object that:

- Receives commands.
- **Validates** and **applies events** to itself.
- Maintains state by replaying all its events.

The aggregate contains **only transient in-memory state** — the true record is the sequence of events.

## Event Store

An **event store** is a special type of database that:

- Appends new events sequentially.
- Returns all events for a given aggregate when asked.
- Guarantees event immutability and order.

An event store can be a specialized database or even a relational table structured like:

# How Event Sourcing Works

1. **Command**: The user sends a command (e.g., `PlaceOrder`).
2. **Aggregate**: The aggregate processes the command, validates it, and produces one or more events.
3. **Event Store**: The event(s) are saved to the event store.
4. **Apply Events**: The aggregate updates its in-memory state based on the event.
5. **Publishing (Optional)**: The event might be published to notify other systems (in a distributed architecture).

# Rebuilding State

Whenever the system needs the current state of an object:

- It **replays all past events** for that object **in order**.
- Applying each event **in sequence** results in the latest state.

Formally, if you have events:

```latex

E_1, E_2, ..., E_n
```

You can reconstruct state by:

```latex

S = apply(E_n, apply(E_{n-1}, ... apply(E_1, initialState) ... ))
```

## Performance Considerations

Rebuilding the state by replaying all events can be inefficient if there are a large number of events. To address this, systems often use snapshots:

Snapshots: You take periodic snapshots of the current state at specific points in time. When rebuilding the state, instead of replaying all events from the start, you can load the most recent snapshot and replay only the events that occurred after that snapshot.

This makes rebuilding the state more efficient, especially for systems with a lot of events.


```
Example: Account Balance Calculation

Assume we have a UserAccount aggregate and events like:

AccountCreated

MoneyDeposited(amount)

MoneyWithdrawn(amount)

```

If we have 1 million deposit and withdrawal events, rebuilding the balance would require replaying all 1 million events. By taking snapshots periodically (e.g., every 100 events), you reduce the number of events to replay. For instance, you'd load the last snapshot and then replay just the last 100 events.

# Simple Example

Imagine an order system.

- `OrderPlaced`
- `ItemAdded`
- `OrderShipped`

Rather than saving an `Order` table with a status field, you'd save the three events.  
When the system needs to know if the order has shipped, it **replays** the events to reconstruct that knowledge.

# Event Sourcing vs Traditional CRUD

# Benefits of Event Sourcing

- **Auditability**: Full history of changes available.
- **Debugging**: Easier to understand what went wrong by inspecting event history.
- **Flexibility**: Can project multiple different views from the same event stream (read models).
- **Temporal Queries**: Can see the state at any point in the past ("show me how this looked last Monday").
- **Integration**: Easier to publish domain events to other services.

# Challenges of Event Sourcing

- **Complexity**: Replaying events, versioning events, and maintaining event schemas over time.
- **Event Versioning**: As the business evolves, old event formats may become outdated and need migration strategies.
- **Querying**: You can’t easily query the current state directly from the event store; you often need **projections**.
- **Tooling**: Traditional ORM tools (like Hibernate) are often not helpful.

# Typical Architecture Overview

```
[Client] 
    |
[Command Handler] 
    |
[Aggregate Root]
    |
[Event Store (append-only)]
    |
[Projector (optional)] -> [Read Model (e.g., SQL, NoSQL, ElasticSearch)]
```

# Relation to CQRS

Event Sourcing often complements **CQRS**:

- Commands mutate aggregates and emit events.
- Events are stored.
- Queries read from optimized **read models** built by **projecting** events.

In a CQRS+Event Sourcing system:

- Write-side handles **commands + events**.
- Read-side handles **queries** from **materialized views**.

# When to Use Event Sourcing

**Good use cases**:

- Systems needing **full audit trail** (e.g., banking, insurance, healthcare).
- Systems where **rebuilding the past** is useful.
- Systems designed to be **highly reactive** (microservices, event-driven architectures).

**Bad use cases**:

- Simple CRUD systems.
- Systems where storage space is highly constrained and historical data isn't valuable.

# Summary

**Event Sourcing** is about **storing every change** to the system as an immutable sequence of events rather than the current state.  
It offers **auditability, flexibility, and temporal insights**, but at the cost of **higher complexity** and **event management overhead**.  
It's most powerful when combined with patterns like **CQRS** in domains that truly need these advanced capabilities.
