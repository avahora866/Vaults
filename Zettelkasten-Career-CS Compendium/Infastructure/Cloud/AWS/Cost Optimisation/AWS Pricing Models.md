---
tags:
  - AWS
---
AWS offers flexible pricing models to suit various use cases and workloads. Understanding these models helps optimize costs and ensures you're only paying for what you use. Here are the primary pricing models:

---

# Pay-as-You-Go

* **Description**: You pay only for the resources you use, without upfront costs or long-term commitments.  
* **Best for**:  
  * Variable or unpredictable workloads.  
  * Applications with spiky traffic patterns.  
* **Example**:  
  * For **EC2**, you're charged by the second or hour for the time your instance is running.  
  * For [[AWS S3|S3]], you're charged based on the storage space used and the number of requests.

---

# Savings Plans

* **Description**: Commit to a consistent amount of usage (e.g., $10/hour) for a 1- or 3-year term to receive discounted rates.  
* **Best for**:  
  * Predictable workloads with long-term requirements.  
* **Benefits**:  
  * Flexibility to switch between compute services (e.g., EC2, [[AWS Lambda|AWS Lambda]], [[AWS Fargate|Fargate]]).  
  * Discounts can reach up to 72% compared to On-Demand prices.  
* **Example**:  
  * A company running steady EC2 instances could save costs by committing to a specific usage level.

---

# Reserved Instances (RIs)

* **Description**: Commit to a specific EC2 instance type in a specific region for 1 or 3 years in exchange for significant discounts.  
* **Types**:  
  * **Standard RIs**:  
    * Up to 75% savings.  
    * Requires full commitment to instance type and region.  
  * **Convertible RIs**:  
    * Up to 54% savings.  
    * Allows changes to instance family, size, or OS.  
  * **Scheduled RIs**:  
    * Available for predictable workloads that only run during specific times.  
* **Best for**:  
  * Applications with steady-state usage.  
* **Example**:  
  * A company running a web server 24/7 could save significantly by purchasing an RI.

---

# Spot Instances

* **Description**: Purchase unused EC2 capacity at discounts of up to 90% compared to On-Demand pricing.  
* **Best for**:  
  * Fault-tolerant, flexible, or stateless workloads.  
  * Batch jobs, data analysis, or CI/CD pipelines.  
* **Limitations**:  
  * Instances can be interrupted by AWS with a 2-minute warning if capacity is needed elsewhere.  
* **Example**:  
  * Running large-scale machine learning training jobs using Spot Instances for cost efficiency.

---

# Free Tier

* **Description**: AWS provides a free tier to let users try services with limited resources for free.  
* **Types**:  
  * **Always Free**:  
    * Services like [[AWS Lambda|AWS Lambda]] (1M requests/month) and [[AWS DynamoDB|DynamoDB]] (25 GB of storage).  
  * **12-Month Free**:  
    * Services like EC2 (750 hours/month for t2.micro) and [[AWS S3|S3]] (5 GB storage).  
  * **Trials**:  
    * Temporary free access to certain services, like [[AWS Relational Database Service (RDS)|RDS]] or SageMaker.  
* **Best for**:  
  * New users exploring AWS.  
* **Example**:  
  * Hosting a small website using the free tier EC2 instance.

---

# Dedicated Hosts

* **Description**: Rent physical EC2 servers dedicated to your use.  
* **Best for**:  
  * Workloads requiring compliance with regulatory requirements.  
  * Licensing requirements for software like Windows or Oracle.  
* **Example**:  
  * Running workloads subject to strict data residency or isolation policies.

---

# Enterprise Discount Program (EDP)

* **Description**: Large organizations negotiate custom pricing based on committed annual spending.  
* **Best for**:  
  * High-volume customers with significant AWS usage.  
* **Example**:  
  * A multinational enterprise committing to $10 million of AWS usage annually may negotiate discounts across multiple services.

---

# On-Demand Capacity Reservations

* **Description**: Reserve EC2 capacity without long-term commitments for specific availability zones.  
* **Best for**:  
  * Applications with strict availability or compliance requirements.  
* **Example**:  
  * Reserving capacity in advance for a big event, such as a product launch.

---

# Comparing Pricing Models

| Model | Best For | Key Benefit | Example Use Case |
| ----- | ----- | ----- | ----- |
| **Pay-as-You-Go** | Dynamic or spiky workloads | Flexibility | Short-term development or testing |
| **Savings Plans** | Predictable compute usage | Discounts (up to 72%) | Steady production workloads |
| **Reserved Instances** | Consistent, long-term workloads | Significant savings | Always-on applications like web servers |
| **Spot Instances** | Flexible or fault-tolerant workloads | Cost-efficiency | Batch processing or big data analytics |
| **Free Tier** | New users, small-scale workloads | Free experimentation | Hosting small websites or testing AWS |
| **Dedicated Hosts** | Compliance or licensing needs | Physical isolation | Regulatory-compliant enterprise workloads |

---

# Key Takeaways

* AWS pricing is highly flexible, catering to different workloads and business needs.  
* Combining models (e.g., using On-Demand for unpredictable traffic, RIs for steady-state apps, and Spot for batch jobs) can optimize costs.  
* Always monitor usage with tools like [[AWS Cost Explorer and Budgets|AWS Cost Explorer, Budgets]], and **Billing Dashboard** to stay within budget.

