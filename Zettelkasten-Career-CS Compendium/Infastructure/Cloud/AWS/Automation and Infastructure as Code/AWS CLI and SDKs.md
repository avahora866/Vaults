---
tags:
  - AWS
---
# AWS Command Line Interface (CLI)

The **AWS CLI** is a tool that allows you to manage AWS resources and services programmatically via the command line. It provides a unified interface to interact with AWS services by running commands in your terminal or shell.

---

## Key Features of AWS CLI

1. **Comprehensive Service Support**:  
   * Supports almost all AWS services, allowing you to create, update, delete, and query resources.  
2. **Automation**:  
   * Enables scripting and automation of repetitive tasks, such as deploying infrastructure or managing resources.  
3. **Cross-Platform**:  
   * Available for Windows, macOS, and Linux.  
4. **Configuration Profiles**:  
   * Use multiple profiles for managing credentials and default settings for different environments (e.g., dev, staging, production).  
5. **JSON and Table Output**:  
   * Provides output in multiple formats, such as JSON, text, and table, for easy parsing and readability.  
6. **Batch Operations**:  
   * Execute commands for multiple resources simultaneously, such as deleting multiple [[AWS S3|S3 objects]] in one go.

---

## Common AWS CLI Use Cases

1. **Resource Management**:  
   * Launch EC2 instances, manage [[AWS S3|S3 buckets]], and configure [[AWS Lambda|Lambda functions]].  
2. **Infrastructure Automation**:  
   * Automate deployments using shell scripts integrated with AWS CLI commands.  
3. **Querying Data**:  
   * Retrieve logs from [[AWS CloudWatch|CloudWatch]], list running EC2 instances, or fetch billing data.  
4. **Diagnostics and Debugging**:  
   * Check resource statuses, log metrics, or test network connectivity using AWS services.

---

## Example CLI Commands

[[AWS S3|List S3 Buckets]]:    
```bash
aws s3 ls
```

**Launch an EC2 Instance**:  
```bash
aws ec2 run-instances --image-id ami-12345678 --count 1 --instance-type t2.micro
```

**Invoke a** [[AWS Lambda|Lambda Function]]:  
```bash
aws lambda invoke --function-name my-function output.txt
```


---

# AWS Software Development Kits (SDKs)

The **AWS SDKs** are language-specific libraries that allow developers to interact with AWS services programmatically. They abstract the complexity of direct API calls and enable seamless integration of AWS services into applications.

---

## Key Features of AWS SDKs

1. **Language Support**:  
   * SDKs are available for many popular programming languages, including Python (Boto3), Java, JavaScript (Node.js), .NET, Ruby, Go, and more.  
2. **Simplified API Interactions**:  
   * Provides high-level abstractions for AWS services, reducing the need to write HTTP requests manually.  
3. **Authentication Management**:  
   * Automatically handles signing requests using AWS credentials and provides support for temporary credentials (e.g., via AWS Identity and Access Management roles).  
4. [[Event-Driven Architecture|Event-Driven Programming]]:  
   * Supports event-driven models for serverless architectures, such as invoking [[AWS Lambda|Lambda functions]] or handling [[AWS Simple Queue Service (SQS)|SQS events]].  
5. **Integration with AWS Services**:  
   * SDKs include prebuilt methods for all AWS services, such as creating [[AWS S3|S3 buckets]], publishing messages to [[AWS Simple Notification Service (SNS)|SNS]], or querying [[AWS DynamoDB|DynamoDB]].  
6. **Open-Source**:  
   * SDKs are open-source, allowing developers to inspect and contribute to the libraries.

---

## Common AWS SDK Use Cases

1. **Build Applications**:  
   * Create applications that interact with AWS services, such as web apps, mobile apps, and backend systems.  
2. **Serverless Integration**:  
   * Use SDKs in [[AWS Lambda|Lambda functions]] to access resources like [[AWS DynamoDB|DynamoDB]] or [[AWS S3|S3]].  
3. **Data Processing**:  
   * Automate data ingestion and analysis workflows using SDKs with services like [[AWS S3]] and SageMaker.  
4. **Custom Monitoring**:  
   * Use [[AWS CloudWatch|CloudWatch]] APIs via SDKs to create custom metrics and alarms.

---

## Example AWS SDK Usage (Python \- Boto3)

Upload a File to [[AWS S3|S3]]:  
```python
import boto3

s3 = boto3.client('s3')  
s3.upload_file('file.txt', 'my-bucket', 'file.txt')
```


Query [[AWS DynamoDB|DynamoDB]]:    
```python
import boto3

dynamodb = boto3.resource('dynamodb')  
table = dynamodb.Table('MyTable')

response = table.get_item(Key={'PrimaryKey': 'Value'})  
print(response['Item'])
```


---

# AWS CLI vs. SDKs

| Feature | AWS CLI | AWS SDKs |
| ----- | ----- | ----- |
| **Usage** | Command-line interface | Embedded in application code |
| **Automation** | Shell scripting | Programmatic control |
| **Language** | Independent of programming language | Language-specific (e.g., Python, Java) |
| **Complexity** | Simple for one-off tasks | Advanced logic for complex systems |
| **Integration** | Limited to CLI scripts | Deep integration into applications |
| **Learning Curve** | Low | Moderate to high, depending on language |

---

# When to Use AWS CLI vs. SDKs

* Use **AWS CLI** for:  
  * Quick, one-off operations.  
  * Automating workflows through shell scripts.  
  * Manual infrastructure management.  
* Use **AWS SDKs** for:  
  * Building applications that interact with AWS services.  
  * Automating complex tasks or workflows with detailed programmatic logic.  
  * Embedding AWS functionality into web, mobile, or server applications.

---

The AWS CLI and SDKs complement each other, with the CLI suited for quick, command-based management and the SDKs providing deep integration for building applications. Both are essential tools for leveraging AWS efficiently. 

