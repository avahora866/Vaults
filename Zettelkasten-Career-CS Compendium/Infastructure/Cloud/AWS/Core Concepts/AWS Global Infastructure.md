---
tags:
  - AWS
---
The AWS Global Infrastructure is the backbone of Amazon Web Services (AWS) and is designed to deliver high availability, low latency, fault tolerance, and global reach for its cloud services. Here’s a breakdown of its components:

---

# Regions

* Definition: A Region is a geographical area where AWS has multiple physical data centers.  
* Key Features:  
  * Each region is isolated and independent.  
  * Designed for compliance and data sovereignty; you can choose a region to meet specific regulatory or proximity requirements.  
  * Examples: `us-east-1 (N. Virginia)`, `eu-west-1 (Ireland)`, `ap-south-1 (Mumbai)`.

---

# Availability Zones (AZs)

* Definition: An Availability Zone is a logical grouping of one or more discrete data centres within a region.  
* Key Features:  
  * Each AZ has independent power, cooling, and networking.  
  * They are interconnected with high-speed, low-latency fibre networks.  
  * Allows you to build resilient applications by spreading workloads across multiple AZs.  
  * Example: The `us-east-1` region has six AZs: `us-east-1a`, `us-east-1b`, etc.

---

# Edge Locations

* Definition: Edge locations are endpoints for AWS services used to cache data and reduce latency.  
* Key Features:  
  * Primarily used by [[AWS Cloudfront|Amazon CloudFront, AWS's Content Delivery Network (CDN)]].  
  * Located in cities worldwide, closer to end users to improve performance.  
  * Example: A user in Sydney accessing data from an edge location will experience lower latency compared to accessing a distant AWS region.

---

# Local Zones

* Definition: Local Zones are extensions of AWS Regions, providing compute, storage, and other services closer to large population centre's.  
* Key Features:  
  * Useful for low-latency applications such as gaming or live streaming.  
  * Example: Local Zones in Los Angeles provide reduced latency for applications used in Southern California.

---

# Wavelength Zones

* Definition: Wavelength Zones are infrastructure extensions specifically designed for 5G mobile networks.  
* Key Features:  
  * Enables ultra-low latency for applications requiring edge computing near 5G devices.  
  * Integrated with telecom providers like Verizon and Vodafone.

---

# Global Network

* AWS regions, AZs, and edge locations are interconnected using a high-speed, private fiber network.  
* Ensures secure and reliable data transfer between regions and AZs.  
* Reduces latency and provides consistent performance.

---

# Benefits of the AWS Global Infrastructure

1. High Availability: Redundant systems across AZs and Regions ensure minimal downtime.  
2. Low Latency: Services like [[AWS Cloudfront|CloudFront]] bring content closer to end users.  
3. Global Reach: Applications can be deployed worldwide, reducing latency for global audiences.  
4. Fault Tolerance: Resources can failover between AZs or even regions if needed.

