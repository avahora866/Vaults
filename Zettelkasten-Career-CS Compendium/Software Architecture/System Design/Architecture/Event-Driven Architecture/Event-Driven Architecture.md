---
tags:
  - Architecture
---
**Event-Driven Architecture** is a software design pattern where components of the system **communicate by producing and consuming events**. An _event_ represents a **change in state** or an **action** that occurred in the system.

This model promotes **loose coupling** between components, high **scalability**, and **asynchronous communication**.

---

# Key Concepts

## Event

An event is a **message that signals something happened**, usually described in past tense.

**Example**:

- `OrderPlaced`
- `UserRegistered`
- `PaymentProcessed`

An event typically has:

- **Type** (e.g., `UserRegistered`)
- **Payload** (data about the event)
- **Metadata** (timestamp, ID, source)

## Event Producer

- The component or service that **detects or causes the event** and **publishes** it.
- It does **not** know who will consume the event.

**Example**: A user service emits `UserCreated` after saving a user.

## Event Consumer

- A component or service that **subscribes to and reacts** to specific events.
- Can have one or many consumers per event.

**Example**: An email service listens to `UserCreated` and sends a welcome email.

## Event Broker / Message Bus

- An intermediary (like **Kafka**, **RabbitMQ**, **SNS/SQS**, or **EventBridge**) that **receives**, **routes**, and **delivers** events between producers and consumers.
- Enables **asynchronous** and **decoupled** communication.

---

# Types of Event-Driven Models

## Simple Event Notification

- Event just signals something happened.
- No data attached (or minimal).
- Consumer may need to fetch additional info.

**Example**: `UserCreated` (without user details)

## Event-Carried State Transfer

- Event includes enough data for the consumer to act without making additional calls.
- Most common in microservices.

**Example**: `UserCreated` with name, email, etc.

## Event Sourcing

- Every change in state is captured as an event.
- Instead of storing the current state, store **a sequence of events**.
- State is reconstructed by **replaying events**.

---

# Example Use Case: E-commerce Checkout

1. **Order Service** places an order → emits `OrderPlaced`
2. **Inventory Service** consumes `OrderPlaced` → updates stock
3. **Billing Service** consumes `OrderPlaced` → processes payment
4. **Shipping Service** consumes `OrderPlaced` → schedules delivery
5. **Notification Service** consumes `OrderPlaced` → sends confirmation email

Each service is independent and only reacts to events it cares about.

---

# Benefits

- **Loose coupling**: Services don’t need to know about each other.
- **Scalability**: Easily scale consumers independently.
- **Asynchronous**: No need to wait for others to respond.
- **Resilience**: One component’s failure doesn’t affect the whole system.
- **Extendability**: Add new consumers without changing existing code.

---

# Drawbacks

- **Eventual consistency**: Data sync isn’t immediate; consumers act _after_ the event.
- **Debugging complexity**: Hard to trace end-to-end flow.
- **Message ordering**: Harder to enforce strict order of processing.
- **Duplication**: Might need deduplication or idempotent consumers.
- **Error handling**: Must manage retries, poison messages, and dead letter queues.

---

# Patterns Related to EDA

- **Pub/Sub (Publish/Subscribe)**: One-to-many broadcast of events.
- **CQRS (Command Query Responsibility Segregation)**: Often uses events to sync read/write models.
- **Event Sourcing**: State is a result of stored events.
- **Saga Pattern**: Coordinates distributed transactions via event chains.


---

# When to Use

- You need **scalable, decoupled services**.
- System must **react to changes quickly** (e.g., real-time alerts, notifications).
- You want to support **extensible business workflows** (e.g., onboarding pipelines).
- **Multiple subsystems** must respond to the same event differently.

---

# Summary

**Event-Driven Architecture** enables:

- Systems to react to changes via events
- Loose coupling and better scalability
- Complex workflows to be handled by multiple services independently

But it requires careful handling of **event design**, **delivery guarantees**, and **monitoring**.