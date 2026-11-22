---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Certainly\! Docker provides several **network modes** that determine how containers communicate with each other and the outside world. The three default network types in Docker are **Bridge**, **Host**, and **None**. Each has its own specific use cases, behavior, and networking configurations. Let’s break them down:

---

# Bridge Network

## Overview:

* **Bridge** is the **default network** for containers when no other network is specified.

* It is a **software-defined virtual network** that allows containers to communicate with each other and the host system.

* The bridge network acts as a **private internal network** where containers are connected by default.

## Behavior:

* When a container is connected to the bridge network, Docker creates a **virtual bridge** (usually named `docker0`), and each container gets an **IP address** on that bridge network.

* Containers on the bridge network can communicate with each other via their IP addresses and can access the host through **port mappings**.

* Containers on the bridge network **cannot communicate directly with the outside world** unless explicitly configured (using port mappings or NAT).

## Example Use Case:

* **Container-to-container communication** within the same Docker host.

* **Accessing services** from the host (e.g., exposing web servers).

## Example Command:

```shell
docker run -d --name my-container --network bridge nginx
```

This command will start a container running the Nginx image and attach it to the default `bridge` network.

---

# Host Network

## Overview:

* The **Host** network mode makes a container share the **network namespace** of the host machine.

* This means that the container does **not get its own IP address** but instead uses the host's IP address for networking.

## Behavior:

* The container shares the **same network interface** as the host. It can access the host's network resources, and vice versa.

* The **port mapping** mechanism is not needed when using the host network because the container directly binds to the host’s network.

* It provides **better performance** since there’s no need for network address translation (NAT) or bridging between the container and the host.

## Example Use Case:

* When you need **direct access to the host’s network** or require **high-performance network throughput** (e.g., for low-latency applications).

## Example Command:

```shell
docker run -d --name my-container --network host nginx
```

This starts the container and connects it directly to the host’s network interface.

---

# None Network

## Overview:

* The **None** network mode disables all networking for the container.

* The container will have no network interfaces and will not be able to communicate with the host or other containers.

## Behavior:

* Containers in the **None** network mode are completely isolated from all networks.

* This network mode is useful when you need to **restrict a container from accessing the network** or when the container will manage its own networking through custom configurations (such as specific network interfaces).

## Example Use Case:

* Containers that should run in **complete isolation** from all external networks.

* Specialized workloads where networking is managed by external systems or configurations.

## Example Command:

```shell
docker run -d --name my-container --network none nginx
```

This starts the container without any network connectivity.

---

# Summary Comparison of Default Networks

| Feature | Bridge | Host | None |
| ----- | ----- | ----- | ----- |
| **Default Mode** | Yes (for containers when no other network is set) | No | No |
| **IP Address** | Container gets its own private IP address | Container shares the host’s IP address | No network interfaces |
| **Communication** | Containers can communicate with each other on the bridge network | Container uses the host network directly | No network connectivity |
| **Port Mapping** | Required for accessing the container from outside | No need for port mapping, container uses host’s ports | No network access (no port mapping) |
| **Use Case** | Inter-container communication on the same host, connecting containers to external world | When you need direct access to the host's network | Containers that need no network access |
| **Network Isolation** | Containers are isolated from the host and other networks | No isolation from the host network | Full isolation |

---

# When to Use Each Network Mode?

* **Bridge**: Use this for **general container communication** where you want isolation between containers but still need access to the outside world (via port forwarding).

* **Host**: Use this when you need **direct access** to the host’s network, or for **performance-sensitive applications** that require low-latency or high-throughput networking.

* **None**: Use this when you want **complete isolation** from the network, or if the container will handle its own networking configuration (e.g., managing network interfaces manually).

---

# Example Commands and Additional Tips

## Switch a Running Container’s Network Mode:

You can set the network mode at the time of container creation. For example, if you started a container with the bridge network:

```shell
docker run -d --name my-container --network bridge nginx
```

And later want to change it to **host network**:

```shell
docker network disconnect bridge my-container
docker network connect host my-container
```

This disconnects the container from the bridge network and reconnects it to the host network.

---

Let me know if you'd like more detailed examples or have any specific questions on networking with Docker\!

