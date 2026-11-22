---
tags:
  - AWS
---
Amazon **S3 Lifecycle Policies** are a set of rules that automatically manage the lifecycle of objects stored in [[AWS S3|S3 buckets]]. These policies allow you to transition objects between different storage classes or delete them after a certain period, helping to optimize storage costs and automate data retention.

---

# Key Features of S3 Lifecycle Policies

1. **Object Transition**:  
   * Transition objects between different storage classes (e.g., from Standard to Standard-IA or Glacier) based on their age or other criteria.  
   * Useful for moving infrequently accessed data to lower-cost storage classes.  
2. **Expiration**:  
   * Automatically delete objects after a specified period or based on certain conditions (e.g., delete files older than 365 days).  
   * Useful for managing temporary files or logs that no longer need to be retained.  
3. **Versioning Support**:  
   * If you have **versioning** enabled, you can create policies that apply to specific versions of objects, including deleting old versions.  
4. **Multiple Actions**:  
   * A single lifecycle rule can perform multiple actions, such as transitioning objects to a cheaper storage class and deleting them after a set time.  
5. **Custom Conditions**:  
   * Set conditions based on object age, creation date, or other object metadata (e.g., size or tags).

---

# Storage Classes for Transition

Lifecycle policies can transition objects between the following storage classes:

1. **S3 Standard**:  
   * Default storage class, optimized for frequently accessed data.  
2. **S3 Standard-IA (Infrequent Access)**:  
   * For data that is less frequently accessed but needs to be quickly retrieved when needed.  
   * Cheaper than Standard but incurs retrieval fees.  
3. **S3 One Zone-IA**:  
   * Similar to Standard-IA but stored in a single availability zone.  
   * Cost-effective for infrequently accessed data that can be recreated if lost.  
4. **S3 Glacier**:  
   * Low-cost storage for archival data that is rarely accessed and may take hours to retrieve.  
5. **S3 Glacier Deep Archive**:  
   * Cheapest storage class for long-term archival data with retrieval times ranging from hours to days.  
6. **S3 Intelligent-Tiering**:  
   * Automatically moves objects between two access tiers (frequent and infrequent access) based on changing access patterns.

---

# Example Lifecycle Policy Rules

1. **Transition to Infrequent Access**:  
   * Transition objects from **S3 Standard** to **S3 Standard-IA** after 30 days.  
   * Transition objects from **S3 Standard-IA** to **S3 Glacier** after 180 days.  
2. **Expiration of Objects**:  
   * Delete objects after 365 days.  
   * Expire versions of objects older than 90 days.  
3. **Multi-step Rule**:  
   * Transition an object from **S3 Standard** to **S3 Glacier** after 90 days.  
   * Delete the object from **S3 Glacier** after 365 days.

---

# How to Create a Lifecycle Policy

1. **Go to the S3 Console**:  
   * Navigate to the S3 bucket where you want to create the lifecycle rule.  
2. **Create a New Lifecycle Rule**:  
   * Choose **Management** → **Lifecycle rules** → **Create lifecycle rule**.  
   * Give the rule a name and define which objects the rule will apply to (e.g., all objects, objects with specific tags, etc.).  
3. **Define Transition Actions**:  
   * Set the conditions for when objects should be transitioned to a different storage class (e.g., after a specific number of days).  
4. **Define Expiration Actions**:  
   * Set expiration criteria, such as deleting objects after a certain period or removing older versions of objects.  
5. **Review and Save**:  
   * Review the lifecycle rule and click **Save** to apply it to your S3 bucket.

---

# Use Cases for S3 Lifecycle Policies

* **Cost Optimization**:  
  * Automatically move older, less frequently accessed data to lower-cost storage classes to reduce overall costs.  
* **Data Retention**:  
  * Comply with organizational or legal data retention policies by automatically deleting data that’s no longer needed.  
* **Backup and Archival**:  
  * Archive data that needs to be stored for long periods but isn’t regularly accessed, reducing costs while maintaining availability.  
* **Managing Log Files**:  
  * Automatically delete old logs after a certain period to prevent storing unnecessary data.

---

# Example Lifecycle Policy Setup

Let's say you want to set up a policy that transitions your data from S3 Standard to Glacier after 60 days and deletes the data after 5 years.

1. Create a lifecycle rule for all objects.  
2. Set the transition action:  
   * After 60 days, transition objects to **S3 Glacier**.  
3. Set the expiration action:  
   * After 5 years, delete objects.

This ensures that your data is archived in Glacier to save costs, while older data is automatically deleted after it's no longer needed.

---

# Benefits of Using S3 Lifecycle Policies

* **Cost Efficiency**: Move infrequently accessed data to cheaper storage classes automatically.  
* **Automation**: No need for manual intervention in data management.  
* **Compliance**: Automate data deletion for regulatory or business requirements.  
* **Scalability**: Efficiently manage large amounts of data without manual oversight.

