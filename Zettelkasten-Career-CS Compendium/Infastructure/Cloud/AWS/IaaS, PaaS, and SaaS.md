---
tags:
  - AWS
---


AWS offers a broad range of services across different cloud computing models: Infrastructure as a Service (IaaS), Platform as a Service (PaaS), and Software as a Service (SaaS). Each model provides different levels of abstraction, management, and control.

Here’s a breakdown of AWS services by their cloud computing model:

# Infrastructure as a Service (IaaS)

IaaS provides the foundational computing resources such as virtual machines, networking, storage, and more. It gives users more control over the environment and is typically used for custom applications where you need to manage everything, from the operating system up to the application layer.

 Amazon EC2 (Elastic Compute Cloud): Virtual servers where you can run applications.  
 [[AWS S3|Amazon S3 (Simple Storage Service)]]: Scalable object storage for storing files and backups.  
 Amazon EBS (Elastic Block Store): Block-level storage that can be attached to EC2 instances.  
 [[VPC Security Groups|Amazon VPC (Virtual Private Cloud)]]: Network isolation for deploying applications securely within AWS.

# Platform as a Service (PaaS)

PaaS abstracts much of the underlying infrastructure management, providing a platform that enables developers to build, deploy, and manage applications without worrying about the hardware or network layer.

 [[AWS Elastic Beanstalk|AWS Elastic Beanstalk]]: A fully managed service for deploying applications in multiple languages, handling scaling, load balancing, and infrastructure management.  
 [[AWS Lambda|AWS Lambda]]: A serverless compute service that allows you to run code without provisioning or managing servers.  
 [[AWS Fargate|AWS Fargate]]: A serverless compute engine for containers, enabling you to run containers without managing the underlying EC2 instances.  
 [[AWS Relational Database Service (RDS)|Amazon RDS (Relational Database Service)]]: Managed relational databases, where AWS takes care of provisioning, patching, and backups.  
 [[AWS Aurora Serverless|Amazon Aurora]]: A high-performance, managed relational database engine compatible with MySQL and PostgreSQL.  
 [[AWS Simple Queue Service (SQS)|Amazon SQS (Simple Queue Service)]]: Managed message queuing service to decouple and scale [[Microservices Architecture|microservices]] and distributed systems.

# Software as a Service (SaaS)

SaaS refers to fully managed applications that are provided to customers over the internet. With SaaS, users don’t need to worry about the underlying infrastructure or platform; they simply access the software through a web interface.

 Amazon Chime: A communication service for video conferencing, messaging, and collaboration.  
 Amazon WorkDocs: A managed document storage and collaboration service.  
 Amazon WorkMail: A managed email service for businesses.  
 AWS Managed Microsoft AD: A fully managed directory service that enables seamless integration with Microsoft Active Directory.

---

# Key Differences Between IaaS, PaaS, and SaaS

 IaaS (Infrastructure as a Service): Provides basic infrastructure services such as virtual machines and storage. The user has full control over the operating system and application layers. Examples: EC2, [[AWS S3|S3]], EBS.  
 PaaS (Platform as a Service): Provides a platform for building, running, and managing applications without worrying about the underlying infrastructure. The user focuses mainly on coding and deploying apps. Examples: [[AWS Elastic Beanstalk|Elastic Beanstalk]], [[AWS Lambda|AWS Lambda]], RDS.  
 SaaS (Software as a Service): Provides fully managed software applications accessible via the internet. The user just uses the application without worrying about how it’s built or maintained. Examples: Amazon Chime, WorkDocs, WorkMail.

---

