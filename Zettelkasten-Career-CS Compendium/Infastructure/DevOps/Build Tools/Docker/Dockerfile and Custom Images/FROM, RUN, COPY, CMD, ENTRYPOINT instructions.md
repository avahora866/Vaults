---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Absolutely\! Here's a **comprehensive explanation** of some of the most essential Dockerfile instructions: `FROM`, `RUN`, `COPY`, `CMD`, and `ENTRYPOINT`.

---

# `FROM`

The `FROM` instruction **specifies the base image** for your Docker image. It must be the **first instruction** in every Dockerfile (except for `ARG` used before it in special cases).

## Purpose:

It defines the starting point for your build process. All other instructions in the Dockerfile build on top of this base image.

## Example:

```
FROM ubuntu:20.04
```

This uses Ubuntu 20.04 as the base image.

## Common Options:

* `FROM node:18`

* `FROM python:3.11-slim`

* `FROM alpine` (lightweight Linux distribution)

---

# `RUN`

The `RUN` instruction executes commands **at build time**, when the Docker image is being created. It is typically used to install packages or perform setup steps.

## Purpose:

Install software, update packages, modify the file system during image creation.

## Example:

```
RUN apt-get update && apt-get install -y curl
```

This updates package lists and installs `curl` inside the image.

## Best Practice:

Use `&&` to chain commands in a single `RUN` line to reduce image layers and size.

---

# `COPY`

The `COPY` instruction copies **files and directories from your host machine** (the one building the image) into the Docker image.

## Purpose:

Bring application code, configuration files, or assets into the container.

## Example:

```
COPY . /app
```

This copies all files from the current directory on the host machine into `/app` inside the image.

## Important Notes:

* It’s preferred over `ADD` for simple copy operations.

* Use `.dockerignore` to exclude unnecessary files from being copied.

---

# `CMD`

The `CMD` instruction provides the **default command** that gets executed **when the container starts**. It can be overridden at runtime using `docker run`.

## Purpose:

Tell Docker what to run when the container is launched.

## Syntax:

* Shell form:

```
CMD python app.py
```

*   
  Exec form (recommended):

```
CMD ["python", "app.py"]
```

## Important Note:

Only the **last `CMD`** in a Dockerfile is used. If multiple `CMD`s exist, the earlier ones are ignored.

---

# `ENTRYPOINT`

The `ENTRYPOINT` instruction defines the **main command** to run when the container starts. Unlike `CMD`, it **cannot be easily overridden** with `docker run` unless you explicitly pass the `--entrypoint` flag.

## Purpose:

Make the container behave like a specific command or application.

## Example:

```
ENTRYPOINT ["python", "app.py"]
```

This means the container always runs `python app.py` when started.

## Used With CMD:

You can combine `ENTRYPOINT` and `CMD`:

```
ENTRYPOINT ["python"]
CMD ["app.py"]
```

This results in `python app.py` when the container starts, but allows overriding `CMD` to run a different script without changing the entry point.

---

# Summary Table

| Instruction | Runs At | Used For | Overridable? |
| ----- | ----- | ----- | ----- |
| `FROM` | Build time | Specifies base image | No |
| `RUN` | Build time | Executes commands to build the image | No |
| `COPY` | Build time | Copies files from host to image | No |
| `CMD` | Container start | Sets default command to run in container | Yes |
| `ENTRYPOINT` | Container start | Sets fixed main command for container | Not easily |

---

Let me know if you'd like examples combining these instructions into a full Dockerfile\!

