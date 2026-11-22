---
tags:
  - AWS
---
**AWS Lambda** is a serverless compute service that lets you run code without provisioning or managing servers. You provide the code, and AWS Lambda handles everything needed to run and scale it, including server management, scaling, monitoring, and fault tolerance. It is designed for [[Event-Driven Architecture|event-driven architectures]], where functions are triggered by events from other AWS services, HTTP requests, or custom applications.

---

# Key Features of AWS Lambda

1. [[Event-Driven Architecture|Event-Driven Execution]]:  
	- AWS Lambda is triggered by events from various sources, such as:  
	     * [[AWS S3|S3]]: When a file is uploaded, modified, or deleted in a bucket.  
	     * [[AWS DynamoDB|DynamoDB]]: When an item is added, updated, or deleted in a table.  
	     * [[AWS API Gateway|API Gateway]]: When an HTTP request is made to a [[Rest|REST]] or HTTP API endpoint.  
	     * [[AWS Simple Queue Service (SQS)|SQS]]: When a new message is added to an SQS queue.  
	     * [[AWS Simple Notification Service (SNS)|SNS]]: When a notification is published to an SNS topic.  
	     * [[AWS CloudWatch|CloudWatch]] **Events** (`EventBridge`): On scheduled intervals or when an event rule is triggered.  
	     * [[AWS CloudWatch|CloudWatch]] **Logs**: When a log entry matches a subscription filter.  
	     * **Kinesis**: When new data is added to a Kinesis stream.  
	     * [[AWS Cognito|Cognito]]: During user pool events like user sign-up, pre-authentication, or post-confirmation.  
	     * **Step Functions**: When a state machine triggers a Lambda task.  
	     * **IoT Core**: When an MQTT message is published to an IoT topic.  
	     * **`CodeCommit`**: When a repository’s commit, branch, or tag changes.  
	     * **`CodePipeline`**: When an action within a pipeline is executed.  
	     * [[AWS CloudFormation|CloudFormation]]: During custom resource creation, update, or deletion.  
	     * **Config**: When a resource compliance or configuration rule evaluation changes.  
	     * **Alexa Skills Kit**: When a user interacts with an Alexa skill.  
	     * [[AWS S3|S3]] Object Lambda: When a request to access an S3 object triggers a transformation.  
	     * [[AWS EventBridge|EventBridge]] Schema Registry: For custom events matching a defined schema.  
	     * **ECS**: When ECS tasks or services send event notifications.  
	     * [[AWS Relational Database Service (RDS)|RDS]] **Proxy**: When a database proxy sends a request.  
	     * **Secrets Manager**: During secret rotation workflows.  
	     * [[AWS Elastic File System (EFS)|EFS (Elastic File System)]]: For file system access points with Lambda integration.  
	- Event-driven execution enables automation and real-time data processing.  
2. **Serverless**:  
	- No server provisioning or management required.  
	- Automatically scales based on the number of incoming requests or events.  
	- You pay only for the compute time used (measured in milliseconds).  
3. **Runtime Support**:  
	- Supports multiple programming languages, including Python, Node.js, Java, C\#, Go, and Ruby.  
	- Custom runtimes allow you to use other languages if needed.  
4. **Function Configuration**:  
	- **Memory Allocation**: You can configure the memory (128 MB to 10 GB) allocated to your function.  
	- **Timeout**: Maximum execution time for a function (up to 15 minutes).  
	- **Environment Variables**: Store sensitive configuration data securely.  
5. **Integration with AWS Services**:  
	- Works seamlessly with services like [[AWS S3|S3]], [[AWS DynamoDB|DynamoDB]], [[AWS API Gateway|API Gateway]], [[AWS Simple Notification Service (SNS)|SNS]], [[AWS Simple Queue Service (SQS)|SQS]], and more.  
	- Often used as glue to connect AWS services in serverless applications.  
6. **Built-in Fault Tolerance**:  
	- Lambda automatically retries failed executions and handles infrastructure failures.  
7. **Security**:  
	- Integrated with [[Identity and Access Management (IAM)|AWS Identity and Access Management (IAM)]] to control access to Lambda functions and resources.  
	- Supports encrypted environment variables and secure invocation.  
8. **Monitoring and Logging**:  
	- Integrated with [[AWS CloudWatch|Amazon CloudWatch]] for monitoring metrics (e.g., invocations, errors) and capturing logs.

