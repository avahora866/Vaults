---
tags:
  - AWS
---
**AWS Key Management Service (KMS)** is a managed service that allows you to create and control cryptographic keys used to encrypt and decrypt your data. It integrates with many AWS services to provide a seamless way to protect sensitive data, ensuring security and compliance requirements are met without the complexity of managing your own key infrastructure.

---

# Core Features of AWS KMS:

1. **Key Management**:  
   * KMS provides a secure environment to create, manage, and store **customer master keys (CMKs)**. CMKs are the primary resources in KMS used to encrypt and decrypt data.  
   * You can control who has access to the keys and define what actions they can perform.  
2. **Encryption and Decryption**:  
   * KMS supports both **symmetric keys** (the same key is used for encryption and decryption) and **asymmetric keys** (a public key for encryption and a private key for decryption).  
   * It performs encryption and decryption operations within a secure environment.  
3. **AWS Service Integration**:  
   * KMS integrates with many AWS services, such as [[AWS S3|AWS S3]], [[AWS DynamoDB|DynamoDB]], [[AWS Relational Database Service (RDS)|RDS]], and [[AWS Lambda|AWS Lambda]], enabling you to encrypt data at rest seamlessly.  
4. **Data Encryption at Scale**:  
   * KMS provides the ability to encrypt large amounts of data efficiently using **envelope encryption**, where a data encryption key (DEK) is used to encrypt data, and the DEK itself is encrypted using a CMK.  
5. **Key Policies and** [[Identity and Access Management (IAM)|IAM Integration]]:  
   * Access to KMS keys is managed through key policies and IAM policies, allowing fine-grained control over who can use or manage the keys.  
6. **Auditing and Logging**:  
   * AWS KMS integrates with [[AWS CloudTrail|AWS CloudTrail]], providing a detailed record of all key management operations, including who accessed or managed a key and when.  
7. **FIPS 140-2 Compliance**:  
   * KMS supports cryptographic operations in compliance with the Federal Information Processing Standard (FIPS) 140-2, ensuring secure handling of keys.  
8. **Custom Key Stores**:  
   * With **AWS CloudHSM**, you can create a custom key store that uses hardware security modules (HSMs) to store your CMKs, giving you additional control over the physical location and lifecycle of your keys.

---

# Key Concepts in AWS KMS:

1. **Customer Master Keys (CMKs)**:  
   * CMKs are the primary entities in KMS used for cryptographic operations.  
   * There are two types of CMKs:  
     * **AWS Managed Keys**: Created and managed by AWS for specific services (e.g., [[AWS S3|S3]], [[AWS Relational Database Service (RDS)|RDS]]).  
     * **Customer Managed Keys**: Created and managed by you, giving you control over rotation, deletion, and policies.  
2. **Data Encryption Keys (DEKs)**:  
   * DEKs are used to encrypt large amounts of data efficiently. KMS generates, encrypts, and provides these keys, while you handle their encrypted versions.  
3. **Envelope Encryption**:  
   * A method where the data is encrypted using a DEK, and the DEK itself is encrypted with a CMK. This improves performance by offloading bulk data encryption to your application while keeping the keys secure.  
4. **Key Policies**:  
   * JSON documents that define who can use or manage a key and the specific actions they can perform.  
5. **Key Rotation**:  
   * AWS KMS can automatically rotate CMKs annually to enhance security. However, you must manually enable this feature for customer-managed CMKs.  
6. **Multi-Region Keys**:  
   * KMS supports multi-region keys, enabling you to replicate keys across AWS regions for disaster recovery or global applications.

---

# How AWS KMS Works:

1. **Creating Keys**:  
   * You create a CMK in KMS. This can be symmetric or asymmetric, depending on your needs.  
   * Define the [[Identity and Access Management (IAM)|key policy and IAM permissions]] to control access to the key.  
2. **Encrypting Data**:  
   * AWS services or your application sends plaintext data to KMS for encryption.  
   * KMS generates a DEK and encrypts the data using envelope encryption.  
3. **Decrypting Data**:  
   * To decrypt data, the encrypted DEK is sent to KMS, which decrypts it using the CMK.  
   * The plaintext DEK is then used to decrypt the original data.

---

# Example Use Case:

Suppose you're storing sensitive customer information in an [[AWS S3|S3 bucket]]. You can enable **server-side encryption** with KMS, and [[AWS S3|S3]] will encrypt all objects using a CMK. KMS handles key storage, access, and auditing:

* When you upload data, [[AWS S3|S3]] requests KMS to encrypt it using the CMK.  
* When you retrieve data, [[AWS S3|S3]] requests KMS to decrypt it before serving the request.

---

# Benefits of AWS KMS:

1. **Simplicity**:  
   * Fully managed service eliminates the need for complex key management infrastructure.  
2. **Integrated Security**:  
   * Seamlessly integrates with AWS services, ensuring encryption is applied consistently and easily.  
3. **Fine-Grained Access Control**:  
   * [[Identity and Access Management (IAM)|Key policies and IAM permissions]] give you precise control over who can use or manage keys.  
4. **Scalability**:  
   * Scales with your workloads to handle encryption needs, from small applications to enterprise-scale systems.  
5. **Auditability**:  
   * Integration with [[AWS CloudTrail]] ensures visibility into key usage and management activities.  
6. **Compliance**:  
   * Meets various regulatory and compliance requirements, including GDPR, HIPAA, and FIPS 140-2.

---

AWS KMS is an essential service for securing sensitive data in AWS, providing a robust and scalable solution for encryption and key management.

