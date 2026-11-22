---
tags:
  - "#TRUNK"
  - HTTP
  - Networking
  - WebDev
---

# HTTP Protocol — Intermediate to Mastery

HTTP (Hypertext Transfer Protocol) is the stateless, text-based application-layer protocol that powers all communication on the modern web. Understanding it deeply — not just how to use it, but *why* it behaves as it does — is foundational for software engineering, API design, and cybersecurity. This note covers the full protocol: its architecture, methods, status codes, headers, state management, and caching, grounded in the RFC specifications that define it.

---

# Pre-Module: The Landscape

> *Before you can study a protocol, you need to know who wrote the rulebook — and who decided how it should be used.*

**RFC 7231** is a formal specification published by the **Internet Engineering Task Force (IETF)**. RFC stands for *Request for Comments* — the IETF's document format for defining internet standards. HTTP/1.1 was originally governed by a single enormous document, **RFC 2616**, but it was so complex it was split into six focused documents (RFC 7230–7235) in 2014. RFC 7231 is specifically titled *"HTTP/1.1: Semantics and Content"* — it defines what every method and status code is *supposed to mean*, giving both clients and servers a shared, unambiguous contract.

**HTTP vs. REST** is one of the most frequently confused distinctions in software engineering.

- **HTTP (Hypertext Transfer Protocol)** is the *transport mechanism* — think of it as the English language. It has grammar (headers), vocabulary (methods), and rules of syntax.
- **REST (Representational State Transfer)** is an *architectural style* — think of it as "Professional Business Writing." It was proposed by Roy Fielding (one of HTTP's own authors) in his doctoral dissertation.

The relationship: you can write English without following business writing conventions. You *could* design a system where `GET /delete-user?id=5` deletes a user — HTTP will carry that message happily. But it violates REST principles, which dictate that `GET` should never modify server state. REST is essentially a set of best practices that encourages you to use HTTP *exactly as RFC 7231 intended*.

---

# Module 1: The Architecture of a Conversation

The first thing to unlearn is the idea that HTTP is a mysterious data stream. At its core, HTTP is a **text-based protocol** — if you intercepted the wire, you would see readable characters, not binary gibberish (unlike TCP or QUIC, which are binary). Every interaction follows a strict **Request → Response** cycle, and both messages share a nearly identical four-part structure.

## The Anatomy of an HTTP Request

```http
// 1. The Start Line — what we want and how we're asking
GET /articles/science.html HTTP/1.1

// 2. The Headers — metadata about the request
Host: www.university.edu
User-Agent: Mozilla/5.0
Accept: text/html

// 3. The Empty Line — signals "headers are done"
[CRLF]

// 4. The Body — empty for GET; contains data for POST/PUT
```

The **Start Line** (also called the *Request Line*) has three components:
- **Method** — the action (`GET`)
- **Target URI** — the resource (`/articles/science.html`)
- **HTTP Version** — the dialect (`HTTP/1.1`)

> ⚠️ **The Crucial Detail:** The empty line between headers and body is not optional. Per RFC 7230, it must be a CRLF (Carriage Return + Line Feed). Without it, the server treats the body as another malformed header and returns a `400 Bad Request`.

## The Anatomy of an HTTP Response

```http
// 1. The Status Line — the result
HTTP/1.1 200 OK

// 2. Headers — metadata about the response
Date: Fri, 09 Jan 2026 12:00:00 GMT
Content-Type: text/html
Content-Length: 124

// 3. Empty Line

// 4. Body — the actual content
<html><body>Welcome to the Science Department</body></html>
```

## The Rule of Statelessness

HTTP is **stateless**: the server retains zero memory of previous requests. Send a second request immediately after the first, and the server processes it as if it has never encountered you before. This is a *deliberate architectural feature*, not a limitation — it makes servers massively scalable because they don't need to maintain session state for every connected client. The tradeoff is that anything requiring "memory" (logins, shopping carts) must be explicitly re-attached to each request, typically via a cookie or token in the headers.

---

# Module 2: Methods & Semantics

Knowing `GET` and `POST` is table stakes. What distinguishes an intermediate from an expert is understanding the two academic properties RFC 7231 assigns to every method: **Safety** and **Idempotency**.

**Safety** — A method is *safe* if it implies a read-only operation: it should never alter server state.
> *Analogy: Reading a restaurant menu. No matter how many times you read it, the kitchen doesn't cook any food.*

**Idempotency** — A method is *idempotent* if making the same request N times produces the same outcome as making it once.
> *Analogy: The "Call Elevator" button. Pressing it 50 times in frustration does not summon 50 elevators. The end state — elevator is coming — is identical to pressing it once.*

| **Method** | **Use Case** | **Safe?** | **Idempotent?** |
|---|---|---|---|
| `GET` | Retrieve a resource | ✅ Yes | ✅ Yes |
| `POST` | Create a resource or trigger a process | ❌ No | ❌ No |
| `PUT` | Replace a resource completely | ❌ No | ✅ Yes |
| `PATCH` | Partially update a resource | ❌ No | ❌ No (usually) |
| `DELETE` | Remove a resource | ❌ No | ✅ Yes |

## The Critical Distinction: POST vs. PUT

This is the most common interview question and architectural fault line in API design.

- **POST is non-idempotent.** Each call creates a *new* resource or triggers a *new* transaction. If a network lag causes your client to retry `POST /payments`, the server will process two separate $10 withdrawals. This is why payment screens warn: *"Do not click Back or Refresh."*
- **PUT is idempotent.** It acts as a "replace" command. Calling `PUT /users/5` ten times with the same payload overwrites the record ten times — the final state is identical to the first call.

> ⚠️ **The Danger Zone:** Never use `POST` for operations that will be automatically retried (by load balancers, scripts, or mobile clients) without a unique **idempotency key** to deduplicate requests server-side. The consequences in financial systems are severe.

---

# Module 3: The Language of Status Codes

When a server replies, it leads with a three-digit integer that communicates the *class* of outcome before the client reads a single byte of body content. The first digit is the only thing you need to orient yourself.

| **Class** | **Meaning** | **Plain English** |
|---|---|---|
| `1xx` | Informational | "Hold on, I'm working on it." |
| `2xx` | Success | "Understood, and it worked." |
| `3xx` | Redirection | "The resource isn't here — go somewhere else." |
| `4xx` | Client Error | "You made a mistake." |
| `5xx` | Server Error | "I made a mistake." |

The specific codes within each class are where real expertise lives.

## Success: 200 vs. 201 vs. 204

All three mean "it worked" — but they carry different semantic payloads:
- **`200 OK`** — Standard success. Here is your data.
- **`201 Created`** — Used specifically after a `POST` or `PUT` that produced a new resource. The response typically includes a `Location` header pointing to the newly created item.
- **`204 No Content`** — Success, but there is nothing to return. Common for `DELETE` requests or updates where no visual feedback is needed.

> **The Crucial Detail:** Using `200` when you should use `201` is technically correct but semantically lazy. A well-designed API uses `201` to signal to the client that a *new resource now exists*, enabling it to update its local state accordingly.

## Redirection: 301 vs. 302

These look similar but have very different long-term consequences:
- **`301 Moved Permanently`** — The old URL is dead. Browsers cache this *indefinitely*. Search engines update their index to the new URL. Use this when a page has truly moved forever.
- **`302 Found` (Temporary Redirect)** — The resource is temporarily elsewhere. Keep using the original URL for future requests. Use this for maintenance windows or A/B testing.

> ⚠️ **Warning:** Accidentally returning a `301` when you meant `302` can haunt you — once a browser caches a permanent redirect, it will never ask the server again, even if you remove it. Users will be silently redirected until they manually clear their cache.

## Client Errors: 401 vs. 403

This distinction is a hallmark of a competent API designer:
- **`401 Unauthorized`** — An *authentication* failure. The server is asking: *"Who are you?"* The user has not logged in, or their credentials are invalid.
- **`403 Forbidden`** — An *authorization* failure. The server knows exactly who you are, but says: *"You are not allowed to do that."* A junior employee trying to delete a CEO's confidential file gets a `403`.

> *Analogy: `401` is being stopped at the front door because you have no ID. `403` is being stopped because your ID is valid, but your clearance level isn't high enough.*

## Server Errors: 500 vs. 503

- **`500 Internal Server Error`** — A generic crash. The application code broke. The server is running, but something went wrong inside it.
- **`503 Service Unavailable`** — The server is functional but cannot handle the request right now — it is overloaded or under maintenance. It implicitly means: *"Try again later."* A well-behaved `503` response includes a `Retry-After` header.

---

# Module 4: Metadata & Content Negotiation

The method tells the server *what to do*. The status code tells the client *what happened*. Headers tell both parties *how to communicate* — before any real data is exchanged. This negotiation process is called **Content Negotiation**.

## The Two Most Important Headers

If you memorize only two headers, make it these:

- **`Content-Type`** — *"Here is what I am sending you."* Used by both the client (when sending a `POST`/`PUT` body) and the server (when replying). Think of it as the label on a sealed package. If you label it `application/json` but the contents are HTML, the system breaks.
- **`Accept`** — *"Here is what I want back."* Used strictly by the *client*. A browser might send `Accept: text/html` to request a rendered page; an API client might send `Accept: application/json` to request raw data.

The values used in these headers are called **MIME types** (Multipurpose Internet Mail Extensions) — a standardized two-part format: `type/subtype`.

| **MIME Type** | **Used For** |
|---|---|
| `application/json` | Modern REST APIs |
| `text/html` | Web pages |
| `image/png` | PNG images |
| `application/x-www-form-urlencoded` | Standard HTML form submissions |

## Negotiation in Action

```http
// 1. Client requests JSON explicitly
GET /api/users/5 HTTP/1.1
Accept: application/json

// 2. Server confirms what it's returning
HTTP/1.1 200 OK
Content-Type: application/json

// 3. The body — in the agreed format
{"id": 5, "name": "Alice"}
```

If the client sends `Accept: application/xml` but the server only speaks JSON, the server should return **`406 Not Acceptable`**.

> ⚠️ **The Danger Zone:** The most common mistake when debugging API integrations is forgetting `Content-Type` on the *request* body. A strict server receiving a `POST` with JSON data but no `Content-Type: application/json` header will reject it with **`415 Unsupported Media Type`** — it sees a stream of bytes and refuses to guess what format they are in, for security reasons.

---

# Module 5: State in a Stateless World

HTTP is stateless — the server forgets you the moment it sends a response. Yet Gmail, your bank, and every modern application obviously "remember" you are logged in. The solution is that we *simulate* state by attaching identity information to every request, using one of two mechanisms: **Cookies** or **Tokens**.

## The Cookie Mechanism

Cookies are the classic approach, suited to traditional browser-based applications.

1. **The Set-Up:** You submit your credentials. The server validates them.
2. **The Tag:** The server issues a session identifier in a response header:
   ```http
   Set-Cookie: session_id=abc12345; Secure; HttpOnly
   ```
3. **The Memory:** The browser saves this cookie automatically.
4. **The Echo:** On *every subsequent request* to that domain, the browser automatically attaches:
   ```http
   Cookie: session_id=abc12345
   ```

The server looks up `abc12345` in its session store, recognises the user, and responds accordingly — no explicit identity ever sent by the user again.

**Key Cookie Security Attributes:**

| **Attribute** | **Effect** |
|---|---|
| `Secure` | Cookie is only transmitted over HTTPS — never plain HTTP |
| `HttpOnly` | JavaScript cannot read this cookie via `document.cookie` |
| `SameSite` | Controls whether the cookie is sent on cross-site requests (CSRF protection) |

> ⚠️ **The Danger Zone — Session Hijacking:** If the `HttpOnly` flag is absent, a successful **XSS (Cross-Site Scripting)** attack allows a malicious script to read `document.cookie` and steal the session ID. The attacker can then inject that ID into their own browser and *become* the user — without ever knowing the username or password. This is called **Session Hijacking**, and it is entirely prevented by a single attribute.

## The Token Mechanism (Modern REST / SPAs)

Modern mobile apps and Single Page Applications (SPAs) typically avoid cookies in favour of **Bearer Tokens** — most commonly **JWTs (JSON Web Tokens)**.

1. **The Login:** You `POST` your credentials.
2. **The Token:** The server replies with a signed token in the *response body* (not a `Set-Cookie` header):
   ```json
   { "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
   ```
3. **The Manual Header:** The client stores this token (in memory or local storage) and manually attaches it to every subsequent request:
   ```http
   Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
   ```

**Cookies vs. Tokens — The Core Difference:**

| **Feature** | **Cookies** | **Bearer Tokens** |
|---|---|---|
| **Management** | Automatic — handled by the browser | Manual — handled by your code |
| **Transport** | `Cookie` header (automatic) | `Authorization` header (explicit) |
| **Best For** | Traditional web apps | APIs, mobile apps, SPAs |
| **CSRF Risk** | Yes (mitigated by `SameSite`) | No (not sent automatically) |
| **XSS Risk** | Mitigated by `HttpOnly` | Higher if stored in `localStorage` |

---

# Module 6: Performance & Caching

> *The fastest HTTP request is the one you never send.*

HTTP has a sophisticated caching layer designed to reduce **latency** (wait time) and **bandwidth** (data transferred). For high-traffic applications, a well-tuned caching strategy is the single highest-leverage performance optimisation available.

## The Instruction: Cache-Control

The `Cache-Control` header tells the browser — and any intermediate proxies like CDNs — what caching rules to follow.

- **`Cache-Control: no-store`** — Do not cache anything under any circumstances. Used for sensitive data like banking transactions.
- **`Cache-Control: no-cache`** — You may store a copy, but you must *revalidate* with the server before using it (triggers a conditional GET, see below).
- **`Cache-Control: max-age=3600`** — Cache this resource for 3600 seconds (1 hour). Do not contact the server again until that window expires.
- **`Cache-Control: public`** — This response may be stored by shared caches (CDNs). Safe for static assets.
- **`Cache-Control: private`** — This response is for a single user only. CDNs must not cache it.

## The Validation: ETags and the Conditional GET

What happens when `max-age` expires? The naive approach is to re-download the file. But what if the file hasn't changed? That download is pure waste. The **Conditional GET** pattern solves this elegantly.

```
// Round 1 — First ever request
Browser  →  GET /logo.png HTTP/1.1
Server   →  200 OK
            ETag: "v1-xyz"
            [500KB of image data]

// Browser stores the image AND the ETag fingerprint.

// Round 2 — Cache expired, browser checks before downloading
Browser  →  GET /logo.png HTTP/1.1
            If-None-Match: "v1-xyz"

// Server compares the submitted tag against the current file's tag.
```

**The Server's Decision:**

- **File unchanged** → Tags match → Server replies `304 Not Modified` with *no body*. The browser loads the cached copy from disk. Zero bandwidth consumed.
- **File changed** → Tags differ → Server replies `200 OK` with the new file and a new `ETag`.

> **The Crucial Detail:** When a browser receives `304 Not Modified`, it does *not* receive the file from the server. The file it renders comes entirely from its own local disk cache. The server's only contribution was a tiny header-only response confirming the cached copy is still valid.

## The Full Caching Flow

```
First visit:
  Client ──GET /style.css──────────────────→ Server
  Client ←──200 OK + ETag:"abc" + 500KB──── Server
  [Client stores file + ETag, starts max-age timer]

Next visit (within max-age):
  Client uses disk cache directly.
  No network request made at all.

Next visit (after max-age expires):
  Client ──GET /style.css──────────────────→ Server
         ──If-None-Match: "abc"────────────→

  IF unchanged:
  Client ←──304 Not Modified (0KB)─────── Server
  [Client serves from disk]

  IF changed:
  Client ←──200 OK + ETag:"xyz" + 500KB── Server
  [Client stores new file + new ETag]
```