---

# How AWS Lambda Works

1. **Upload Your Code**:  
	- Write your function code and upload it to AWS Lambda, either as a ZIP file or directly in the AWS Management Console.  
	- Use AWS SDKs or the [[AWS CLI and SDKs|AWS CLI]] for deployment automation.  
2. **Define Triggers**:  
	- Specify the event sources (e.g., [[AWS S3|S3 bucket]], [[AWS DynamoDB|DynamoDB table]]) that will invoke your Lambda function.  
3. **Configure Permissions**:  
	- Attach an [[Identity and Access Management (IAM)|IAM]] role to the function to grant access to other AWS resources.  
4. **Execution**:  
	- When an event occurs, AWS Lambda automatically:  
		- Retrieves the code.  
		- Provisions the necessary compute environment.  
		- Executes the function.  
		- Scales to handle the volume of incoming events.

---

# Use Cases for AWS Lambda

1. **Data Processing**:  
	- Process files uploaded to [[AWS S3|S3]] (e.g., resizing images, generating reports).  
	- Stream and analyse real-time data from Kinesis or [[AWS DynamoDB|DynamoDB]].  
2. **Backend for APIs**:  
	- Create [[Rest|RESTful]] or [[GraphQL]] APIs using Lambda with [[AWS API Gateway|API Gateway]].  
3. **Automation**:  
	- Automate resource management (e.g., start/stop EC2 instances based on schedules).  
	- Perform operational tasks like backups or log rotation.  
4. **IoT Applications**:  
	- Handle events from IoT devices with Lambda and AWS IoT Core.  
5. **Event-Driven Workflows**:  
	- Connect and orchestrate AWS services in workflows (e.g., using Step Functions).

---

# Benefits of AWS Lambda

- *Cost-Effectiveness**: Pay only for the execution time used, with no charges when functions are idle.  
* **Scalability**: Automatically scales based on event volume.  
* **Flexibility**: Supports multiple runtimes and integrates with most AWS services.  
* **Reduced Operational Overhead**: No need to manage servers or worry about scaling.

# Lambda is the key to serverless architecture

AWS Lambda is considered the "key to serverless computing" because it abstracts away server management, allowing developers to focus solely on writing code. It enables [[Event-Driven Architecture|event-driven execution]], automatically scaling in response to incoming events with no need for manual infrastructure management. With a pay-as-you-go model, Lambda is cost-efficient and integrates seamlessly with a wide range of AWS services. By supporting decoupled, modular applications and reducing operational overhead, Lambda simplifies development, maintenance, and deployment. It excels in modern use cases like real-time data processing, [[Microservices Architecture|microservices]], and API backends, making it the foundation of serverless architectures.
# Choosing a langugae

|Language|Cold Start|Execution Speed|Ease of Deployment|AWS SDK Support|Best Use Case|
|---|---|---|---|---|---|
|**Python**|Fast|Good|Very Easy|Best (`boto3`)|Data processing, ML, quick scripts|
|**Node.js**|Fast|Good|Very Easy|Strong|API Gateway, web apps, real-time apps|
|**Go**|Fast|Best|Moderate|Moderate|Performance-sensitive workloads|
|**Java**|Slow|Good (optimized)|Complex|Good|Enterprise apps, high concurrency|
|**.NET**|Slow|Good (optimized)|Complex|Good|Microsoft-based environments|
|**Ruby**|Moderate|Moderate|Easy|Moderate|General scripting|

## Cold Start

A **cold start** happens when AWS Lambda **initializes a new execution environment** to handle an incoming request. This process takes time, leading to increased response latency.

---

### Why Do Cold Starts Happen?

AWS Lambda is **serverless**, meaning it does not keep instances running all the time. When a function is invoked:

1. If an **existing warm instance** is available, AWS reuses it (**no cold start**).
2. If no warm instance is available, AWS **creates a new execution environment**, leading to a **cold start**.

Cold starts occur when:

- A function is **invoked for the first time** after deployment.
- A function has been **idle for a while** (usually several minutes).
- There’s a **scale-up event**, and AWS needs to create additional instances.

---

### Steps in a Cold Start

1. **Provisioning the Execution Environment**
    - AWS sets up a virtual machine (Firecracker microVM) with necessary resources.
    - The Lambda runtime (Node.js, Python, Java, etc.) is loaded.

