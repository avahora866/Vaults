---
tags:
  - FORMAT
  - Spring
---

**Digest Authentication** is a more secure version of Basic Authentication. Unlike Basic Authentication, which sends credentials in plaintext (base64 encoding is not encryption), Digest Authentication hashes the credentials and includes additional data to prevent replay attacks.

# How Digest Authentication Works:

1. **Client Sends Request**: The client sends a request to a protected resource without credentials.

2. **Server Responds with 401 Unauthorized**: The server responds with a `401 Unauthorized` status code and provides a **WWW-Authenticate** header. This header includes a **nonce** (a one-time value), a **realm** (which identifies the server), and other data required for the digest process.

```
WWW-Authenticate: Digest realm="Example", qop="auth", nonce="dcd98b7102dd2f0e8b7e3d2d4e9a8e3c", opaque="5ccc069c403ebaf9f0171e9517f40e41"
```

3.   
   **Client Creates Hash**: The client hashes the following data using a cryptographic hash function (typically MD5):

   * **Username** (from the request)  
   * **Password** (stored securely)  
   * **Nonce** (from the server's `WWW-Authenticate` header)  
   * **URI** (the requested resource)  
   * **Other data**, such as the **HTTP method** (GET, POST, etc.). The client sends the hash in the **Authorization** header along with the nonce and other data.  
4. Example Authorization Header for Digest Authentication:

```
Authorization: Digest username="admin", realm="Example", nonce="dcd98b7102dd2f0e8b7e3d2d4e9a8e3c", uri="/protected", qop="auth", nc=00000001, cnonce="0a4f113b", response="e02e4d1c4fe383e329586380e28be29f"
```

5.   
   **Server Verifies the Hash**: The server performs the same hashing process with the stored password, nonce, and other data, and compares it with the hash provided by the client. If the hashes match, the server grants access.

---

 Configuring Digest Authentication in Spring Security

Digest Authentication is not directly supported in the same way as Basic Authentication, but you can implement custom filters or use third-party libraries to enable Digest Authentication. The process involves extending Spring Security's authentication mechanism and handling the hashing and nonce verification manually.

## Example of Digest Authentication Custom Filter:

To enable Digest Authentication, you would typically need to create a custom filter and integrate it with Spring Security's filter chain. Here's a simplified version of what that might look like:

```java
public class DigestAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
    public DigestAuthenticationFilter(String defaultFilterProcessesUrl) {
        super(defaultFilterProcessesUrl);
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException, IOException, ServletException {
        // Extract the Digest authentication information from the request header
        String authHeader = request.getHeader("Authorization");

        // Parse the authHeader and compute the hash based on the Digest algorithm
        // Compare the computed hash with the user credentials (using a custom authentication provider)

        // Return an Authentication object if valid, or throw an AuthenticationException if not
    }
}
```

# Custom Authentication Provider

You'd also need to implement a custom `AuthenticationProvider` to handle the process of hashing the credentials and verifying the request:

```java
public class DigestAuthenticationProvider implements AuthenticationProvider {
    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        // Extract and hash the credentials
        // Compare the hash with the stored password or hashed credentials
        // Return an authenticated token if the hash matches
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return DigestAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

---

 Security Considerations

  * Digest Authentication is more secure than Basic Authentication because it hashes the password, reducing the risk of interception. However, it still requires HTTPS to protect against man-in-the-middle attacks.  
  * Digest Authentication is not as widely used as Basic Authentication due to its complexity and the availability of more modern authentication protocols like OAuth 2.0 and JWT.

---

 Conclusion

**Basic Authentication** and **Digest Authentication** are simple methods to authenticate users using HTTP headers. Basic Authentication is easier to implement but less secure, while Digest Authentication offers better security by hashing the credentials but is more complex.

In Spring Security:

* **Basic Authentication** is easy to enable with just a few lines of configuration.  
* **Digest Authentication** requires a custom solution, as Spring Security doesn't directly support it out of the box.

Both methods are suitable for applications where the simplicity of HTTP-based authentication is sufficient, but for more complex or highly secure environments, more advanced authentication mechanisms like **OAuth 2.0**, **JWT**, or **SAML** may be more appropriate.