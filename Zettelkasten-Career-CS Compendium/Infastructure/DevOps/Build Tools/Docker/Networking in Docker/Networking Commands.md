---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Certainly\! Here are examples for creating custom networks, connecting containers to a network, and inspecting network configurations in Docker:

---

# Creating Custom Networks (`docker network create`)

You can create a custom Docker network using the `docker network create` command. This allows you to define your own network settings (e.g., custom subnet, driver).

## Example Command:

```shell
docker network create --driver bridge my-custom-network
```

*   
  **`--driver bridge`**: Specifies the driver for the network. The default is `bridge`, but you can also use `host`, `none`, or other network drivers (e.g., `overlay` for Docker Swarm).

* **`my-custom-network`**: The name of the custom network you're creating.

## Inspect the Custom Network:

To verify the custom network was created:

```shell
docker network ls
```

This lists all the networks, including `my-custom-network`.

---

# Connecting Containers to a Network (`docker network connect`)

Once you have a custom network, you can connect one or more containers to it using the `docker network connect` command.

## Example Command:

```shell
docker run -d --name container-1 nginx
docker network connect my-custom-network container-1
```

*   
  This command first creates a container (`container-1`) running Nginx, and then it connects `container-1` to the custom network `my-custom-network`.

## Connect Another Container:

```shell
docker run -d --name container-2 nginx
docker network connect my-custom-network container-2
```

Now, both `container-1` and `container-2` are connected to `my-custom-network` and can communicate with each other using their container names (`container-1` and `container-2`) as hostnames.

---

# Inspecting Network Configurations (`docker network inspect`)

You can inspect the details of a network to see its configuration, containers connected to it, IP ranges, etc., using the `docker network inspect` command.

## Example Command:

```shell
docker network inspect my-custom-network
```

This command provides detailed information about the `my-custom-network`, including:

* The network driver being used (e.g., `bridge`).

* The subnet and IP range allocated to the network.

* A list of containers currently connected to the network.

* The gateway for the network.

## Example Output:

```json
[
    {
        "Name": "my-custom-network",
        "Id": "a6d3b0f59a60",
        "Created": "2025-04-16T14:31:42.217712706Z",
        "Scope": "local",
        "Driver": "bridge",
        "IPAM": {
            "Driver": "default",
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Containers": {
            "f1d1e2d3a4f5": {
                "Name": "container-1",
                "EndpointID": "b0530d7a50b2",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "d1e2f3a4g5h6": {
                "Name": "container-2",
                "EndpointID": "c1234d7b1e2f",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

This output shows that:

* The network is using the `bridge` driver.

* The subnet for the network is `172.18.0.0/16`, and the gateway is `172.18.0.1`.

* Two containers (`container-1` and `container-2`) are connected, with IP addresses `172.18.0.2` and `172.18.0.3`, respectively.

---

 Summary of Commands

| Command | Purpose |
| ----- | ----- |
| `docker network create my-custom-network` | Create a custom network (default driver is `bridge`) |
| `docker network connect my-custom-network container-1` | Connect a container to a specific network |
| `docker network inspect my-custom-network` | Inspect details of a specific network, including containers connected |

---

Let me know if you'd like to dive deeper into any of these commands or if you need additional examples\!