2. **Initializing the Function Code**
    - If using Java, .NET, or other compiled languages, the runtime and dependencies (e.g., JVM, CLR) must be loaded.
    - AWS runs the function’s **initialization code** (e.g., static blocks, constructors).

3. **Executing the Function**
    - Once initialized, AWS runs the function’s **handler method** and processes the request.
---

### Cold Start vs. Warm Start

| Feature                 | Cold Start                           | Warm Start           |
| ----------------------- | ------------------------------------ | -------------------- |
| Environment Setup       | Needed                               | Already available    |
| Function Initialization | Runs from scratch                    | Skipped              |
| Execution Speed         | Slower (high latency)                | Faster (low latency) |
| When It Happens         | First invocation, idle time, scaling | Repeated invocations |

---

### Which Languages Have the Worst Cold Starts?

Some languages take longer to initialize because of runtime requirements.

|Language|Cold Start Impact|
|---|---|
|**Python**|🟢 Fast (low impact)|
|**Node.js**|🟢 Fast (low impact)|
|**Go**|🟢 Fast (low impact)|
|**Java**|🔴 Slow (high impact) - JVM startup required|
|.NET (C#)|🔴 Slow (high impact) - CLR startup required|
|**Ruby**|🟡 Moderate|

**Java and .NET have the slowest cold starts** because they require the **JVM or CLR** to initialize. Python, Node.js, and Go have minimal setup and are much faster.

---

### How to Reduce Cold Starts

1. **Use a Lightweight Language**
    - Prefer **Python, Node.js, or Go** over Java or .NET.

2. **Enable Provisioned Concurrency**
    - Keeps a fixed number of execution environments **always warm**.
    - Reduces cold start time but **incurs extra cost**.

3. **Keep Functions Small and Optimized**
    - Reduce dependencies and external libraries.
    - Minimize initialization logic (e.g., avoid unnecessary database connections in the global scope).

4. **Use AWS Lambda SnapStart (for Java)**
    - Available for **Java 11+**, it reduces cold start time by caching the initialized execution environment.

5. Keep Functions Warm with Scheduled Invocations
    - Use **Amazon CloudWatch** or **AWS Lambda Warmer** to periodically invoke the function and keep instances warm.
# Structure
An AWS Lambda function's code file typically follows a specific structure depending on the runtime used (Python, Node.js, Java, etc.). Here's a general breakdown of the structure:

## Handler Function

- This is the entry point for AWS Lambda to execute your function.
- The function must match the handler name specified in the AWS Lambda configuration.
- The handler function receives:
    - `event`: The input payload (e.g., API Gateway request, S3 event, DynamoDB stream).
    - `context`: Information about the execution environment (e.g., request ID, remaining execution time).

## Dependencies

- If your function relies on external libraries, they should be included in the package.
- For Python, this is typically a `requirements.txt` file.
- For Node.js, dependencies go in `package.json`.

## Configuration Files (Optional)

- Environment variables can be used instead of hardcoding values.
- Sometimes, configuration files like `.env` or `config.json` are used.

## Deployment Package

- If deploying manually, you zip the source code along with dependencies.
- AWS Lambda also supports container images.

---
## Examples
### Example: Python (Runtime: Python 3.x)

```python
import json

def lambda_handler(event, context):
    # Log event for debugging
    print("Received event:", json.dumps(event, indent=2))
    
    # Business logic
    response = {
        "statusCode": 200,
        "body": json.dumps({"message": "Hello from Lambda!"})
    }
    
    return response
```

---

### Example: Node.js (Runtime: Node.js 18.x)

```javascript
exports.handler = async (event, context) => {
    console.log("Received event:", JSON.stringify(event, null, 2));
    
    return {
        statusCode: 200,
        body: JSON.stringify({ message: "Hello from Lambda!" }),
    };
};
```

---

### Example: Java (Runtime: Java 11)

```java
import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
import java.util.Map;

public class LambdaFunctionHandler implements RequestHandler<Map<String, String>, String> {
    @Override
    public String handleRequest(Map<String, String> event, Context context) {
        context.getLogger().log("Received event: " + event.toString());
        return "Hello from Lambda!";
    }
}
```

---

## Deployment Methods

- **AWS Console:** Manually upload the code file.
- **AWS CLI:** Use `aws lambda update-function-code`.
- **AWS SAM/Serverless Framework:** Automate deployments.