---
tags:
  - Spring
---
# Authentication (Proving Identity)

This is the process of verifying _who_ the user is. It happens in two stages:

## Stage 1: Logging In

- The user provides **credentials** (e.g., username/password, OAuth token, API key).
- Spring Security uses the **authentication mechanism** configured (`JWT`, `Basic Auth`, `OAuth2`, etc.).
- If authentication is **successful**, Spring Security creates an **Authentication object** (usually stored in the `SecurityContext`).

## Stage 2: Sending Requests

- Every time the user makes a request, they must **prove their identity** again:
    - **Session-based authentication** → A session ID (stored in cookies) is checked.
    - **Token-based authentication (JWT, OAuth)** → A token is sent in the request header and validated.
    - **Basic Authentication** → The username and password are sent in every request (not recommended).

# Authorization (Checking Permissions)

Once the user is authenticated, we check _what_ they are allowed to do.

- Spring Security uses **roles, permissions, or policies** to decide access.
- Authorization is checked at different levels:
    - **URL-based authorization** → `"Only admins can access /admin"` (configured in `SecurityFilterChain`).
    - **Method-level authorization** → `@PreAuthorize("hasRole('ADMIN')")` on service methods.
    - **Custom authorization rules** → Using expressions, ACLs, or context-based policies.

---

# Example Flow: JWT Authentication with Role-Based Authorization

1. **User Logs In**
    - Sends a `POST` request to `/login` with username/password.
    - If successful, the server returns a **JWT token**.
2. **User Sends a Request**
    - Includes the **JWT token** in the `Authorization` header (`Bearer <token>`).
3. **Authentication Check**
    - The system verifies the JWT token.
    - If valid, it extracts the user's identity and roles.
4. **Authorization Check**
    - The system checks if the user has the right role/permission.
    - Example: If the request is to `/admin`, the system checks if the user has `ROLE_ADMIN`.
5. **Access Granted or Denied**
    - If authorized → The request proceeds.
    - If unauthorized → A `403 Forbidden` response is returned.

---
# Summary

- **Authentication:** Proves _who_ you are (login & request validation).
- **Authorization:** Checks _what_ you can do (roles, permissions).
- Authentication happens _first_, then authorization decides if access is allowed.