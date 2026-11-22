---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Certainly\! Here's a comprehensive explanation of **bind mounts** in Docker, how they work, and when to use them.

---

# What Are Bind Mounts?

**Bind mounts** allow you to **mount a specific file or directory from your host machine into a container**. Unlike Docker volumes (which are managed by Docker), bind mounts give you **direct access to the filesystem** of your host machine.

This is especially useful during development when you want to:

* Share source code with a container

* Modify files on the host and see updates instantly inside the container

* Read or write logs or outputs from a container to the host

---

# How Bind Mounts Differ from Volumes

| Feature | Bind Mounts | Volumes |
| ----- | ----- | ----- |
| Managed by Docker | No – relies on exact host paths | Yes – stored under Docker’s internal directory |
| Setup | Specify full host path | Just give a name; Docker handles storage |
| Use Case | Local development, direct file sync | Production data storage, portability |
| Flexibility | Full control over exact mount location | More abstract and portable |

---

# Basic Syntax

```shell
docker run -v /host/path:/container/path <image>
```

*   
  **/host/path**: Absolute path on the host machine

* **/container/path**: Path where the host directory will appear inside the container

---

# Practical Example

Imagine you have a local directory with a Node.js app:

```shell
/project
  ├── app.js
  └── package.json
```

You want to run it inside a container and share the code.

## Run the container with a bind mount:

```shell
docker run -it --rm \
  -v $(pwd):/usr/src/app \
  -w /usr/src/app \
  node:18 \
  node app.js
```

## Explanation:

* `-v $(pwd):/usr/src/app`: Mounts your current directory into `/usr/src/app` in the container

* `-w /usr/src/app`: Sets the working directory inside the container

* `node:18`: Uses the official Node.js image

* `node app.js`: Runs your app inside the container

Any changes you make to files on your host will reflect **instantly** inside the container.

---

# Read-Only Bind Mounts

To prevent a container from modifying host files:

```shell
docker run -v /host/path:/container/path:ro <image>
```

*   
  `:ro` makes the mount read-only

---

# Common Use Cases

1. **Development**: Share source code with live reload tools (e.g., for Node.js, Python, etc.)

2. **Accessing Configs**: Mount a config file or secrets file from the host

3. **Writing Logs**: Allow a container to write logs to a host directory

4. **Static Assets**: Serve static files stored on the host via a web server in the container

---

# Security Considerations

* Containers can access and modify any part of the mounted host path

* Incorrect permissions can allow containers to overwrite or delete important host files

* Avoid bind mounting sensitive host directories unless absolutely necessary

---

# Summary

| Feature | Description |
| ----- | ----- |
| Direct host-container link | Mounts exact paths from the host into the container |
| Ideal for development | Lets you edit code on the host and see changes in the container |
| More control, less abstraction | You control exactly what files are shared and where |
| Not portable | Tied to the host filesystem, so it’s not ideal for production |

---

Let me know if you’d like an example involving a specific app or development setup\!

