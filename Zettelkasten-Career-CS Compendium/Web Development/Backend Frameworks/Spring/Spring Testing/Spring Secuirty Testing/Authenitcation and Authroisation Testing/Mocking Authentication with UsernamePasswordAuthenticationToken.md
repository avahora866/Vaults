---
tags:
  - FORMAT
  - Spring
---
# Mocking Authentication with `UsernamePasswordAuthenticationToken`

In Spring Security, authentication is the process of verifying the identity of a user. When testing Spring Security configurations, you often need to mock authentication to simulate a user who is logged in. One way to do this is by using `UsernamePasswordAuthenticationToken`, which is a class that implements the `Authentication` interface in Spring Security. This token is used to represent a user's authentication credentials (e.g., username and password) and their granted authorities or roles.

Mocking authentication with `UsernamePasswordAuthenticationToken` is helpful in unit and integration tests when you need to simulate a user being authenticated without relying on an actual login process.

## What is `UsernamePasswordAuthenticationToken`?

`UsernamePasswordAuthenticationToken` is a Spring Security class used to represent a user's authentication credentials, typically consisting of a username and a password. It is often used for manual authentication (e.g., in unit tests) when you need to mock a user's authentication details.

This token can be populated with a user's credentials and authorities, and then passed into the `SecurityContext`, which represents the security context of the current request. Once in the `SecurityContext`, the token is accessible throughout the application to determine if the user is authenticated and what roles or authorities they have.

## How `UsernamePasswordAuthenticationToken` Works

`UsernamePasswordAuthenticationToken` works by accepting the following parameters:

* **Principal**: The user's username or identifier.

* **Credentials**: The user's password or other authentication credentials (often set to `null` when using tokens).

* **Authorities**: A list of granted authorities or roles that the user has.

It can be used either as an **unauthenticated** token (when the credentials are empty) or an **authenticated** token (with user authorities set).

## Mocking Authentication with `UsernamePasswordAuthenticationToken`

In tests, mocking authentication allows you to simulate different users with various roles and permissions. The `UsernamePasswordAuthenticationToken` is typically created manually in a test method and then set into the `SecurityContext` to mock the authentication.

## Steps for Mocking Authentication

### Step 1: Set up the `SecurityContext`

In Spring Security, the `SecurityContext` holds the `Authentication` object. When testing, you can manually create a `UsernamePasswordAuthenticationToken` and set it into the `SecurityContext`.

### Step 2: Use `UsernamePasswordAuthenticationToken` to Mock Authentication

You can use `UsernamePasswordAuthenticationToken` to simulate an authenticated user and inject it into the `SecurityContext`.

## Example: Mocking Authentication in Tests

Here is a typical use case for mocking authentication in a unit or integration test using `UsernamePasswordAuthenticationToken`:

### Mocking Authentication in Unit Tests

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testAuthenticatedUser() throws Exception {
        // Create an Authentication token
        UsernamePasswordAuthenticationToken authentication =
                new UsernamePasswordAuthenticationToken("user", "password", Collections.singletonList(new SimpleGrantedAuthority("ROLE_USER")));

        // Set the Authentication token in the SecurityContext
        SecurityContextHolder.getContext().setAuthentication(authentication);

        // Perform the test with MockMvc
        mockMvc.perform(get("/securePage"))
                .andExpect(status().isOk());  // Should succeed since the user has ROLE_USER
    }

    @Test
    public void testUnauthenticatedUser() throws Exception {
        // No authentication set in SecurityContext, simulating an unauthenticated user
        SecurityContextHolder.clearContext();

        // Perform the test
        mockMvc.perform(get("/securePage"))
                .andExpect(status().isUnauthorized());  // Should fail due to lack of authentication
    }
}
```

## Explanation:

* **Authentication Token**: The `UsernamePasswordAuthenticationToken` is created by passing in the username, password, and a list of authorities (`ROLE_USER` in this case). In a real-world scenario, authorities would be granted based on the user's roles.

* **Setting the Security Context**: The `SecurityContextHolder.getContext().setAuthentication(authentication)` method is used to set the created authentication token into the `SecurityContext`. This makes the user "authenticated" for the duration of the test.

* **Testing Protected Endpoints**: The test then performs a request to a protected resource (`/securePage`). The result depends on whether the user has the required role (`ROLE_USER`) to access the endpoint.

### Mocking Authentication with `@WithMockUser`

Spring Security’s `@WithMockUser` annotation can also be used to mock authentication in tests without explicitly creating an `Authentication` token. However, `UsernamePasswordAuthenticationToken` allows you to more precisely control the authentication details (e.g., credentials, authorities).

Example using `@WithMockUser`:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testWithMockUser() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());  // Should pass for admin
    }

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testWithMockUserUserRole() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());  // Should fail for non-admin user
    }
}
```

