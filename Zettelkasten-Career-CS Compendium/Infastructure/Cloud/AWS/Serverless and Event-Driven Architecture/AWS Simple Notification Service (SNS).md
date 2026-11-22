---
tags:
  - AWS
---
Amazon Simple Notification Service (SNS) is a fully managed messaging service that allows you to send notifications or messages to multiple recipients (subscribers) using a publish-subscribe (pub/sub) model. It is designed for high-throughput, low-latency communication and is commonly used for broadcasting notifications, application integration, and [[Event-Driven Architecture|event-driven]] systems.

---

# Core Features of Amazon SNS

1. Topics:  
    A topic is a logical access point that acts as a communication channel.  
    Publishers send messages to topics, and subscribers receive messages from topics.  
2. Message Delivery:  
    Messages published to a topic are delivered to all topic subscribers.  
    Supports multiple delivery protocols, including:  
      [[AWS Simple Queue Service (SQS)|Amazon SQS]]: For queuing messages.  
      [[AWS Lambda|AWS Lambda]]: For serverless processing.  
      HTTP/HTTPS: To invoke endpoints.  
      Email/Email-JSON: For email notifications.  
      SMS: For text messages.  
      Mobile Push: For notifications to iOS, Android, and other mobile platforms.  
3. Message Filtering:  
    Subscribers can define message filters to receive only relevant messages, reducing processing overhead.  
4. Fan-Out Pattern:  
    SNS allows messages to be sent to multiple recipients (e.g., [[AWS Simple Queue Service (SQS)|SQS queues]] or [[AWS Lambda|Lambda functions]]), enabling a fan-out architecture.  
5. High Throughput and Scalability:  
    Designed to handle millions of messages per second with automatic scaling.  
6. Durable Storage:  
    Messages are stored across multiple availability zones for durability during transit.  
7. Monitoring and Security:  
    Integrates with [[AWS CloudWatch|AWS CloudWatch]] for monitoring message delivery metrics.  
    Supports encryption at rest using [[AWS Key Management Service (KMS)|AWS KMS]] and in-transit security using HTTPS.

---

# How Amazon SNS Works

1. Publishers:  
    Applications or services publish messages to an SNS topic.  
2. Topics:  
    The topic acts as an intermediary that broadcasts messages to all subscribed endpoints.  
3. Subscribers:  
    Services, applications, or users subscribe to the topic and receive messages using their chosen protocol.

---

# Example Use Cases

1. Broadcast Notifications:  
    Use SNS to send email or SMS notifications to users when a specific event occurs (e.g., system outages, promotional alerts).  
2. [[Event-Driven Architecture|Event-Driven Architectures]]:  
    Publish events to SNS, which then triggers downstream services like [[AWS Lambda|Lambda functions]] or [[AWS Simple Queue Service (SQS)|SQS queues]] for further processing.  
3. Application Integration:  
    Enable communication between [[Microservices Architecture|microservices]] or decoupled application components.  
4. Mobile Push Notifications:  
    Send push notifications to mobile apps using SNS’s integration with platform push notification services like Firebase Cloud Messaging (FCM).

---

# Benefits of Amazon SNS

1. Scalable and Reliable:  
    Automatically scales to handle large message volumes while ensuring high availability.  
2. Low Latency:  
    Messages are delivered in near real-time to subscribers.  
3. Flexibility:  
    Supports multiple delivery protocols, allowing integration with diverse systems.  
4. Cost-Effective:  
    Pay only for the messages you publish and deliver.  
5. Decoupling:  
    Enables services to communicate without being tightly integrated.

---

# Comparison with Amazon SQS

| Feature | Amazon SNS | Amazon SQS |
| ----- | ----- | ----- |
| Message Model | Pub/Sub (one-to-many) | Queue (one-to-one) |
| Message Persistence | Not stored after delivery | Messages are retained until consumed |
| Use Case | Broadcast notifications, event-driven systems | Message queuing, decoupling producers/consumers |
| Fan-Out | Sends messages to multiple subscribers | Requires integration with SNS for fan-out |

---

# Integration Examples

1. [[Event-Driven Architecture|Event-Driven]] Systems:  
    Publish events from a web application to an SNS topic. The topic notifies [[AWS Lambda|Lambda functions]] and [[AWS Simple Queue Service (SQS)|SQS queues]] for further processing.  
2. Notification System:  
    Use SNS to send SMS alerts to users when an order status changes or a system error occurs.  
3. IoT Systems:  
    IoT devices publish telemetry data to an SNS topic. Subscribers process and store the data in services like [[AWS DynamoDB|DynamoDB]] or [[AWS S3|S3]].

---

Amazon SNS is a versatile service that simplifies building scalable, event-driven architectures and delivering notifications to diverse endpoints.

