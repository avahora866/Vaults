---
tags:
  - Spring
---
**Basic Authentication** is a simple and widely used HTTP authentication mechanism. With Basic Authentication, a client sends the **username and password in the HTTP request header in the form of a base64-encoded string**.

# How Basic Authentication Works:

1. **Client Sends Request**: When a user tries to access a protected resource, the client (e.g., browser or HTTP client) sends a request to the server without authentication details.
2. **Server Responds with 401 Unauthorized**: The server responds with a `401 Unauthorized` status code, indicating that authentication is required.
3. **Client Sends Credentials**: The client responds by sending the credentials in the form of an HTTP `Authorization` header. The username and password are concatenated with a colon (`username:password`) and then base64 encoded. The header looks like this:
    
    ```
    Authorization: Basic base64(username:password)
    ```
    
4. **Server Verifies Credentials**: The server decodes the base64 string and compares the username and password with stored credentials (e.g., from an in-memory store, database, or LDAP). If the credentials are valid, the server allows access to the requested resource.
    

## Example of Basic Authentication Header:

```
Authorization: Basic YWRtaW46cGFzc3dvcmQ=
```

This is the base64 encoding of `admin:password`.

---

# Basic Authentication Flow in Spring Security

## Spring Security Basic Authentication Setup

- **SecurityFilterChain Configuration:**
    - In Spring Security, Basic Authentication can be configured to require the `Authorization` header for protected resources.
    - The login endpoint (e.g., `/login`) should be publicly accessible (`permitAll()`) so the client can initially send the `username` and `password` to authenticate.
    - Once the client is authenticated, it will use Basic Authentication (`Authorization: Basic <encoded_credentials>`) for subsequent requests.

### Example Security Config for Basic Authentication:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            .antMatchers(HttpMethod.POST, "/login").permitAll()  // Allow unauthenticated access to login
            .anyRequest().authenticated()  // Protect all other requests
        .and()
        .httpBasic();  // Enable HTTP Basic Authentication

    return http.build();
}
```

## Login Flow:

- **Initial Authentication Request (POST `/login`)**:
    - The client sends their credentials (`username` and `password`) as part of the request body (usually in JSON format).
    - The login endpoint should be publicly accessible using `permitAll()` because the client is unauthenticated when they first submit their credentials.

- **Authentication Validation**:
    - Spring Security validates the credentials and, if valid, an `Authentication` token is created and stored in the `SecurityContextHolder`.

- **Subsequent Requests**:
    - After the first successful authentication, the client includes the **Authorization header** (`Authorization: Basic <encoded_credentials>`) with each subsequent request.
    - Spring Security automatically checks the `Authorization` header, validates the credentials, and grants access to protected resources.

## Spring Security’s Role in Basic Authentication:

- **UsernamePasswordAuthenticationToken**: Used to store the authenticated user’s details.
- **AuthenticationManager**: Handles the authentication logic. It checks the credentials provided and returns an `Authentication` object if the credentials are valid.
- **SecurityContextHolder**: Stores the `Authentication` object, which holds the user’s authentication details, so that Spring Security can authorize the user for subsequent requests.

## The `/login` Endpoint:

- Since Basic Authentication involves sending credentials in the `Authorization` header after the login, the `/login` endpoint must be **accessible without authentication** (hence, `permitAll()` is used).
- This endpoint is used to authenticate the client, and once authenticated, the `Authorization` header is used for all future requests.

## Why `/login` Needs `permitAll()`:

- The `/login` endpoint is used to initially authenticate the user by checking their credentials.
- If it's not publicly accessible, the user wouldn't be able to send their credentials and authenticate.
- After the user is authenticated, the `Authorization` header is used in subsequent requests, so the login endpoint is no longer needed.

## Basic Authentication Details:

- After the login process, Spring Security handles the authorization automatically by storing the authentication details in the `SecurityContextHolder`.
- The client can then send the `Authorization: Basic <encoded_credentials>` header for all future requests to access protected resources.

## Example UserDetailsService & AuthenticationManager:

- You typically configure an in-memory user details service for testing or use a database-backed service for production.

```java
@Bean
public UserDetailsService userDetailsService() {
    UserDetails userDetails = User.withUsername("user")
            .password(passwordEncoder().encode("password"))
            .roles("USER")
            .build();

    return new InMemoryUserDetailsManager(userDetails);
}

@Bean
public AuthenticationManager authenticationManager(
        UserDetailsService userDetailsService,
        PasswordEncoder passwordEncoder) {
    DaoAuthenticationProvider authenticationProvider = new DaoAuthenticationProvider();
    authenticationProvider.setUserDetailsService(userDetailsService);
    authenticationProvider.setPasswordEncoder(passwordEncoder);

    return new ProviderManager(authenticationProvider);
}
```

---

## No Prior Login Request:

- **Before login (first-time request)**: If the client sends a request with the **Authorization** header, there’s **no session or previous login context**. Spring Security will **attempt to authenticate** using the credentials provided in the header.
- **Login happens implicitly**: Essentially, sending the **Authorization header** acts like the **login** step, where the credentials are verified and the user is considered authenticated for that request.

---

# Summary of Steps in Basic Authentication Flow:

1. **Client sends credentials (username/password)** to the server through a POST request (usually to `/login`).
2. The server **validates the credentials**.
3. If valid, an **authentication token** is created and stored in the `SecurityContextHolder`.
4. For subsequent requests, the client **sends the `Authorization` header** with the encoded credentials.
5. Spring Security **automatically validates** the credentials from the `Authorization` header and grants access to protected resources.

---

# Enabling Basic Authentication in Spring Security

You can enable Basic Authentication in Spring Security by simply configuring HTTP security. Here's an example of how to set it up:

## Java Configuration Example for Basic Authentication:

```java
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll() // Allow access to public pages
                .anyRequest().authenticated() // Protect other pages
            .and()
            .httpBasic(); // Enable Basic Authentication
    }
}
```

This configuration will enable Basic Authentication for your application. Spring Security will automatically handle the `Authorization` header and manage the authentication flow.

---

# Security Considerations:

- Since Basic Authentication sends the credentials in base64 (not encrypted), it is **not secure over unencrypted connections**. It must always be used with **HTTPS** to protect credentials from being intercepted during transmission.
    
- It's recommended to use Basic Authentication in conjunction with other security features like **CSRF protection** and **session management**.
    

---

# Basic Authentication vs Digest Authentication

**Basic Authentication** and **Digest Authentication** are HTTP-based authentication methods that provide simple ways to authenticate users by sending credentials (username and password) via HTTP headers. These are lightweight authentication mechanisms, suitable for applications that don't require complex user interfaces or additional security measures like tokens or session management.

- **Basic Authentication** sends credentials in plaintext (albeit base64 encoded), while **Digest Authentication** provides a more secure alternative by hashing the credentials.