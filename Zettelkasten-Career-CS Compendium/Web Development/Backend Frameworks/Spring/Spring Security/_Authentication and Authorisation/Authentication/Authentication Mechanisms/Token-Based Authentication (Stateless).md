---
tags:
  - FORMAT
  - Spring
---
Token-based authentication is a common approach for securing APIs and applications. Instead of storing session data on the server, the client receives a token that represents its authentication state.

# Key Components

* **Access Token**: A short-lived JSON Web Token (JWT) that is used to authenticate API requests. It typically contains user details, roles, and expiration time.  
* **Refresh Token**: A long-lived token used to obtain a new access token when the previous one expires. It ensures a seamless user experience without frequent logins.

---

 JSON Web Token (JWT)

JWT is a compact, self-contained token format that securely transmits information between parties. It consists of three parts:

1. **Header**: Contains metadata about the token and the algorithm used (e.g., HS256, RS256).  
2. **Payload**: Contains claims (data) about the user, including username, roles, and expiration.  
3. **Signature**: A cryptographic signature using a secret key or private key to ensure the token’s integrity.

**Example JWT (Decoded)**:

```
{
  "sub": "user123",
  "roles": ["ROLE_USER"],
  "exp": 1710832020,
  "iss": "myapp"
}
```

# How JWTs Are Secured

* **Signed, Not Encrypted**: JWTs are typically signed using a secret key, ensuring they haven’t been tampered with.  
* **Stateless**: Since JWTs contain all necessary information, Spring Security doesn't need to store session data on the server.  
* **Short Lifespan**: Tokens are given a short expiration time to minimize the impact of a compromised token.

---

 Authentication Flow Using JWT

1. **User Login**  
   * The user submits their username and password to a login endpoint.  
   * Spring Security validates the credentials using an `AuthenticationManager` and a `UserDetailsService`.  
2. **Generate Tokens**  
   * Upon successful authentication:  
     * An **Access Token** (JWT) is created with user details and roles.  
     * A **Refresh Token** is also issued with a longer expiration time.  
   * Both tokens are returned to the client.  
3. **Access Token Usage**  
   * The client includes the access token in the `Authorization` header using the `Bearer` scheme:

```
Authorization: Bearer <access_token>
```

4. **Token Validation**  
   * Spring Security intercepts incoming requests using a `JwtAuthenticationFilter`.  
   * The filter extracts and verifies the token using a secret key.  
   * If valid, Spring sets the user’s authentication context using `SecurityContextHolder`.  
5. **Access Token Expiration**  
   * When the access token expires, the client sends a request to the `/refresh` endpoint with the refresh token.  
6. **Token Refresh**  
   * The server verifies the refresh token and generates a new access token.  
   * The client receives the new token and continues making requests.

---

# Basic Authentication vs Token Authentication

## Basic Authentication

* In **Basic Authentication**, the username and password are encoded in Base64 and sent with every request using the `Authorization: Basic <base64(username:password)>` header.  
* **No Persistent Session**: Once the browser tab is closed, the authentication state is lost. When the user reopens the site, they will typically need to log in again.  
* Some browsers may cache the credentials temporarily, but this is generally unreliable and insecure.

✅ **Needs login again** after closing the tab.

---

## Token-Based Authentication (JWT)

With **Token-Based Authentication** (using JWTs):

* **Access Tokens**: After logging in, the client receives a JWT access token (typically stored in memory or browser storage).  
* **Refresh Tokens**: If refresh tokens are implemented and stored securely (e.g., in HTTP-only cookies), the user can remain authenticated even after closing and reopening the tab.

## Depending on Storage Mechanism:

1. **If the Access Token is in Memory (e.g., Redux Store, Context API)**

   * Once the tab is closed, the access token is lost.  
   * The user will need to log in again unless there is a refresh token stored securely in a cookie.  
2. **If the Access Token is in LocalStorage or SessionStorage**

   * **SessionStorage**: The token will be cleared when the tab or browser is closed. The user will need to log in again.  
   * **LocalStorage**: Tokens persist even after the tab is closed, meaning the user can return without logging in, as long as the token is still valid. However, storing tokens in LocalStorage can be a security risk (prone to XSS attacks).  
3. **If Using Refresh Tokens**

   * The refresh token is often stored as an **HTTP-only cookie**.  
   * When the user reopens the browser, the application can silently send a request using the refresh token to obtain a new access token.  
   * This means the user will remain logged in without needing to re-enter their credentials.

✅ **No login required** if a valid refresh token exists.  
 ❗ **Login required** if both tokens are expired or invalid.

 Access Token vs. Refresh Token

| Aspect | Access Token | Refresh Token |
| ----- | ----- | ----- |
| **Purpose** | Authenticate API requests | Obtain a new access token |
| **Lifespan** | Short (e.g., 15 min \- 1 hr) | Longer (e.g., days or weeks) |
| **Security Risk** | Higher if leaked (since it grants access) | Lower risk (doesn't access resources directly) |
| **Storage Location** | In-memory or secure storage (e.g., browser storage) | HTTP-only cookie (prevents XSS attacks) |
| **Validation** | Self-contained JWT validation | Verified using a secret key and possible database check |
| **Revocation** | Cannot be revoked without token blacklist | Can be revoked or invalidated |

---

 Best Practices for Token-Based Authentication

1. **Use HTTPS**: Always ensure tokens are transmitted over secure connections to prevent interception.  
2. **Short Expiration for Access Tokens**: Keep JWT lifetimes short to reduce risk.  
3. **Store Refresh Tokens Securely**: Use HTTP-only cookies to protect against XSS attacks.  
4. **Token Rotation**: Consider implementing **rotating refresh tokens** to prevent reuse of stolen tokens.  
5. **Blacklist Revoked Tokens**: Maintain a token blacklist in the database to revoke compromised refresh tokens.  
6. **Monitor and Log**: Track authentication events to detect anomalies and potential abuse.

---

 Conclusion

Spring Security’s token-based authentication using JWT and refresh tokens provides a scalable, secure, and stateless way to manage authentication. Access tokens ensure fast authentication for API requests, while refresh tokens maintain user sessions without requiring frequent logins.

Would you like further guidance on implementing this in Spring Security, or do you have any questions on specific configurations?

