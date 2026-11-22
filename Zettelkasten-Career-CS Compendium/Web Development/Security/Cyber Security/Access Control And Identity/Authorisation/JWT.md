---
tags:
  - Security
  - FORMAT
---
A JSON Web Token (JWT) is a compact, URL-safe token format used to represent claims between two parties (usually a client and a server). It’s often used for securely transmitting information between a server and a client in the context of authentication and authorization. JWTs are typically used in stateless authentication, where the server does not need to store session data because the token itself contains all necessary information.

JWTs are used for scenarios where you need to securely verify the identity of a user or system and grant access to protected resources without requiring repeated logins or server-side session storage.

# Structure of a JWT

A JWT consists of three parts: the Header, Payload, and Signature. These are separated by dots (.) and form a string like this: header.payload.signature.

 Header: The header typically consists of two parts:  
   Type of the token, which is JWT.  
   Signing algorithm used, such as HS256 (HMAC with SHA-256) or RS256 (RSA with SHA-256).

Example:  

```json
{  
  "alg": "HS256",  
  "typ": "JWT"  
}
```

   
 Payload: The payload contains the claims, which are statements about an entity (typically, the user) and additional data. Claims can be of three types:  
   Registered Claims: Standard claims like iss (issuer), exp (expiration), sub (subject), and iat (issued at).  
   Public Claims: Claims that can be defined by anyone (e.g., role, user_id).  
   Private Claims: Claims that are agreed upon by the parties involved (e.g., username or email).

Example:  
  
```json
{  
  "sub": "1234567890",  
  "name": "John Doe",  
  "iat": 1516239022  
}
```

 

Signature: The signature is used to verify that the sender of the JWT is who it says it is, and to ensure that the message wasn’t altered along the way. To create the signature, you take the encoded header and payload, and sign it using a secret key (for HMAC algorithms) or a private key (for RSA algorithms).  
Example (using HMAC SHA256):  
  

```
HMACSHA256(  
  base64UrlEncode(header) + "." + base64UrlEncode(payload),  
  secret)

```
 

The JWT token would look something like this:

```
xxxxx.yyyyy.zzzzz
```

Where:

```
 xxxxx is the base64url-encoded header.  
 yyyyy is the base64url-encoded payload.  
 zzzzz is the signature.
```

# Theory: Stateless Authentication with JWT

JWTs are often used in stateless authentication, meaning that the server does not need to store session information. The server relies on the JWT itself, which is passed along with each request (typically in the HTTP Authorization header) to authenticate the user and grant access.

In stateless authentication:

 The client stores the JWT (in localStorage, sessionStorage, or as a cookie).  
 The server validates the JWT by checking the signature and claims to verify that it hasn’t expired and that the client is authorized to access the resource.

## Benefits:

 Scalability: Because no session data is stored on the server, JWTs can scale better across distributed systems (e.g., multiple servers or [[Microservices Architecture|microservices]]).  
 Decentralization: Authentication can be handled independently by multiple services, as long as they share the same signing key or public key infrastructure.  
 Performance: JWTs allow quick validation without requiring database lookups or session storage, making requests faster.

## Drawbacks:

 Security Risks: If the secret key used to sign the token is compromised, attackers can create their own valid tokens. Additionally, if tokens are not properly expired or revocation mechanisms are absent, an attacker could reuse an old token.  
 Token Size: Since the payload may contain more information, the token size can grow, which can impact network bandwidth.  
 Token Expiration: JWTs are usually given an expiration time (exp), and if not properly handled, tokens could expire before the user intends or remain valid too long.

# Use Cases for JWTs

 User Authentication: After a user logs in, a server generates a JWT containing user claims (e.g., user ID, roles) and sends it to the client. The client stores the token and sends it with each subsequent request to authenticate and authorize access to protected resources.  
  Example:  
   A user logs in, and the server generates a JWT with claims like user_id, email, and role. The client uses the JWT to access protected routes on the server without needing to log in repeatedly.  
 API Authorization: JWTs are often used for authorizing access to APIs. The server can verify that the client has the appropriate permissions and access to the requested resource using the JWT.  
  Example:  
   A mobile app uses a JWT to make authorized API requests to fetch user data from a server. Each API call includes the JWT in the Authorization header to prove the app's identity.  
 Single Sign-On (SSO): JWTs are used in many SSO systems where users authenticate once and then can access various services without needing to log in multiple times.

---

JWTs provide an efficient, stateless method of authentication, but they must be handled securely to prevent issues like token leakage or misuse. Would you like more details on best practices for securing JWTs, or would you like to focus on common use cases and examples of JWT implementation in specific systems?

