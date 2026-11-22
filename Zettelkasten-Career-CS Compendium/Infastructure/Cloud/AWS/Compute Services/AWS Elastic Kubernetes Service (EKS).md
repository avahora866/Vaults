---
tags:
  - AWS
---
**Amazon Elastic Kubernetes Service (EKS)** is a fully managed service that makes it easy to run and scale Kubernetes applications in AWS. Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. EKS allows you to deploy Kubernetes clusters without having to manage the underlying infrastructure, while providing seamless integration with other AWS services for enhanced security, networking, and scalability.

# Key Features of Amazon EKS

1. **Fully Managed Kubernetes**:  
   * EKS manages the Kubernetes control plane (the components responsible for scheduling, scaling, and maintaining the state of your applications), allowing you to focus on your containerized applications instead of managing Kubernetes clusters.  
2. **Integration with AWS Services**:  
   * EKS integrates with other AWS services, such as **Elastic Load Balancing (ELB)** for load balancing, [[Identity and Access Management (IAM)|IAM]] for access control, [[VPC Security Groups|VPC]] for networking, [[AWS CloudWatch|CloudWatch]] for logging and monitoring, and **ECR** for container image storage.  
3. **High Availability and Scalability**:  
   * EKS runs [[Kubernetes|Kubernetes]] clusters across multiple **availability zones (AZs)** within a region for fault tolerance and high availability.  
   * EKS supports horizontal scaling, automatically adding or removing worker nodes based on demand using **Auto Scaling Groups**.  
4. **Security**:  
   * EKS integrates with [[Identity and Access Management (IAM)|IAM]] to provide fine-grained access control to resources within the cluster.  
   * You can use [[VPC Security Groups|AWS VPC]] to isolate your Kubernetes cluster network and ensure private, secure communication between the nodes and applications.  
   * EKS supports [[Kubernetes|Kubernetes]] **RBAC** (Role-Based Access Control) and integrates with AWS security services to safeguard your applications.  
5. **Managed Node Groups**:  
   * EKS simplifies node management with **managed node groups**, which automatically provision and manage the EC2 instances (worker nodes) running your containerized applications.  
   * These node groups are managed directly by EKS and allow for scaling based on workload demands.  
6. **Multi-Region Support**:  
   * EKS allows you to run Kubernetes clusters across multiple regions and manage them from a single control plane.  
7. **Integrated Monitoring and Logging**:  
   * [[AWS CloudWatch|Amazon CloudWatch]] integrates with EKS to provide monitoring, logging, and alerting, helping you track your application’s health and performance.  
   * You can use **Amazon CloudWatch Container Insights** to view detailed metrics about your containers, nodes, and clusters.  
8. **Support for** [[AWS Fargate|Fargate]]:  
   * EKS can use [[AWS Fargate|AWS Fargate]] as the compute engine, allowing you to run Kubernetes pods without managing any EC2 instances. [[AWS Fargate|Fargate]] automatically provisions the underlying compute resources needed to run the containers.  
9. **Custom** [[Kubernetes|Kubernetes]] **Features**:  
   * EKS supports all the standard Kubernetes features such as **Pods**, **Deployments**, **Services**, **Namespaces**, and **Ingress**.  
   * It also supports **Helm** charts for easier management and deployment of applications.

---

# How Amazon EKS Works

1. **Create an EKS Cluster**:  
   * You start by creating an EKS cluster, which includes the Kubernetes control plane managed by AWS. This control plane is responsible for managing the worker nodes and scheduling the containers.  
2. **Provision Worker Nodes**:  
   * Once the control plane is set up, you provision worker nodes (EC2 instances) to run the containers. You can either use **managed node groups** or manually manage your nodes.  
3. **Deploy Applications**:  
   * You deploy containerized applications to the cluster using Kubernetes manifests or Helm charts. Kubernetes schedules your containers across the available nodes in the cluster.  
4. **Scaling and Monitoring**:  
   * EKS allows you to scale your applications up or down based on demand. Kubernetes automatically distributes containers across your worker nodes, and you can set up horizontal pod autoscaling to adjust the number of replicas of your applications based on resource usage.  
   * You can monitor the health and performance of your applications using **CloudWatch**.  
5. **Networking and Security**:  
   * EKS uses [[VPC Security Groups|AWS VPC]] for networking, ensuring that the Kubernetes cluster runs in a secure, isolated network. It also integrates with [[Identity and Access Management (IAM)|IAM]] for secure access to AWS services and **Kubernetes RBAC** for controlling user access within the cluster.

---

# Benefits of Amazon EKS

1. **Fully Managed**:  
   * EKS eliminates the need to manage the Kubernetes control plane and infrastructure, freeing up your team to focus on building applications.  
2. **Scalability**:  
   * Automatically scale your Kubernetes workloads with features like horizontal pod autoscaling, cluster auto-scaling, and integration with AWS auto scaling for EC2 instances.  
3. **Integration with AWS Services**:  
   * Seamlessly integrate with other AWS services such as Elastic Load Balancing, [[Identity and Access Management (IAM)|IAM]], [[AWS CloudWatch|CloudWatch]], and [[VPC Security Groups|VPC]] to improve security, monitoring, and networking for your applications.  
4. **Security and Compliance**:  
   * EKS leverages AWS security services, such as [[Identity and Access Management (IAM)|IAM]] for authentication and authorization, and provides a highly secure environment with [[VPC Security Groups|VPC]] isolation and RBAC control within Kubernetes.  
5. **High Availability**:  
   * EKS is built for high availability, deploying Kubernetes control planes across multiple AZs to ensure that your applications remain available even in the event of a failure in one AZ.  
6. **Multi-Cloud and Hybrid Deployments**:  
   * EKS supports multi-cloud and hybrid architectures, allowing you to run Kubernetes clusters across AWS, on-premises, and other cloud environments.  
7. [[AWS Fargate|Fargate Support]]:  
   * With [[AWS Fargate|AWS Fargate]] integration, you can run Kubernetes workloads without managing EC2 instances, enabling serverless containerized application deployment.  
8. **Managed Upgrades**:  
   * EKS handles Kubernetes version upgrades and patches automatically, ensuring your cluster is always up-to-date with the latest features and security patches.

---

# EKS vs ECS

* [[AWS Elastic Container Service (ECS)|Amazon ECS (Elastic Container Service)]] is another container orchestration service offered by AWS. The key difference between EKS and ECS is that EKS is based on [[Kubernetes|Kubernetes]], while ECS is AWS’s native container orchestration service.  
  * **EKS**: Ideal for users who are familiar with Kubernetes and need to leverage its ecosystem, tools, and flexibility.  
  * **ECS**: Suitable for users who prefer AWS-native services and do not need the full complexity of Kubernetes.

---

# Use Cases for Amazon EKS

1. [[Microservices Architecture|Microservices Architectures]]:  
   * Manage and deploy multiple microservices across a Kubernetes cluster with automated scaling and resource management.  
2. **CI/CD Pipelines**:  
   * Automate the deployment and scaling of containerized applications using EKS in a CI/CD pipeline, integrating with AWS CodePipeline and CodeBuild.  
3. **Hybrid Applications**:  
   * Run hybrid applications that span across AWS and on-premises infrastructure by extending Kubernetes clusters across multiple environments.  
4. **Machine Learning and AI**:  
   * Run containerized machine learning and AI workloads that require scalability and high availability.  
5. **Web Applications**:  
   * Host containerized web applications with load balancing, auto-scaling, and high availability using EKS.

