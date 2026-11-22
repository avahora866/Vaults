---
tags:
  - BuildTool
---
Kubernetes (often abbreviated as K8s) is an open-source platform used to automate the deployment, scaling, and management of containerized applications. While Docker handles the containerization of applications, Kubernetes orchestrates the deployment and operation of those containers in a scalable and fault-tolerant way.

Here’s an overview of the key concepts in Kubernetes:

# Containers and Pods

 Containers: As mentioned before, containers are self-contained environments that include everything needed to run an application. They can be created using Docker or other containerization technologies.  
 Pods: A pod is the smallest and simplest unit in Kubernetes. A pod can contain one or more containers that are tightly coupled and share the same network, storage, and configuration. Typically, a pod contains a single container, but it can include multiple containers that work together (e.g., a web server and a sidecar container for logging).

# Nodes

 A node is a physical or virtual machine in a Kubernetes cluster. Each node contains the services necessary to run pods, including a container runtime (like Docker), a kubelet (which manages the state of the node), and a network proxy.  
 Nodes are grouped together in a cluster, and Kubernetes schedules pods to run on nodes in the cluster.

# Kubernetes Cluster

 A Kubernetes cluster consists of a set of nodes that run containerized applications. The cluster is managed by a control plane, which is responsible for orchestrating the deployment, scaling, and monitoring of applications across the nodes.  
 The control plane includes various components like the API server, controller manager, scheduler, and etcd (a distributed key-value store that holds cluster state).

# Deployments

 A deployment is a higher-level abstraction in Kubernetes that defines the desired state for running a set of identical pods (e.g., a web application). The deployment ensures that the specified number of replicas of a pod are running and takes care of rolling updates, scaling, and self-healing (restarting pods if they fail).  
 Kubernetes makes sure that the number of pods in a deployment matches the desired count, scaling up or down as needed.

# Services

 A service is a Kubernetes resource that defines how to access a set of pods. It provides a stable endpoint (IP address and DNS name) for a set of pods that may change over time due to scaling, failures, or restarts.  
 Services help decouple application components and allow them to communicate with each other even when pods are dynamically scheduled across nodes.

# Ingress

 An Ingress is a set of rules that allow external HTTP/S traffic to reach the services in a Kubernetes cluster. Ingress controllers manage the routing of traffic based on these rules.  
 It’s commonly used to expose HTTP/S-based applications (like web apps) to the internet, and it provides features like load balancing, SSL termination, and URL path-based routing.

# ConfigMaps and Secrets

 ConfigMap is used to store configuration data that can be consumed by pods at runtime. This allows you to separate configuration from the container image.  
 Secrets are similar to ConfigMaps but are used for storing sensitive information like passwords, tokens, or API keys. Secrets are stored in an encrypted format for security.

# Namespaces

 Namespaces provide a way to divide resources within a cluster. They allow multiple teams or applications to share the same cluster while ensuring that their resources (like pods, services, etc.) are isolated from one another.  
 They are often used for multi-tenant environments, testing, or different environments (dev, staging, production).

# Horizontal Pod Autoscaling (HPA)

 HPA automatically adjusts the number of pods in a deployment or replica set based on CPU utilization or other custom metrics. For example, if the traffic increases, HPA can scale the number of pods to meet the demand.

# Persistent Storage

 Kubernetes supports persistent storage using Persistent Volumes (PVs) and Persistent Volume Claims (PVCs). These resources provide a way to manage storage outside of the lifecycle of pods, meaning data can persist even if pods are terminated or moved.  
 Storage can be backed by various systems, including cloud storage (e.g., AWS EBS), network-attached storage, or even local disks.

# Key Features of Kubernetes:

 - Scalability: Kubernetes can automatically scale applications up or down based on demand, making it easier to manage traffic spikes or reduced load.  
 - Self-Healing: Kubernetes can automatically restart failed containers, replace and reschedule containers, and kill containers that don’t respond to health checks.  
 - Declarative Configuration: Kubernetes operates using declarative configuration. You specify the desired state of your application, and Kubernetes ensures that the actual state matches the desired state.  
 - Rolling Updates and Rollbacks: Kubernetes allows for smooth updates to applications, ensuring that there’s no downtime. It can also roll back to a previous version in case something goes wrong.  
 - Load Balancing: Kubernetes can distribute network traffic across multiple pods, improving availability and performance.

# Example Workflow

Create a Deployment: You define a deployment YAML file that describes the desired state of your application (e.g., a web app with three replicas of a pod).  

```yaml  
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: my-web-app  
spec:  
  replicas: 3  
  selector:  
    matchLabels:  
      app: web  
  template:  
    metadata:  
      labels:  
        app: web  
    spec:  
      containers:  
      - name: web  
        image: my-web-app-image  
        ports:  
        - containerPort: 80
```
1. Deploy the Application: You apply the deployment to the cluster:  
```  
kubectl apply -f deployment.yaml
```
2. Expose the Application: You create a service to expose your application to the outside world:  
```yaml    
apiVersion: v1  
kind: Service  
metadata:  
  name: my-web-app-service  
spec:  
  selector:  
    app: web  
  ports:  
    - protocol: TCP  
      port: 80  
      targetPort: 80  
  type: LoadBalancer
```
3. Scale the Application: You can scale the number of pods up or down using:  
```
kubectl scale deployment my-web-app --replicas=5
```
1. Kubernetes automates much of the operational complexity associated with managing large-scale applications, making it ideal for managing [[Microservices Architecture|microservices]], hybrid clouds, and large-scale distributed systems.

