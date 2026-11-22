---
tags:
  - Architecture
---
The **publish/subscribe (pub/sub) model** is a messaging pattern where producers (publishers) send messages to a topic (such as an [[AWS Simple Notification Service (SNS)|SNS topic]]), without needing to know who the subscribers are. Consumers (subscribers) receive those messages by subscribing to the topic. This model decouples the publisher and subscriber, allowing the system to scale efficiently and enabling multiple subscribers to react to the same message independently. It is often used in distributed systems where real-time message delivery is required.

This model promotes decoupling between components, as publishers and subscribers operate independently, enhancing scalability and flexibility in distributed systems.

# Example
#TODO 