---
tags:
  - FORMAT
  - Theory
---
It lets you define what your API does—its endpoints, parameters, request/response formats—using a structured file (usually YAML or JSON). This file can then be used for:

- **API documentation**
- **Client SDK generation**
- **Server stub generation**
- **Validation and testing**

---

# Components of an OpenAPI Specification:

Example in YAML:

```yaml
openapi: 3.0.0
info:
  title: Example API
  version: 1.0.0
paths:
  /users:
    get:
      summary: List all users
      responses:
        '200':
          description: A list of users
```

- `openapi`: Version of the spec
- `info`: Metadata about the API
- `paths`: Defines endpoints and operations (e.g., `GET /users`)
- Each method (`get`, `post`, etc.) includes request parameters, response types, status codes, etc.

---

# Swagger vs OpenAPI:

- **Swagger** was the original project
- **OpenAPI** is the specification now maintained by the **OpenAPI Initiative**
- Swagger still refers to tools built around OpenAPI:
    - **Swagger Editor** (write/edit specs)
    - **Swagger UI** (interactive API docs)
    - **Swagger Codegen** (generate client/server code)

---

# Why it's useful:

- **Standardized** API definition
- Reduces human error in documentation
- Allows tools to automate boilerplate
- Makes it easier for teams and third parties to understand and integrate with your API