---
tags:
  - AWS
  - FORMAT
  - Concurrency
---
VPC Security Groups in AWS are virtual firewalls that control inbound and outbound traffic to and from resources within an Amazon Virtual Private Cloud (VPC). They operate at the instance level and are associated with Elastic Compute Cloud (EC2) instances, load balancers, [[AWS Relational Database Service (RDS)|RDS]] databases, or any other resource within a VPC.

---

# Key Features of Security Groups

1. Instance-Level Control:  
    Security Groups are applied to specific instances or resources, providing fine-grained control over traffic.  
2. Stateful Rules:  
    Security Groups are stateful, meaning that if an inbound request is allowed, the corresponding outbound response is automatically allowed, and vice versa. You don’t need to create separate rules for the return traffic.  
3. Inbound and Outbound Rules:  
    You can define rules to allow specific types of traffic in and out of your resource. By default:  
      Inbound traffic is denied unless explicitly allowed.  
      Outbound traffic is allowed unless explicitly denied.  
4. Protocol and Port-Based Filtering:  
    Rules can specify protocols (e.g., TCP, UDP, ICMP), port ranges, and source/destination IP addresses or CIDR blocks.  
5. Dynamic Membership:  
    Resources can belong to multiple Security Groups, and changes to Security Group rules are applied immediately to all associated resources.  
6. No Deny Rules:  
    Security Groups only allow traffic. You cannot explicitly deny traffic; instead, traffic not explicitly allowed is automatically denied.

---

# How Security Groups Work

1. Define Rules:  
    Each Security Group consists of a set of rules that specify:  
      The type of traffic (e.g., HTTP, SSH).  
      The protocol (e.g., TCP, UDP, ICMP).  
      The port range.  
      The source or destination (e.g., an IP address, another Security Group).  
2. Associate with Resources:  
    Security Groups are associated with instances or other resources in the VPC. Each instance can be associated with multiple Security Groups.  
3. Traffic Filtering:  
    When a packet reaches a resource:  
      Inbound Rules are evaluated for incoming traffic.  
      Outbound Rules are evaluated for outgoing traffic.  
    Traffic not matching any rule is automatically dropped.

---

# Common Use Cases

4. Web Server Access:  
    Allow inbound HTTP/HTTPS traffic on ports 80 and 443 from anywhere (0.0.0.0/0).  
    Allow SSH access (port 22\) only from specific IPs (e.g., your office network).  
5. Application Server Communication:  
    Allow inbound traffic only from the Security Group associated with the web servers, ensuring that only traffic from those servers can reach the application servers.  
6. Database Access:  
    Allow inbound traffic on the database port (e.g., 3306 for MySQL) only from the application server's Security Group.  
7. Restrict Outbound Traffic:  
    Limit outbound traffic from a resource to specific destinations for enhanced security.

---

# Default Behavior of Security Groups

 Inbound Rules: All traffic is denied by default.  
 Outbound Rules: All traffic is allowed by default.  
 You must explicitly define rules to allow traffic as per your application's requirements.

---

# Example Security Group Rules

## Web Server Security Group:

 Inbound Rules:  
   HTTP (TCP, Port 80\) from 0.0.0.0/0 (any IP).  
   HTTPS (TCP, Port 443\) from 0.0.0.0/0 (any IP).  
   SSH (TCP, Port 22\) from a specific IP range (e.g., 203.0.113.0/24).  
 Outbound Rules:  
   All traffic is allowed (default setting).

## Database Server Security Group:

 Inbound Rules:  
   MySQL (TCP, Port 3306\) from the Web Server Security Group.  
 Outbound Rules:  
   All traffic is allowed (default setting).

---

# Best Practices

8. Principle of Least Privilege:  
    Configure rules to allow only the minimum traffic required for the resource's operation.  
9. Use Security Group References:  
    Use Security Groups as sources or destinations instead of IP ranges for better manageability and dynamic updates.  
10. Monitor Rules:  
    Regularly review and update Security Group rules to avoid stale or overly permissive configurations.  
11. Limit Public Access:  
    Restrict public access by avoiding overly broad CIDR blocks like `0.0.0.0/0`, especially for sensitive services like SSH or database ports.  
12. Combine with Network ACLs:  
    Use Security Groups with Network ACLs (which operate at the subnet level) for an additional layer of security.

---

# Key Differences: Security Groups vs. Network ACLs

| Feature | Security Groups | Network ACLs |
| ----- | ----- | ----- |
| Scope | Instance level | Subnet level |
| Statefulness | Stateful | Stateless |
| Rules | Allow rules only | Allow and deny rules |
| Evaluation Order | All rules evaluated | Rules evaluated in numerical order |

---

AWS Security Groups are essential for defining and controlling resource access in a VPC, ensuring that only authorized traffic flows to and from your applications

