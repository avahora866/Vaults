---
tags:
  - FORMAT
  - AWS
---


# What is AWS SAM?

AWS Serverless Application Model (**AWS SAM**) is an open-source framework for building, testing, and deploying serverless applications on **AWS**. It simplifies defining and deploying serverless resources like AWS Lambda, API Gateway, DynamoDB, and more.

---

# How AWS SAM Works

1. **Define Resources**

   * Uses a YAML-based template (`template.yaml`) to define AWS resources.

   * Extends AWS CloudFormation with simplified syntax.

2. **Build & Test Locally**

   * AWS SAM CLI allows local testing of Lambda functions, API Gateway endpoints, and event-driven applications.

3. **Deploy to AWS**

   * Packages and deploys the application using `sam deploy`, translating the template into CloudFormation stacks.

---

# AWS SAM Components

## AWS SAM Template (`template.yaml`)

Defines resources using **SAM-specific syntax** that simplifies CloudFormation definitions.

Example:

```
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: app.lambda_handler
      Runtime: python3.8
      CodeUri: .
      Events:
        ApiEvent:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

## AWS SAM CLI

A command-line tool that helps develop, build, test, and deploy serverless applications.

Common commands:

* `sam init` – Creates a new AWS SAM project.

* `sam build` – Builds the application.

* `sam local invoke` – Runs the Lambda function locally.

* `sam deploy` – Deploys the application to AWS.

## AWS SAM Transform

* The `Transform: AWS::Serverless-2016-10-31` directive in the template allows AWS SAM to **expand shorthand** definitions into full CloudFormation configurations.

---

# Benefits of AWS SAM

✔ **Simplified Serverless Development** – Reduces boilerplate code in CloudFormation.  
 ✔ **Local Testing** – Run and debug serverless apps before deploying.  
 ✔ **Fast Deployment** – Automates deployment with `sam deploy`.  
 ✔ **Integration with AWS Services** – Easily connects to Lambda, API Gateway, DynamoDB, and more.  
 ✔ **Supports CI/CD** – Works with AWS CodePipeline, GitHub Actions, and Jenkins.

---

# AWS SAM vs. AWS CloudFormation

| Feature | AWS SAM | AWS CloudFormation |
| ----- | ----- | ----- |
| **Resource Definition** | Uses simpler syntax | Requires full CloudFormation |
| **Local Testing** | Yes (SAM CLI) | No |
| **Deployment Speed** | Faster with `sam deploy` | Slower, more manual |
| **Ideal Use Case** | Serverless applications | Any AWS infrastructure |

---

# When to Use AWS SAM?

* When building **serverless applications** using AWS Lambda.

* If you need **local testing** before deployment.

* When you want **faster deployments** compared to CloudFormation.

* If you're using **event-driven architectures** like S3, DynamoDB Streams, or SNS.

---

Would you like an example AWS SAM project or help setting one up? 🚀

