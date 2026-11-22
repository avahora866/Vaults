---
tags:
  - FORMAT
  - Theory
---
**API versioning** is how you manage changes to an API without breaking existing clients.

# Why it's needed:

- APIs evolve (new features, changes in structure, bug fixes)
- You need to support older clients while letting new ones use updated functionality

---

# Common Versioning Strategies:

1. **URI versioning (most common)**
    
    ```
    GET /v1/users
    GET /v2/users
    ```
    
    - Pros: Easy to see and test
    - Cons: Breaks RESTful “resource” purity (URI should identify the resource, not version)
2. **Query parameter versioning**
    
    ```
    GET /users?version=1
    ```
    
    - Pros: Simple to implement
    - Cons: Can be less visible to caching proxies or tools
3. **Header versioning**
    
    ```
    GET /users
    Headers: X-API-Version: 1
    ```
    
    - Pros: Keeps URL clean
    - Cons: Harder to test/debug manually; more complex
4. **Content negotiation (Accept header)**
    
    ```
    Accept: application/vnd.company.v1+json
    ```
    
    - Pros: RESTful; version tied to representation
    - Cons: Complexity; few clients implement it well

---

# Semantic Versioning (Optional, but useful):

- Format: `MAJOR.MINOR.PATCH` (e.g., `v2.3.1`)
- Bump:
    - **MAJOR**: Breaking changes
    - **MINOR**: New, backward-compatible features
    - **PATCH**: Bug fixes only

---

# Best Practices:

- Use URI versioning unless you have a strong reason not to
- Keep versions stable—don’t change behavior in a version after release
- Deprecate old versions clearly and give clients time to upgrade