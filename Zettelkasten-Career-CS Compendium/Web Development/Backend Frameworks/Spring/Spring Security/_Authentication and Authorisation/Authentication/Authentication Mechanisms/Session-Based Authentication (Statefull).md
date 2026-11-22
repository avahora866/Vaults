---
tags:
  - FORMAT
  - Spring
---
Session-Based Authentication is a **stateful** authentication method where the server maintains a record of the user's authentication state in a session. This allows users to log in once and remain authenticated across multiple requests without needing to re-enter credentials.

---

# How Session-Based Authentication Works

1. **User Logs In**
    - The user provides credentials (e.g., username and password).
    - The server verifies the credentials against the database.
    - If valid, the server creates a session and stores an `Authentication` object in the session.
2. **Session ID is Stored in a Cookie**
    - The server generates a unique **Session ID** and sends it to the client in a cookie.
    - Example response header:
        
        ```
        Set-Cookie: JSESSIONID=123456789abcdef; Path=/; HttpOnly
        ```
        
    - The `HttpOnly` flag prevents JavaScript from accessing the cookie, improving security.
3. **User Makes a Request**
    - On subsequent requests, the browser automatically includes the session cookie:
        
        ```
        Cookie: JSESSIONID=123456789abcdef
        ```
        
    - The server retrieves the `Authentication` object from the session and restores the user's identity.
4. **Authorization Checks**
    - The server checks if the user has permission to access the requested resource.
    - If authorized, the request proceeds. If not, the server returns `403 Forbidden`.
5. **Session Expiry or Logout**
    - The session expires after a certain period (e.g., 30 minutes of inactivity).
    - The user can also log out, which invalidates the session.
    - Example logout implementation:
        
        ```java
        @GetMapping("/logout")
        public String logout(HttpServletRequest request) {
            request.getSession().invalidate();
            return "redirect:/login";
        }
        ```
        

---

# How Spring Security Manages Sessions

Spring Security provides a built-in mechanism for handling session-based authentication. The key components are:

1. **Security Context**
    - The `SecurityContextHolder` stores the authenticated user's details.
    - By default, Spring Security stores this in the session.
2. **Session Management Strategies**
    - `IF_REQUIRED` (default): Creates a session **only if needed** (e.g., after login).
    - `ALWAYS`: Creates a session for every request.
    - `NEVER`: Spring Security won’t create a session, but it will use an existing one.
    - `STATELESS`: No session is used (common in REST APIs).
    - Configurable in `SecurityFilterChain`:
        
        ```java
        http.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED));
        ```
        
3. **Session Repository**
    - The session is stored in the server’s memory by default.
    - For distributed applications, session storage can be handled with **Redis**, **database-backed sessions**, or **Spring Session**.

---

# Pros and Cons of Session-Based Authentication

## ✅ Pros:

✔ **Simple to implement** in traditional web applications.  
✔ **No need for tokens** to be passed with every request.  
✔ **Secure against token theft** (since session IDs are stored in cookies).

## ❌ Cons:

✘ **Not scalable** for distributed applications (sessions are tied to a specific server).  
✘ **Requires session management**, which can consume memory.  
✘ **Vulnerable to session hijacking** if cookies are not secured properly.

---

# When to Use Session-Based Authentication

✔ Traditional **web applications** with a single backend.  
✔ Enterprise apps where **sessions are managed in a secure environment**.  
✔ When **server-side authentication state is preferred** over stateless authentication.

**When NOT to use it:**  
✘ **REST APIs and Microservices** (use JWT or OAuth 2.0 instead).  
✘ If your app requires **horizontal scaling** (use distributed session storage if necessary).

Would you like an example Spring Security configuration for session-based authentication?