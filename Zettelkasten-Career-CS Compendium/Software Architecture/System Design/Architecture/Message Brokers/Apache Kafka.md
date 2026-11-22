---
tags:
  - SystemDesign
---
Apache Kafka is a distributed event streaming platform designed to handle high-throughput, fault-tolerant, real-time data feeds. Originally developed at LinkedIn and later donated to the Apache Software Foundation, Kafka has become a cornerstone technology for building real-time data pipelines and streaming applications.

# Core Concepts

## Event Streaming
Kafka treats data as a continuous, never-ending stream of events. Each event represents something that happened in the world (like a user click, a sensor reading, or a business transaction).

## Topics
Topics are the fundamental organizational unit in Kafka, similar to folders in a file system or tables in a database. Each topic represents a particular stream of data.

## Partitions
Each topic is divided into partitions, which allow Kafka to scale horizontally. Partitions are the unit of parallelism in Kafka.

## Producers and Consumers
- **Producers**: Applications that publish (write) events to Kafka topics
- **Consumers**: Applications that subscribe to (read) topics and process the events

## Brokers
Kafka servers are called brokers. Multiple brokers form a Kafka cluster, providing redundancy and fault tolerance.

# Architecture

```
Producers → [Kafka Cluster (Brokers)] → Consumers
                     ↑
              [ZooKeeper/KRaft]
```

# Example Scenario

Let's walk through an e-commerce example:

## Setting Up Topics

```bash
# Create a topic for order events with 3 partitions and replication factor 2
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 \
                   --replication-factor 2 \
                   --partitions 3 \
                   --topic order-events
```

## Producer Code (Java)

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

Producer<String, String> producer = new KafkaProducer<>(props);

// Create an order event
JSONObject orderEvent = new JSONObject();
orderEvent.put("order_id", "12345");
orderEvent.put("customer_id", "C789");
orderEvent.put("amount", 99.95);
orderEvent.put("items", Arrays.asList("book", "headphones"));
orderEvent.put("timestamp", System.currentTimeMillis());

// Send the event to Kafka
ProducerRecord<String, String> record = 
    new ProducerRecord<>("order-events", orderEvent.toString());
producer.send(record);
producer.close();
```

## Consumer Code (Java)

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "order-processing-group");
props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Collections.singletonList("order-events"));

while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        System.out.println("Received order: " + record.value());
        JSONObject order = new JSONObject(record.value());
        // Process the order
        processOrder(order);
    }
}
```

# Key Features and Benefits

## High Throughput
Kafka can handle millions of messages per second, making it suitable for high-volume data such as tracking user activity, logs, and IoT sensor data.

## Durability and Reliability
Data is persisted to disk and replicated across multiple brokers, providing fault tolerance and high availability.

## Scalability
Easily scale horizontally by adding more brokers to a cluster or more partitions to a topic.

## Retention
Kafka can store data for configurable periods, allowing for replay and historical analysis.

## Ecosystem
Rich ecosystem of connectors (Kafka Connect) for integrating with other systems and stream processing libraries (Kafka Streams, KSQL).

# Common Use Cases

**Messaging System**: Replacing traditional message brokers with higher throughput and durability
1. **Activity Tracking**: Collecting user interaction data for analytics
**Metrics Collection**: Gathering operational data from distributed systems
2. **Log Aggregation**: Centralizing logs from multiple services
**Stream Processing**: Real-time data transformation and analysis
3. **Event Sourcing**: Capturing all changes to application state as a sequence of events

# Differences from Traditional Messaging Systems

- Designed for high throughput rather than just reliability
- Built-in partitioning for scalability
- Persistent storage with configurable retention
- Supports both queue (one consumer per message) and publish-subscribe (broadcast to all consumers) patterns

Kafka's combination of durability, scalability, and performance has made it a fundamental component in modern data architectures, especially for real-time data processing and microservices communication.