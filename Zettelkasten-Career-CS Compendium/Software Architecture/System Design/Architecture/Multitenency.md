---
tags:
  - SystemDesign
---
Multitenancy is a software architecture where a single instance of an application serves multiple users, called *tenants*. Each tenant is logically separated but shares common resources like databases, computing power, and infrastructure.

# How Multitenancy Works

* **Shared Resources:** Multiple tenants share the same application instance, but they experience it as if it's their own dedicated system.

* **Logical Separation:** Each tenant’s data and configurations are kept separate through unique identifiers or partitions.

* **Customization:** Tenants may have personalized settings, permissions, and access controls.

# Types of Multitenancy

1. **Database-Level Multitenancy**

   * **Shared Database, Shared Schema:** All tenants share the same database and tables, with tenant IDs distinguishing records.

   * **Shared Database, Separate Schemas:** Each tenant has its own schema within the same database.

   * **Separate Databases:** Each tenant has its own independent database.

2. **Application-Level Multitenancy**

   * **Single Instance:** A single application serves all tenants but ensures tenant-specific data isolation.

   * **Multiple Instances:** Each tenant gets a separate application instance, offering more isolation at the cost of efficiency.

# Benefits of Multitenancy

* **Cost-Efficiency:** Shared infrastructure reduces costs.

* **Scalability:** Easier to onboard new tenants without setting up new infrastructure.

* **Simplified Maintenance:** Updates and security patches can be applied universally.

* **Resource Optimization:** Better utilization of server resources.

# Challenges of Multitenancy

* **Security Risks:** A vulnerability in one tenant could affect others if not properly isolated.

* **Performance Issues:** High usage by one tenant could impact others.

* **Complex Data Management:** Ensuring proper separation and retrieval of tenant-specific data.

# Examples of Multitenancy

* **Cloud Services (AWS, Azure, Google Cloud)**

* **SaaS Applications (Salesforce, Slack, Google Workspace)**

* **Hosting Providers (Shared web hosting environments)**