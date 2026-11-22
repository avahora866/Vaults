---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Absolutely\! Let’s break down **how to build custom Docker images** using the command:

```shell
docker build -t image-name .
```

---

# What Is a Docker Image?

A **Docker image** is a snapshot of an application and its environment. It includes everything the app needs to run: code, dependencies, OS packages, and runtime settings. You create an image from a **Dockerfile**, which contains a series of instructions for assembling the image.

---

# The `docker build` Command Explained

## Basic Syntax:

```shell
docker build [OPTIONS] -t <image-name> <path-to-Dockerfile>
```

## The Key Parts of the Command:

```shell
docker build -t image-name .
```

*   
  **`docker build`**: The main command to create a Docker image.

* **`-t image-name`**: The `-t` flag assigns a **tag** (name) to the image you're building. You can use a format like `name:tag`, e.g., `myapp:latest`.

* **`.`**: The dot (`.`) tells Docker to look for the **Dockerfile in the current directory**. You can specify another path if your Dockerfile is elsewhere.

---

# Example:

Let’s say you have the following directory structure:

```
my-flask-app/
│
├── Dockerfile
├── app.py
└── requirements.txt
```

And your Dockerfile looks like this:

```
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

## To build the image:

```shell
cd my-flask-app
docker build -t flask-demo .
```

This tells Docker to:

1. Read the Dockerfile in the current directory (`.`)

2. Execute the instructions inside it

3. Build an image

4. Tag it as `flask-demo`

---

# Verifying the Image Was Built

After the build completes, you can verify the image exists by running:

```shell
docker images
```

You’ll see something like:

```
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
flask-demo    latest    1a2b3c4d5e6f    10 seconds ago  120MB
```

---

# Using the Built Image

Once the image is built, you can run a container from it:

```shell
docker run -p 5000:5000 flask-demo
```

This starts a container from your custom image and maps port 5000 on your host to port 5000 in the container.

---

# Optional: Tagging with Version

You can tag an image with a specific version:

```shell
docker build -t flask-demo:v1 .
```

This is useful for versioning your builds.

---

# Summary

| Component | Purpose |
| ----- | ----- |
| `docker build` | Command to create a Docker image |
| `-t` | Tag the image with a name (and optionally a version) |
| `.` | The build context (directory where Dockerfile is located) |

---

Let me know if you’d like a step-by-step build example or how to push this image to Docker Hub\!