This test uses `@WithMockUser` to mock a user with a specific role, which is simpler and doesn't require manual creation of the authentication token.

### Clearing the Security Context

After mocking authentication in your tests, you might need to clear the `SecurityContext` to ensure that the context doesn't persist across multiple tests, which could lead to security issues or unexpected behavior.

```java
SecurityContextHolder.clearContext();
```

This clears the authentication and any associated details from the `SecurityContext`, making the user unauthenticated for subsequent tests.

---

## Customizing `UsernamePasswordAuthenticationToken`

In addition to mocking simple users with a username and password, you can also customize the `UsernamePasswordAuthenticationToken` to simulate more complex scenarios:

* **Simulating Custom Authorities**: Instead of using roles like `ROLE_USER` or `ROLE_ADMIN`, you can mock custom authorities (e.g., `PERMISSION_READ`, `PERMISSION_WRITE`).

Example of custom authorities:

```java
GrantedAuthority readAuthority = new SimpleGrantedAuthority("PERMISSION_READ");
GrantedAuthority writeAuthority = new SimpleGrantedAuthority("PERMISSION_WRITE");

UsernamePasswordAuthenticationToken authentication =
        new UsernamePasswordAuthenticationToken("user", "password", Arrays.asList(readAuthority, writeAuthority));
```

*   
  **Simulating Disabled Accounts**: You can mock a disabled account by using the `setDetails()` method or creating a custom `Authentication` object.

---

## Integrating with Spring Security Tests

When using `UsernamePasswordAuthenticationToken` in integration tests, it is common to use tools like `MockMvc` or `@SpringBootTest` to perform requests and validate security restrictions. The following steps can be followed:

* Use `UsernamePasswordAuthenticationToken` to simulate different users in tests.

* Set the token in the `SecurityContext` so that it can be accessed by Spring Security.

* Use `MockMvc` or `WebTestClient` to make requests and assert the results based on the user’s roles and authorities.

Example:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class MyServiceTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testAccessWithCustomAuthentication() throws Exception {
        // Mocking custom authentication
        UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken("admin", "password", Collections.singletonList(new SimpleGrantedAuthority("ROLE_ADMIN")));
        SecurityContextHolder.getContext().setAuthentication(authentication);

        // Test the secured resource
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());  // Should pass because the user has ROLE_ADMIN
    }
}
```

---

## Conclusion

`UsernamePasswordAuthenticationToken` is a powerful way to mock authentication in Spring Security tests, providing control over the username, password, and authorities associated with the user. This allows for more precise testing of security configurations and ensures that you can simulate different authentication scenarios in unit and integration tests.

* **Manual Authentication**: Use `UsernamePasswordAuthenticationToken` to manually create authentication tokens and set them in the `SecurityContext`.

* **Test User Roles and Permissions**: You can mock users with various roles and authorities to test different access control rules.

* **Integration with MockMvc**: Once the authentication is mocked, you can use `MockMvc` to test access to secured endpoints based on the user’s roles or authorities.

