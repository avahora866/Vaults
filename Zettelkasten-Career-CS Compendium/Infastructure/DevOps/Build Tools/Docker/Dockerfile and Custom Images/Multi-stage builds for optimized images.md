---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Absolutely\! Let’s dive into **multi-stage builds** in Docker—one of the most powerful features for creating **lean, production-ready images**.

---

# ✅ What Are Multi-Stage Builds?

**Multi-stage builds** allow you to use **multiple `FROM` statements** in a single Dockerfile. Each `FROM` starts a new stage. You can copy files from one stage to another using the `--from` flag.

## 🎯 Purpose:

To **separate the build environment from the final runtime image**, so that the final image only includes the minimal files necessary to run the app—without compilers, build tools, or source files.

---

# 🛠 Why Use Multi-Stage Builds?

| Benefit | Description |
| ----- | ----- |
| Smaller Image Size | No need to include compilers, dev dependencies, etc. |
| Cleaner and More Secure Images | No debug tools, no sensitive build data |
| Reusable Stages | Can copy only needed artifacts to final stage |
| Easier to Maintain | Keeps build logic in one file, instead of separate files |

---

# 🧪 Simple Example – Building a Go App

Let’s say you have a simple Go app (`main.go`):

```go
package main
import "fmt"
func main() {
    fmt.Println("Hello, Docker multi-stage build!")
}
```

## Dockerfile with Multi-Stage Build:

```
 Stage 1: Build
FROM golang:1.21 AS builder

WORKDIR /app
COPY . .
RUN go build -o app

 Stage 2: Production
FROM alpine:latest

WORKDIR /app
COPY --from=builder /app/app .

CMD ["./app"]
```

## Explanation:

* **Stage 1 (`builder`)**:

  * Uses the official Go image.

  * Builds the Go binary (`go build -o app`).

  * This stage includes all the dev tools and source code.

* **Stage 2 (final)**:

  * Uses a minimal Alpine image.

  * Only copies the compiled binary from the first stage (`COPY --from=builder`).

  * The resulting image is small and secure.

## Build it:

```shell
docker build -t go-multistage-app .
```

## Run it:

```shell
docker run go-multistage-app
```

---

# 🧼 Real-World Use Cases

## 🧪 Node.js \+ React Example:

```
 Stage 1: Build the React app
FROM node:18 AS builder

WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

 Stage 2: Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

This builds a production React app and serves it using Nginx—with **only static files** ending up in the final image.

---

# 🔁 Reusing Stages (Optional Optimization)

You can name stages and reference them multiple times:

```
FROM node:18 AS build-stage
 ...
FROM nginx:alpine AS production-stage
COPY --from=build-stage ...
```

---

# 🧩 Summary

| Feature | Description |
| ----- | ----- |
| Multiple `FROM` lines | Each starts a new stage in the build process |
| `AS` keyword | Names a build stage so it can be referenced later |
| `COPY --from` | Pulls artifacts from earlier stages |
| Result | Smaller, more secure, and efficient Docker images |

---

Let me know if you'd like an example tailored to a specific language or framework\!

