---
tags:
  - "#TRUNK"
  - Security
  - JWT
  - OAuth2
---

# Security Theory Fundamentals

Before you can implement security correctly, you must understand *why* the mechanisms exist. Every security primitive — sessions, tokens, OAuth2 — was invented to solve a specific problem. If you learn only the "how", you will misconfigure it when requirements change. This note covers the three foundational pillars: Stateful vs. Stateless authentication, the anatomy of a JWT, and the OAuth2/OIDC framework.

---

# 1. Stateful vs. Stateless Authentication

The most critical architectural decision in web security is not which library to use — it is **where you store the user's identity between requests**. After a user provides credentials, the server must "remember" them for subsequent requests without demanding the password again. Two fundamentally different strategies exist.

## 1.1 Stateful Authentication (Session-Based)

In a traditional stateful system, the **server owns the state**. When the user logs in successfully, the server creates a `Session` object in its own memory or database and assigns it a unique ID (e.g., `JSESSIONID`). That ID is sent back to the client as a **Cookie**.

**The Flow:**

```
User logs in → Server creates Session in memory → Server sends Session ID cookie
→ Browser auto-attaches cookie on every request → Server looks up Session ID → Grants access
```

> **Analogy — The Coat Check:** You hand over your coat (credentials). The attendant gives you a numbered ticket (Session ID). The ticket has no intrinsic value; it is just a *reference*. To retrieve your coat, the attendant must physically look up the hanger in the back room (server memory). Without that back room, the ticket is worthless.

## 1.2 Stateless Authentication (Token-Based / JWT)

In a stateless system, the **token owns the state**. When the user logs in, the server generates a token containing the user's identity directly inside it, then discards all memory of the transaction. The token is sent to the client and must be **explicitly attached** to every subsequent request (typically in the `Authorization: Bearer <token>` header). The server validates it by checking the cryptographic signature — no database lookup required.

**The Flow:**

```
User logs in → Server generates JWT with claims → Server sends token (no memory kept)
→ Client attaches token manually on every request → Server validates signature → Grants access
```

> **Analogy — The Passport:** A passport carries your photo, name, and nationality printed directly on it. The border agent does not phone your home country to confirm your identity — they check the holographic seal (signature) to confirm the document is authentic. If the seal is valid, the data inside can be trusted.

## 1.3 Key Trade-offs

Both architectures are correct choices in different contexts. Understanding the trade-offs prevents accidental misuse.

| **Feature** | **Stateful (Session)** | **Stateless (Token / JWT)** |
|---|---|---|
| **Server Memory** | High — stores every active user session | Zero — server stores nothing |
| **Scalability** | Hard — requires sticky sessions or shared cache (e.g., Redis) | Easy — any server can validate the token independently |
| **Revocation** | Easy — delete the session record; the ticket becomes invalid immediately | Hard — the token is valid until it expires; requires a deny-list to revoke early |
| **Use Case** | Monoliths, server-rendered apps | Microservices, SPAs, mobile apps |

> ⚠️ **The Revocation Trap:** A common mistake is building a stateless JWT system and then adding a "token blacklist" database table to support instant revocation. This is architecturally self-defeating — you have added state back into a stateless system. If you need instant revocation (e.g., "log out all devices immediately"), a **Stateful** architecture is the correct choice. Use the right tool for the job.

---

# 2. The Anatomy of a JWT

A **JSON Web Token (JWT)** is the most common implementation of stateless authentication. It looks like random noise, but it has a rigid, three-part structure separated by dots:

```
Header.Payload.Signature
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VyQGV4YW1wbGUuY29tIn0.abc123xyz
```

## 2.1 Part 1: The Header

