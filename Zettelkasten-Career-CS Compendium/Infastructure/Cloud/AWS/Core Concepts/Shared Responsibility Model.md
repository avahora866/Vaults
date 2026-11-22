---
tags:
  - AWS
---
The **Shared Responsibility Model** in AWS defines the division of security and compliance responsibilities between AWS and its customers. It ensures clarity about which aspects AWS manages and which are the customer’s responsibility, enabling secure and efficient cloud operations.

---

# AWS Responsibilities: "Security OF the Cloud"

AWS is responsible for protecting the infrastructure that runs all of the services offered in the AWS Cloud. This includes:

1. **Physical Security**:  
   * Securing data centers (e.g., power, cooling, physical access control).  
   * Regular audits and certifications to maintain compliance.  
2. **Hardware and Software**:  
   * Maintenance and security of compute, storage, and networking hardware.  
   * Underlying software components like hypervisors and networking layers.  
3. **Global Infrastructure**:  
   * Management of regions, availability zones, and edge locations.  
   * Redundancy, disaster recovery, and fault tolerance.  
4. **Managed Services**:  
   * Services like [[AWS S3|AWS S3]], [[AWS Lambda|AWS Lambda]], and [[AWS Relational Database Service (RDS)|RDS]] include built-in security features (e.g., encryption, data replication).  
   * AWS ensures these services are operational and secure.

---

# Customer Responsibilities: "Security IN the Cloud"

Customers are responsible for securing what they deploy and configure within AWS. This includes:

1. **Data Protection**:  
   * Encrypting sensitive data (e.g., using [[AWS Key Management Service (KMS)|AWS KMS]]).  
   * Implementing access control for data stored in [[AWS S3|S3]], [[AWS Relational Database Service (RDS)|RDS]], or other services.  
2. [[Identity and Access Management (IAM)|Identity and Access Management (IAM):]]  
   * Configuring users, groups, roles, and permissions.  
   * Enforcing best practices like MFA (Multi-Factor Authentication).  
3. **Application Security**:  
   * Securing applications running on AWS resources like EC2 instances or containers.  
   * Applying patches and updates to operating systems and applications.  
4. **Network Security**:  
   * Configuring firewalls (e.g., Security Groups, Network ACLs).  
   * Securing VPNs, load balancers, and custom [[VPC Security Groups|VPC]] configurations.  
5. **Compliance**:  
   * Ensuring applications and data meet compliance requirements (e.g., GDPR, HIPAA) based on how AWS services are configured and used.  
6. **Monitoring and Logging**:  
   * Setting up and monitoring logs with [[AWS CloudWatch|CloudWatch]], [[AWS CloudTrail|CloudTrail]], and other tools.  
   * Acting on security events and alerts.

---

# Examples of Shared Responsibilities

1. **Amazon EC2**:  
   * AWS manages the physical servers and hypervisors.  
   * Customers are responsible for the security of their instances (e.g., OS patching, antivirus).  
2. [[AWS S3|Amazon S3]]:  
   * AWS ensures the durability and availability of the S3 storage system.  
   * Customers configure access controls, bucket policies, and encryption settings.  
3. [[AWS Lambda|AWS Lambda]]:  
   * AWS secures the infrastructure running Lambda functions.  
   * Customers write secure code, manage sensitive data in environment variables, and configure [[Identity and Access Management (IAM)|IAM roles]].

---

# Benefits of the Shared Responsibility Model

* **Clarity**: Defines clear boundaries, reducing confusion about security and compliance.  
* **Scalability**: Allows AWS to handle global infrastructure, while customers focus on application-level security.  
* **Security by Design**: Encourages customers to adopt best practices for securing their wor

