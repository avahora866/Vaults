---
tags:
  - FORMAT
  - Spring
---
# `SecurityMockMvcRequestPostProcessors` for Custom Security Contexts

In Spring Security testing, **`SecurityMockMvcRequestPostProcessors`** provide a way to modify the security context during tests. These post-processors can be used to customize how requests are processed by `MockMvc` in your tests, specifically for security-related features like authentication and authorization.

## What are `SecurityMockMvcRequestPostProcessors`?

`SecurityMockMvcRequestPostProcessors` are utility methods that can be used with `MockMvc` to simulate different authentication scenarios, set custom security contexts, and perform actions like logging in with a specific user or adding roles and authorities. These post-processors are part of the `spring-security-test` module, which simplifies testing security-related functionality without needing to set up a full authentication flow manually.

## Use Cases for `SecurityMockMvcRequestPostProcessors`

* **Customizing Security Context**: You can simulate requests with custom security contexts (e.g., a specific authenticated user with roles and authorities).

* **Simulating Logged-in Users**: You can test how your application handles requests for specific users without needing to actually log in.

* **Testing Authorization**: You can check if a user with certain roles or authorities can access particular resources.

* **Custom Authentication**: You can simulate custom authentication mechanisms like JWT tokens, basic authentication, or custom credentials.

## Key Post-Processors in `SecurityMockMvcRequestPostProcessors`

### `user()`: Simulate a User with Specific Roles and Authorities

The `user()` post-processor allows you to simulate a user being authenticated. You can set the username, password, roles, authorities, etc., for the user making the request.

#### Example:

```java
mockMvc.perform(get("/adminPage")
        .with(user("admin").password("adminpass").roles("ADMIN")))
        .andExpect(status().isOk());  // Should pass if the user has "ADMIN" role
```

*   
  **`user()`**: This method creates a simulated user. You can specify a username, password, and roles (which are mapped to authorities).

* **`roles()`**: You can pass in roles such as `"ADMIN"`, `"USER"`, etc. These roles are typically mapped to authorities, and Spring Security uses them to authorize access to protected resources.

### `authenticated()`: Simulate an Authenticated User (Without Specifying Details)

The `authenticated()` post-processor is used when you want to simulate an authenticated user but without needing to specify detailed user information like roles or username.

#### Example:

```java
mockMvc.perform(get("/userPage")
        .with(authenticated()))  // Any authenticated user
        .andExpect(status().isOk());
```

*   
  **`authenticated()`**: This is a simpler version of `user()`, which indicates the request should be processed as if the user is authenticated but does not specify the username, roles, or password.

### `with(csrf())`: Simulate CSRF Token

Cross-Site Request Forgery (CSRF) is a common security mechanism to prevent attacks. The `csrf()` post-processor adds a CSRF token to the request, which is necessary for making POST requests in Spring Security-enabled applications (if CSRF protection is enabled).

#### Example:

```java
mockMvc.perform(post("/submitForm")
        .with(csrf())  // Include CSRF token for protected POST request
        .param("param", "value"))
        .andExpect(status().isOk());
```

*   
  **`csrf()`**: This adds a CSRF token to the request, which is required for POST requests when CSRF protection is enabled. You can use it in scenarios where your application has CSRF protection and you need to simulate an authenticated request.

### `bearerToken()`: Simulate Bearer Token Authentication

For testing JWT or OAuth2-based authentication, `bearerToken()` simulates requests that are authenticated using a Bearer token (common in REST APIs). You can pass the token as a string for testing purposes.

#### Example:

```java
mockMvc.perform(get("/api/protectedResource")
        .with(bearerToken("your-jwt-token")))
        .andExpect(status().isOk());
```

*   
  **`bearerToken()`**: This is typically used when your application is using JWT (JSON Web Tokens) for authentication. It simulates a request with a `Authorization: Bearer <token>` header, making it useful for testing REST API endpoints protected by JWT tokens.

### `httpBasic()`: Simulate Basic Authentication

The `httpBasic()` post-processor simulates Basic Authentication by adding an `Authorization: Basic <base64-encoded-username:password>` header to the request.

#### Example:

```java
mockMvc.perform(get("/protectedResource")
        .with(httpBasic("username", "password")))  // Simulate Basic Authentication
        .andExpect(status().isOk());
```

*   
  **`httpBasic()`**: This simulates Basic Authentication using a username and password. The credentials are encoded using Base64 and added to the `Authorization` header.

### `anonymous()`: Simulate an Anonymous (Unauthenticated) User

If you want to test how your application behaves for unauthenticated users, the `anonymous()` post-processor will simulate an anonymous (unauthenticated) user making a request.

#### Example:

```java
mockMvc.perform(get("/publicPage")
        .with(anonymous()))  // Simulate an anonymous user
        .andExpect(status().isOk());
```

*   
  **`anonymous()`**: This simulates an unauthenticated user, which can be useful when testing public pages or scenarios where access is restricted based on authentication.

---

## Example Scenario: Custom Security Context in a Test

Let’s say you are testing a controller that has security restrictions based on user roles. You can use `SecurityMockMvcRequestPostProcessors` to simulate users with different roles and verify whether they can access certain resources.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class SecurityTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testAccessAdminPageWithAdminRole() throws Exception {
        mockMvc.perform(get("/adminPage")
                .with(user("admin").password("adminpass").roles("ADMIN")))  // Simulating an admin user
                .andExpect(status().isOk());  // Should succeed because user has ADMIN role
    }

    @Test
    public void testAccessAdminPageWithUserRole() throws Exception {
        mockMvc.perform(get("/adminPage")
                .with(user("user").password("userpass").roles("USER")))  // Simulating a user with only USER role
                .andExpect(status().isForbidden());  // Should fail because the user is not an ADMIN
    }

    @Test
    public void testAccessPublicPageAsAnonymous() throws Exception {
        mockMvc.perform(get("/publicPage")
                .with(anonymous()))  // Simulating an unauthenticated user
                .andExpect(status().isOk());  // Should succeed because the page is public
    }
}
```

In this example:

* The `user()` post-processor is used to simulate users with specific roles (`ADMIN`, `USER`).

* The `anonymous()` post-processor is used to simulate an unauthenticated user.

## Conclusion

`SecurityMockMvcRequestPostProcessors` are powerful tools for simulating different security scenarios in your Spring Security tests. They allow you to customize the security context of each request, enabling you to test:

* **Authenticated users** with specific roles, authorities, or custom tokens.

* **Unauthenticated (anonymous) users**.

* **Custom authentication schemes**, such as Basic Authentication, JWT tokens, or CSRF tokens.

Using these post-processors, you can create a variety of test scenarios to ensure that your application's security configurations behave as expected, without needing to perform actual logins or complex setup.

