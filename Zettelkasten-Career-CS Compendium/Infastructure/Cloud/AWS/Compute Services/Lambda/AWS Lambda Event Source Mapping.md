---
tags:
  - FORMAT
  - AWS
---
# What is AWS Event Source Mapping?

**AWS Event Source Mapping** is a feature that connects an AWS **event source** (like SQS, DynamoDB Streams, or Kinesis) to an AWS **Lambda function**. It enables **automatic invocation** of the Lambda function whenever new events are available in the event source.

---

# How Event Source Mapping Works

1. **A Service Generates Events**

   * Example: An SQS queue receives messages, or a DynamoDB table records a new update.

2. **Lambda Polls the Event Source**

   * AWS Lambda **automatically** polls the event source at a configured interval.

   * It retrieves events in batches and invokes the Lambda function.

3. **Lambda Processes Events**

   * The Lambda function executes using the event data as input.

   * After processing, Lambda **acknowledges** the event, marking it as complete.

---

# Supported Event Sources

AWS Event Source Mapping works with **poll-based** services:

| Event Source | Invocation Mode |
| ----- | ----- |
| **Amazon SQS** (Standard & FIFO) | **At-Least-Once** |
| **Amazon Kinesis Data Streams** | **At-Least-Once, Ordered per Shard** |
| **Amazon DynamoDB Streams** | **At-Least-Once, Ordered per Shard** |

🛑 **Note:** API Gateway, S3, and SNS do **not** use Event Source Mapping. They invoke Lambda **directly** via push-based triggers.

---

# Example: Setting Up Event Source Mapping (AWS CLI)

To associate an **SQS queue** with a Lambda function:

```
aws lambda create-event-source-mapping \
    --function-name MyLambdaFunction \
    --event-source-arn arn:aws:sqs:us-east-1:123456789012:MyQueue \
    --batch-size 10
```

This tells Lambda to poll the **SQS queue** and invoke `MyLambdaFunction` whenever messages arrive.

---

# Event Source Mapping Parameters

| Parameter | Description |
| ----- | ----- |
| **Batch Size** | Number of events sent to Lambda in one invocation (1–10,000 for SQS, 1–1000 for Kinesis/DynamoDB). |
| **Parallelism** | Kinesis/DynamoDB invocations are parallel per **shard**. SQS allows concurrent message processing. |
| **Retry Behavior** | Lambda retries failed events automatically (configurable). |
| **Event Filtering** | Lambda can filter events before processing. |
| **Maximum Retry Attempts** | Limits how many times Lambda retries a failed event. |

---

# Benefits of AWS Event Source Mapping

✔ **Fully Managed Polling** – No need to write polling logic; AWS handles it.  
 ✔ **Scalability** – Lambda scales automatically based on event volume.  
 ✔ **Event Ordering** – Maintains order for Kinesis & DynamoDB Streams.  
 ✔ **Efficient Processing** – Batches events for optimized execution.

---

# When to Use AWS Event Source Mapping?

✅ **Message Queues (SQS):** Process messages asynchronously with retries.  
 ✅ **Streaming Data (Kinesis/DynamoDB Streams):** Real-time analytics and event-driven architectures.  
 ✅ **Event-Driven Processing:** Automate function execution when new data arrives.

Would you like a step-by-step example in Terraform or CloudFormation? 🚀

