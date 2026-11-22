---
tags:
  - AWS
---
**AWS Identity and Access Management (IAM)** is a service provided by AWS that allows you to securely manage access to AWS resources. With IAM, you can control who can access your AWS resources (identity) and what actions they can perform (access permissions). It is a foundational service that ensures the right people and systems have the right access to your AWS infrastructure.

---

# Key Features of IAM:

1. **Centralized Access Control**:  
   * Manage permissions for multiple AWS services from a single interface.  
   * Define granular access policies to control what resources can be accessed and what actions can be performed.  
2. **Users, Groups, and Roles**:  
   * **Users**: Represent individual identities, such as employees or applications. Each user has a unique name and credentials (e.g., passwords, access keys).  
   * **Groups**: Collections of users with shared permissions. Assigning permissions to groups makes it easier to manage users with similar access requirements.  
   * **Roles**: Temporary credentials assigned to entities (like applications, AWS services, or external users) to perform specific tasks.  
3. **Policies**:  
   * Policies are JSON documents that define permissions. They specify:  
     * **Actions**: What actions are allowed or denied (e.g., `s3:PutObject`).  
     * **Resources**: The specific AWS resources the actions apply to (e.g., an [[AWS S3|S3 bucket]]).  
     * **Conditions**: Optional conditions that control when and how the permissions apply.  
   * Policies can be attached to users, groups, or roles.  
4. **Multi-Factor Authentication (MFA)**:  
   * Enhances security by requiring a second form of authentication (e.g., a one-time code from a mobile app) in addition to a password.  
5. **IAM Permissions Boundaries**:  
   * Define the maximum permissions a user or role can have. This helps enforce security boundaries, even if a user or role is granted broader permissions elsewhere.  
6. **Service Integration**:  
   * IAM integrates with all AWS services, enabling fine-grained access control for services like [[AWS S3|AWS S3]], EC2, [[AWS Lambda|AWS Lambda]], [[AWS DynamoDB|DynamoDB]], and more.  
7. **Federated Access**:  
   * Allows you to grant access to AWS resources using existing corporate credentials (e.g., via SAML 2.0 or OpenID Connect).  
8. **IAM Access Analyzer**:  
   * Automatically analyses permissions for your resources and identifies those that are accessible from outside your AWS environment.

---

# Core Concepts in IAM:

1. **Authentication**:  
   * Verifies the identity of a user, system, or application trying to access AWS. IAM supports different authentication methods, including passwords, access keys, and federation.  
2. **Authorization**:  
   * Determines what actions an authenticated identity can perform. Permissions are granted using policies attached to identities (users, groups, roles) or resources.  
3. **Least Privilege**:  
   * IAM enforces the principle of least privilege, meaning users and roles should only have the permissions necessary to perform their tasks—nothing more.  
4. **Identity Federation**:  
   * Enables users to access AWS resources without creating IAM users. Instead, they can authenticate using external identity providers like Microsoft Active Directory or Google Workspace.

---

# Example Use Case:

Let’s say you’re managing an e-commerce application. You can use IAM as follows:

1. **Admins**:  
   * Create an IAM group for administrators with full access to AWS resources. Attach the `AdministratorAccess` policy to the group.  
   * Add individual IAM users (e.g., DevOps engineers) to this group.  
2. **Developers**:  
   * Create an IAM group for developers with restricted permissions, such as access to development environments but no production access.  
3. **Applications**:  
   * Use IAM roles for applications running on EC2 or [[AWS Lambda|AWS Lambda]] to grant them the necessary permissions (e.g., accessing an [[AWS S3|S3 bucket]] or a [[AWS DynamoDB|DynamoDB]] table).  
4. **MFA**:  
   * Enforce MFA for all users, especially those with elevated permissions.

---

# Best Practices for IAM:

1. **Enable MFA**:  
   * Require multi-factor authentication for all users to enhance account security.  
2. **Grant Least Privilege**:  
   * Only grant the permissions necessary for a task. Regularly review and update policies to avoid over-provisioning.  
3. **Use Roles for Applications**:  
   * Use IAM roles instead of embedding access keys in applications. This ensures secure, temporary access.  
4. **Audit and Monitor Permissions**:  
   * Use AWS IAM Access Analyzer and [[AWS CloudTrail|AWS CloudTrail]] to monitor and audit access to AWS resources.  
5. **Rotate Credentials**:  
   * Regularly rotate access keys and passwords for IAM users to minimize the risk of compromise.  
6. **Use Service Control Policies (SCPs)**:  
   * If using AWS Organizations, implement SCPs to enforce permissions at the organizational level.

---

# Common IAM Policies:

* **AdministratorAccess**: Grants full access to all AWS resources.  
* **AmazonS3ReadOnlyAccess**: Provides read-only access to all [[AWS S3|S3 buckets]].  
* **AWSLambdaExecute**: Grants permissions to invoke and manage [[AWS Lambda|AWS Lambda functions]].  
* **AmazonEC2FullAccess**: Provides full access to Amazon EC2 resources.

---

In summary, **AWS IAM** is essential for securing your AWS environment, ensuring proper identity management, and granting appropriate access to users and applications. It provides powerful tools to enforce security and compliance in your cloud infrastructure.