The header describes the token itself — it is metadata, not data. It is a JSON object Base64Url-encoded into the first segment.

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```

- `typ` — confirms the token type is JWT
- `alg` — the signing algorithm (`HS256` = HMAC with SHA-256, symmetric; `RS256` = RSA, asymmetric)

## 2.2 Part 2: The Payload (Claims)

The payload carries the actual data. In JWT terminology, each data field is called a **claim**.

Three categories of claims exist:

| **Type** | **Description** | **Examples** |
|---|---|---|
| **Registered Claims** | Standardised fields with defined meaning | `sub` (subject/user ID), `iss` (issuer), `exp` (expiry timestamp), `iat` (issued at) |
| **Public Claims** | Custom fields your application defines | `role: "ADMIN"`, `email: "user@example.com"` |
| **Private Claims** | Agreed-upon fields between two specific parties | Internal service-to-service identifiers |

> ⚠️ **The Crucial Detail — Encoding ≠ Encryption:** The header and payload are **Base64Url encoded**, not encrypted. Base64 is a reversible encoding scheme — anyone who intercepts the token can decode the payload and read every claim. It is like writing in English backwards; it looks strange but decodes trivially. **Never store secrets — passwords, SSNs, API keys — in a JWT payload.** Only store identifiers and permissions.

## 2.3 Part 3: The Signature

The signature is the security guarantee of the JWT. It is computed by combining the encoded header, the encoded payload, and a **secret key known only to the server**:

```
Signature = HMACSHA256(
  base64(Header) + "." + base64(Payload),
  SECRET_KEY
)
```

If an attacker intercepts a token and modifies the payload (e.g., changes `role: "USER"` to `role: "ADMIN"`), the signature they would need to present no longer matches what the server would compute. The server rejects the token.

**The chain of trust:**
```
Server issues token → Attacker intercepts → Attacker modifies payload
→ Server recomputes signature → Signatures do not match → Request rejected
```

The secret key never leaves the server. The signature is tamper-evident by construction.

---

# 3. The OAuth2 Framework & OpenID Connect (OIDC)

## 3.1 What OAuth2 Actually Is

OAuth2 is frequently misunderstood as a "login system." It is not. OAuth2 is a framework for **Delegated Authorization** — it solves the problem of granting a third party *limited access* to your resources without handing over your credentials.

> **Analogy — The Valet Key:** You arrive at a hotel in your luxury car. You want the valet to park it, but you do not want them accessing your glove box or boot. You hand them a special **valet key** — it can start the engine and drive the car, but it cannot open the trunk. You never surrendered your master key (your password). You issued a **scoped, limited-access token**.

## 3.2 The Four Roles

Every OAuth2 flow involves exactly four actors. Memorise these — they map directly to configuration in Spring Security.

| **Role** | **Definition** | **Example** |
|---|---|---|
| **Resource Owner** | The user. The entity who grants access to their data. | You, the Google account holder |
| **Client** | The application requesting access on behalf of the user. | Your To-Do List app |
| **Authorization Server** | Verifies the user's identity and issues tokens. | Google Accounts (`accounts.google.com`) |
| **Resource Server** | Hosts the protected data. Validates tokens. | Google Calendar API |

> **Worked Example — "Log in with Google" for a To-Do app:**
> - You (Resource Owner) grant your To-Do app (Client) permission to read your Google Calendar (Resource Server) via Google Accounts (Authorization Server).
> - The To-Do app never sees your Google password. It receives a scoped token instead.

## 3.3 The Authorization Code Flow

This is the gold-standard OAuth2 flow for server-side applications. It uses a back-channel exchange to prevent the access token from ever appearing in a browser URL.

```
1. User clicks "Login with Google"
2. Your App → redirects browser to Google's Authorization Server (with client_id + scopes)
3. User authenticates on Google's domain (your app never sees the password)
4. Google → redirects browser back to your app with a short-lived authorization CODE
5. Your App → sends CODE to Google server-to-server (back-channel)
6. Google → returns Access Token + ID Token
```

> ⚠️ **Why the two-step?** The authorization code is short-lived and single-use. If an attacker intercepts the redirect (step 4), the code is useless without your app's `client_secret` to exchange it. The actual access token travels only on a server-to-server connection, never through the browser.

## 3.4 OAuth2 vs. OpenID Connect (OIDC)

OAuth2 was designed for authorization (granting access to resources). It was *not* designed for authentication (verifying identity). Developers tried to "hack" OAuth2 for login, which led to inconsistent, insecure implementations.

**OpenID Connect (OIDC)** is a thin identity layer built on top of OAuth2. It standardises the authentication use case by adding a second token.

| **Token** | **Protocol** | **Purpose** | **Contains** |
|---|---|---|---|
| **Access Token** | OAuth2 | Authorization — "what this bearer may do" | Scopes, permissions |
| **ID Token** | OIDC | Authentication — "who this user is" | `sub`, `name`, `email`, `picture` |

> **The practical rule:** When you want to display `"Welcome, John"` in your app, read the **ID Token**. When you want to call the Google Calendar API on the user's behalf, send the **Access Token**.

---

# 4. Quick Reference

## Token Architecture Decision Tree

```
Do you need instant revocation (e.g., "suspend this user now")?
  └─ YES → Use Stateful (Session-based) authentication
  └─ NO  → Are you building a distributed / microservice system?
              └─ YES → Use Stateless (JWT) authentication
              └─ NO  → Either approach works; prefer Session for simplicity
```

## Security Rules Summary

| **Rule** | **Reason** |
|---|---|
| Never store secrets in JWT payload | Payload is Base64 encoded, not encrypted — anyone can decode it |
| Always use HTTPS | JWTs sent over HTTP can be intercepted; the signature prevents modification but not interception |
| Keep JWT expiry short (15–60 min) | Stolen tokens are valid until expiry; short windows limit damage |
| Use refresh tokens for long sessions | Short-lived access tokens + longer-lived refresh tokens give both security and usability |
| BCrypt-hash passwords, never encrypt them | Encryption is reversible; hashing is one-way. A stolen DB with encrypted passwords can be decrypted if the key is also stolen |
