---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Certainly\! Below are **examples** of the Docker commands you've requested, explaining each in detail.

---

# Pulling Images from Docker Hub (`docker pull image-name`)

The `docker pull` command is used to download a Docker image from a registry (like Docker Hub) to your local machine.

## Example:

Let’s say you want to pull the official **Ubuntu** image from Docker Hub. You can use the following command:

```shell
docker pull ubuntu:latest
```

This command does the following:

* `docker pull`: Tells Docker to pull an image.

* `ubuntu:latest`: Specifies the image you want to pull from Docker Hub. Here, `ubuntu` is the image name, and `latest` is the tag indicating the version. If no tag is specified, Docker defaults to `latest`.

Once the pull is complete, Docker will store the `ubuntu:latest` image locally on your machine, and you can use it to create containers.

---

# Running Containers (`docker run image-name`)

The `docker run` command is used to create and start a container from an image. It can also be used to specify additional options like port mappings and environment variables.

## Example:

Let’s use the `ubuntu` image (that you just pulled) and run a simple container that opens a bash shell inside it.

```shell
docker run -it ubuntu:latest /bin/bash
```

This command does the following:

* `docker run`: Runs a container from an image.

* `-it`: Combines two options: `-i` (interactive) and `-t` (allocate a terminal), allowing you to interact with the container.

* `ubuntu:latest`: Specifies the image to run, in this case, the `ubuntu` image with the `latest` tag.

* `/bin/bash`: Specifies the command to run inside the container — in this case, it opens the bash shell.

This will start a container and drop you into a bash prompt inside the running Ubuntu container.

---

# Listing Running and Stopped Containers (`docker ps -a`)

The `docker ps -a` command lists all containers, both running and stopped. By default, `docker ps` only lists running containers, but `docker ps -a` shows all containers on the system.

## Example:

To list all containers (including stopped ones), run the following:

```shell
docker ps -a
```

This command outputs a list of all containers, showing information like:

* **Container ID**: A unique identifier for each container.

* **Image**: The image used to create the container.

* **Command**: The command the container is running.

* **Status**: The current state (e.g., running, exited).

* **Ports**: Port mappings between the container and the host system.

For example, the output might look like this:

```
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                      PORTS     NAMES
b69e7b7d8e64   ubuntu:latest  "/bin/bash"              3 hours ago     Exited (0) 2 hours ago                angry_johnson
f70e9f5d430d   ubuntu:latest  "/bin/bash"              4 hours ago     Up 2 hours                   80/tcp    hopeful_lehmann
```

Here:

* **`angry_johnson`** is a container that has stopped.

* **`hopeful_lehmann`** is running.

---

# Stopping, Restarting, and Removing Containers

## Stopping Containers (`docker stop`)

The `docker stop` command is used to stop a running container.

## Example:

To stop a running container (e.g., `hopeful_lehmann`), run the following:

```shell
docker stop hopeful_lehmann
```

This will gracefully stop the container named `hopeful_lehmann`. If the container doesn’t stop within 10 seconds, Docker will forcefully terminate it.

## Restarting Containers (`docker restart`)

The `docker restart` command is used to restart a container (i.e., stop and then start it).

## Example:

To restart the container `hopeful_lehmann`, run the following:

```shell
docker restart hopeful_lehmann
```

This command will stop the container and immediately start it again.

## Removing Containers (`docker rm`)

The `docker rm` command is used to remove one or more stopped containers.

## Example:

To remove a stopped container (`angry_johnson`), run the following:

```shell
docker rm angry_johnson
```

This will **delete** the container from the system. If you try to remove a container that is still running, Docker will give you an error.

If you want to remove multiple containers at once, you can specify multiple container IDs or names:

```shell
docker rm container1 container2 container3
```

To forcefully remove a running container, you can use the `-f` option:

```shell
docker rm -f hopeful_lehmann
```

This will stop the container and remove it in one command.

---

# Summary of Commands:

| Command | Description | Example |
| ----- | ----- | ----- |
| `docker pull image-name` | Pulls an image from a registry (e.g., Docker Hub) | `docker pull ubuntu:latest` |
| `docker run image-name` | Runs a container from an image | `docker run -it ubuntu:latest /bin/bash` |
| `docker ps -a` | Lists all containers, both running and stopped | `docker ps -a` |
| `docker stop container-name` | Stops a running container | `docker stop hopeful_lehmann` |
| `docker restart container-name` | Restarts a container (stops and then starts it again) | `docker restart hopeful_lehmann` |
| `docker rm container-name` | Removes a stopped container | `docker rm angry_johnson` |

---

These commands give you the basic functionality to pull images, run containers, manage container lifecycles, and inspect running or stopped containers. Let me know if you'd like additional examples or more details\!

