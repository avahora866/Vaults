---
tags:
  - AWS
---
Amazon EventBridge is a serverless event bus service that enables applications to communicate by passing events between them in a loosely coupled way. It simplifies building [[Event-Driven Architecture|event-driven]] architectures by allowing you to connect applications, AWS services, and [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS providers]] through a centralized event bus. EventBridge helps route events to targets like [[AWS Lambda|AWS Lambda]], [[AWS Simple Queue Service (SQS)|SQS]], Step Functions, or other AWS services, based on rules you define.

---

### Core Features of Amazon EventBridge

1. **Event Bus**:  
   * An event bus is a pipeline where events are sent and routed to targets. EventBridge provides:  
     * **Default Event Bus**: Captures events from AWS services.  
     * **Custom Event Buses**: Allows applications to publish their own events.  
      Partner Event Buses: Receives events directly from supported [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS]] applications.  
2. **Event Sources**:  
   * Events can originate from:  
     * AWS services (e.g., EC2, [[AWS S3|S3]], [[AWS DynamoDB|DynamoDB]]).  
     * Custom applications (via the EventBridge API).  
      [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS]] applications integrated with EventBridge (e.g., Zendesk, Datadog).  
3. **Rules**:  
   * Rules define how events are routed. You can filter events using JSON-based patterns and send them to multiple targets.  
   * Rules are highly flexible, allowing specific actions based on event content.  
4. **Targets**:  
   * Supported targets include [[AWS Lambda|AWS Lambda]], [[AWS Simple Notification Service (SNS)|SNS]], [[AWS Simple Queue Service (SQS)|SQS]], Step Functions, Kinesis, and more. EventBridge can also invoke APIs through [[AWS API Gateway|API Gateway]].  
5. **Event Filtering**:  
   * EventBridge allows you to filter and process only relevant events using patterns in rules. This reduces the need for additional processing downstream.  
6. **Schema Registry**:  
   * EventBridge includes a schema registry to discover, store, and reuse schemas (event structures), making it easier for developers to integrate and use events in their applications.  
7. **Reliability and Scalability**:  
   * Automatically scales to handle large volumes of events and ensures delivery with a high level of reliability.  
8. **Monitoring and Security**:  
   * Integrates with Amazon CloudWatch for event tracking and monitoring.  
   * Supports fine-grained access control with [[Identity and Access Management (IAM)|AWS IAM]].

---

### How Amazon EventBridge Works

1. **Event Creation**:  
    An event source (e.g., [[AWS S3|S3]], a custom app, or a [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS]] service) generates an event in JSON format and sends it to an EventBridge event bus.  
2. **Event Filtering**:  
   * EventBridge evaluates the event against rules to determine which targets should receive it.  
3. **Event Routing**:  
   * The matched event is routed to the specified targets, such as a [[AWS Lambda|Lambda function]] or an [[AWS Simple Queue Service (SQS)|SQS queue]].  
4. **Event Processing**:  
   * The target processes the event and performs the desired action, such as triggering an alert, starting a workflow, or updating a database.

---

### Use Case Examples

1. **Serverless Applications**:  
   * Use EventBridge to trigger [[AWS Lambda|Lambda functions]] in response to changes in data or state. For example, automatically process uploaded files to [[AWS S3|S3]].  
2. **Application Integration**:  
   * Connect multiple [[Microservices Architecture|microservices]] by passing events between them without tight coupling.  
3. [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS Integration]]:  
   * Receive and respond to events from SaaS applications like Stripe or Zendesk without building custom polling or webhook solutions.  
4. **Data Processing Pipelines**:  
   * Create workflows that process data in real-time using services like Kinesis, Step Functions, and [[AWS Lambda|AWS Lambda]].  
5. **Cross-Account Event Sharing**:  
   * Share events securely between AWS accounts to enable multi-account architectures.

---

### Benefits of Amazon EventBridge

1. **Simplified** [[Event-Driven Architecture|Event-Driven Architecture]]:  
   * Eliminates the need for complex polling or custom integrations by providing a central event bus.  
2. **Scalability and Reliability**:  
   * Automatically scales to handle millions of events with low latency and high availability.  
3. **Cost-Effective**:  
   * Pay only for the events published, matched, and delivered.  
4. **Broad Integration**:  
    Works seamlessly with AWS services and many third-party [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS]] applications.  
5. **Decoupling**:  
   * Allows services to operate independently, reducing dependencies and improving maintainability.  
6. **Flexibility**:  
   * Supports custom applications and multiple event buses, enabling tailored workflows.

---

### Comparison with Amazon SNS

| Feature | EventBridge | SNS |
| ----- | ----- | ----- |
| **Event Filtering** | Advanced filtering with rules and patterns | Basic topic-based subscription |
| **Targets** | Supports 18+ targets, including Step Functions | Primarily delivers messages to subscribers |
| **Schema Registry** | Built-in schema registry for event discovery | Not supported |
| **Integration** | Supports AWS services, SaaS apps, and custom apps | AWS services and custom apps via HTTP/S |
| **Event Delivery** | Event bus routes events to multiple targets | Messages are broadcast to all subscribers |

---

Amazon EventBridge is a powerful service for building modern, [[Event-Driven Architecture|event-driven]] applications. It enables seamless integration between AWS services, custom applications, and external [[IaaS, PaaS, and SaaS#Software as a Service (SaaS)|SaaS]] platforms while decoupling components for greater scalability and flexibility.

