---
tags:
  - FORMAT
  - AWS
---
# What is Lambda RPC?

**Lambda RPC (Remote Procedure Call)** refers to a way of invoking AWS Lambda functions **synchronously** using **direct communication** between a client and the function. It allows you to execute a Lambda function and receive the response immediately.

---

# How Lambda RPC Works

1. **Client Calls Lambda Directly**

   * The client (e.g., an API, SDK, or another AWS service) sends a request to AWS Lambda.

   * The request includes **input parameters**, function name, and invocation type.

2. **Lambda Executes the Function**

   * AWS Lambda runs the function and processes the request.

   * The function's execution environment handles the computation.

3. **Lambda Returns a Response**

   * If the function is invoked synchronously (`RequestResponse`), the client gets an **immediate response**.

   * If invoked asynchronously (`Event`), the function executes in the background without waiting for a response.

---

# Lambda Invocation Types

1. **Synchronous Invocation (RPC Style)**

   * The caller waits for the function’s response.

   * Used for real-time processing (e.g., APIs, interactive apps).

   * **Example Services:** API Gateway, AWS SDK (direct calls).

   * **Example API Call:**

```
aws lambda invoke --function-name MyFunction --payload '{"key": "value"}' response.json
```

2.   
   **Asynchronous Invocation**

   * The caller sends a request but doesn’t wait for a response.

   * Used for background tasks (e.g., event-driven processing).

   * **Example Services:** S3 Event Notifications, SNS, EventBridge.

   * **Example API Call:**

```
aws lambda invoke --function-name MyFunction --invocation-type Event --payload '{"key": "value"}' response.json
```

3.   
   **Poll-Based (Event Source Mapping)**

   * AWS services like SQS, DynamoDB Streams, or Kinesis automatically trigger Lambda.

   * Lambda **polls** for new data and processes events.

---

# Why Use Lambda RPC?

✔ **Low Latency** – Clients receive responses quickly.  
 ✔ **Simple & Direct** – No need for additional infrastructure.  
 ✔ **Scalable** – AWS Lambda automatically scales with demand.  
 ✔ **Cost-Efficient** – Pay only for the execution time.

---

# Example: Calling Lambda via SDK (Python)

```py
import boto3

lambda_client = boto3.client('lambda')

response = lambda_client.invoke(
    FunctionName='MyLambdaFunction',
  InvocationType='RequestResponse',  # Synchronous (RPC)
    Payload=b'{"key": "value"}'
)

print(response['Payload'].read().decode('utf-8'))
```

---

# When to Use Lambda RPC?

* **API Backends:** When using Lambda with API Gateway for real-time responses.

* **Microservices Communication:** When invoking Lambda directly from another service.

* **Data Processing Pipelines:** For real-time processing of data requests.

* **Chatbots & AI Apps:** When immediate function execution is needed.

Would you like an in-depth example with API Gateway or another AWS service? 🚀

