---
tags:
  - FORMAT
  - SoftwareDesign
---
# Definitions

| Concept            | Description                                                                    |
| ------------------ | ------------------------------------------------------------------------------ |
| **Authentication** | Verifies **who** a user or system is. "Are you who you say you are?"           |
| **Authorization**  | Determines **what** an authenticated user is allowed to do. "Can you do this?" |

---

# Authentication – Basic Overview

**Purpose**: Confirm identity.

## Common Methods

| Method                     | Description                                                     |
| -------------------------- | --------------------------------------------------------------- |
| Username + Password        | Most common. Easily implemented but vulnerable if not secured.  |
| Multi-Factor Auth (MFA)    | Adds a second factor: e.g., SMS, email, authenticator app.      |
| OAuth 2.0 / OpenID Connect | Used in delegated auth (e.g., sign in with Google).             |
| Biometrics                 | Fingerprints, facial recognition, etc.                          |
| Certificates / Keys        | Used in machine-to-machine auth (e.g., mutual TLS).             |
| Tokens                     | Session tokens, JWTs, API keys – represent authenticated state. |

---

# Authorization – Basic Overview

**Purpose**: Define access and permissions.

## Common Models

| Model                                 | Description                                                        |
| ------------------------------------- | ------------------------------------------------------------------ |
| Role-Based Access Control (RBAC)      | Access based on assigned roles (e.g., admin, user).                |
| Attribute-Based Access Control (ABAC) | Access based on user attributes, environment, resource properties. |
| Access Control Lists (ACLs)           | Specific permissions attached to objects/resources.                |
| Policy-Based Access                   | Defined policies grant/deny access (e.g., AWS IAM policies).       |

---

# How They Work Together

**Authentication**: User logs in with credentials.
1. System verifies the identity and creates a session/token.
**Authorization**: Every action/request is checked against permissions assigned to that user or session.

---

# Examples

## Web App

* You log in → **Authentication**
* You try to access admin panel → System checks if your role allows it → **Authorization**

## API

* You send a request with a bearer token → API verifies token → **Authentication**
* API checks your scopes or roles in the token → **Authorization**

---

# Technologies and Standards

| Area           | Common Tools/Standards                                                   |
| -------------- | ------------------------------------------------------------------------ |
| Authentication | OAuth 2.0, OpenID Connect, SAML, LDAP, Kerberos                          |
| Authorization  | JWT scopes/claims, RBAC, ABAC, OAuth scopes, IAM systems (e.g., AWS IAM) |
| Protocols      | HTTPS, TLS (transport security), HTTP Basic, Digest, Bearer tokens       |
| Libraries      | Keycloak, Auth0, Okta, Firebase Auth, Spring Security, Passport.js       |

---

# Security Considerations

* Always hash passwords with strong algorithms (e.g., bcrypt, Argon2)
* Use HTTPS to protect credentials in transit
* Store and verify tokens securely
* Apply least privilege in authorization
* Rotate secrets, keys, and tokens periodically
* Monitor for abuse or unusual access patterns