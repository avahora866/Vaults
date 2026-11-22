---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Absolutely\! Here's a **comprehensive explanation of the Docker CLI**—what it is, how it works, its key commands, and why it’s such a central part of working with Docker.

---

# What is Docker CLI?

The **Docker CLI** (Command Line Interface) is the **primary tool** for interacting with Docker. When you install Docker, you gain access to the `docker` command, which allows you to **communicate with the Docker Daemon** to build, run, and manage containers and images.

In simple terms, the Docker CLI is the **user-facing tool** used to control Docker.

---

# How the Docker CLI Works

* The CLI sends **commands** (via the `docker` command) to the **Docker Daemon**.

* These commands are translated into **API calls** using the Docker Engine REST API.

* The Daemon processes the commands and returns results to the CLI.

## Example:

```shell
docker run nginx
```

This command:

1. Asks the Docker Daemon to pull the **nginx** image (if not present).

2. Creates a new container from the image.

3. Starts the container.

---

# Key Docker CLI Commands

Here are the most important and commonly used commands, grouped by category:

## Working with Containers

| Command | Description |
| ----- | ----- |
| `docker run` | Creates and starts a container |
| `docker ps` | Lists running containers |
| `docker ps -a` | Lists all containers (running \+ stopped) |
| `docker start <container>` | Starts a stopped container |
| `docker stop <container>` | Stops a running container |
| `docker restart <container>` | Restarts a container |
| `docker rm <container>` | Removes a container |
| `docker exec` | Runs a command inside a running container |
| `docker logs <container>` | Fetches logs from a container |
| `docker inspect <container>` | Displays detailed container information |

---

## Working with Images

| Command | Description |
| ----- | ----- |
| `docker build` | Builds an image from a Dockerfile |
| `docker pull <image>` | Downloads an image from a registry |
| `docker push <image>` | Uploads an image to a registry |
| `docker images` | Lists locally available images |
| `docker rmi <image>` | Deletes an image |
| `docker tag` | Adds a new tag to an image |

---

## Dockerfile and Compose (Advanced Dev)

| Command | Description |
| ----- | ----- |
| `docker-compose up` | Starts services defined in `docker-compose.yml` |
| `docker-compose down` | Stops and removes services and containers |
| `docker-compose build` | Builds services from Dockerfiles |

Note: `docker compose` (no dash) is now the standard in newer Docker versions.

---

## Working with Volumes and Networks

| Command | Description |
| ----- | ----- |
| `docker volume create` | Creates a named volume |
| `docker volume ls` | Lists all volumes |
| `docker volume rm` | Deletes a volume |
| `docker network ls` | Lists Docker networks |
| `docker network create` | Creates a custom network |
| `docker network inspect` | Shows network details |

---

## System and Cleanup

| Command | Description |
| ----- | ----- |
| `docker system prune` | Cleans up unused containers, images, volumes |
| `docker info` | Shows Docker system info |
| `docker version` | Displays Docker client and daemon versions |

---

# Benefits of the Docker CLI

| Benefit | Description |
| ----- | ----- |
| **Simplicity** | Easy to run and manage containers with simple commands |
| **Automation-friendly** | Works well in scripts and CI/CD pipelines |
| **Fine-grained control** | Lets you specify ports, volumes, networks, environment variables, etc. |
| **Speed** | Fast feedback loop during development |
| **Consistency** | Standardized across platforms and environments |

---

# Command Structure Format

```shell
docker [COMMAND] [OPTIONS] [ARGS]
```

## Example with options:

```shell
docker run -d -p 8080:80 --name my-nginx nginx
```

*   
  `-d`: Run in detached mode

* `-p 8080:80`: Map host port 8080 to container port 80

* `--name my-nginx`: Assign a name to the container

---

# CLI vs GUI (Docker Desktop Dashboard)

| Docker CLI | Docker Desktop (GUI) |
| ----- | ----- |
| More powerful and scriptable | Easier to use for beginners |
| Preferred for automation | Better for visual monitoring |
| Can be used over SSH/remotely | Limited to local interaction |

---

# In Summary

| Component | Description |
| ----- | ----- |
| **Docker CLI** | Command-line tool to interact with Docker |
| **Talks to** | Docker Daemon (`dockerd`) via REST API |
| **Purpose** | Build, run, manage containers, images, volumes, networks, etc. |
| **Used in** | Local development, scripting, CI/CD pipelines, production tooling |

---

Let me know if you’d like a cheat sheet of common Docker CLI commands or a breakdown of how to use the CLI in a real-world scenario\!

