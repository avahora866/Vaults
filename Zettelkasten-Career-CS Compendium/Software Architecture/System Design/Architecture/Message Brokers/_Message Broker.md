---
tags:
  - Architecture
---
A **message broker** is a software component that facilitates communication between different applications, services, or systems by **handling the sending, receiving, and routing of messages**. It enables asynchronous messaging, meaning the sender and receiver do not need to interact in real time.

# How Message Brokers Work

1. **Producers (Senders)** generate messages and send them to the message broker.
2. The **message broker** receives the message, processes it, and determines the appropriate recipient(s).
3. **Consumers (Receivers)** retrieve the message when they are ready to process it.

This setup allows systems to be **loosely coupled**, meaning they can operate independently without knowing the details of each other.

# Types of Messaging Patterns in Message Brokers

1. **Point-to-Point (Queue-Based Messaging)**
    - Messages are placed in a queue, and a single consumer picks up and processes each message.
    - Example: **Order processing in an e-commerce system.**
2. **Publish-Subscribe (Pub/Sub)**
    - A producer sends a message to a **topic**, and multiple consumers subscribed to that topic receive the message.
    - Example: **Sending notifications to multiple users when a new blog post is published.**

# Examples of Message Brokers

- [[Apache Kafka]] – Distributed, high-throughput event streaming.
- **RabbitMQ** – Lightweight, queue-based messaging.
- **ActiveMQ** – Supports multiple messaging protocols.
- **Redis Pub/Sub** – Lightweight in-memory messaging.