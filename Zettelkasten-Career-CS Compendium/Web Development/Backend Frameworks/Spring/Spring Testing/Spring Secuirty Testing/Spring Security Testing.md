---
tags:
  - FORMAT
  - Spring
---
# Spring Security Testing

Spring Security is a powerful and customizable framework for securing Java applications. In many cases, especially for web applications, you need to ensure that your security mechanisms (like authentication, authorization, and roles) are working correctly. **Spring Security Testing** provides tools and annotations to help you easily test your security configurations and ensure that your application's security behaves as expected.

## Key Concepts in Spring Security Testing

Spring Security testing is designed to verify that the security constraints you have configured are working correctly, including aspects like user authentication, authorization, access control, role management, and URL security.

### Key Areas to Test in Spring Security:

* **Authentication**: Ensure that users are correctly authenticated with the right credentials.

* **Authorization**: Ensure that users can access resources they are authorized to access and are denied resources they shouldn't have access to.

* **Roles and Permissions**: Ensure that users with specific roles have the correct level of access to resources.

* **Security Context**: Ensure that the security context is properly set and managed throughout the application.

* **HTTP Security**: Ensure that proper HTTP security configurations are applied, such as CSRF protection, session management, etc.

---

## Spring Security Testing Tools and Annotations

Spring Security provides several tools, annotations, and utility methods for testing different aspects of security.

### `@WithMockUser` Annotation

`@WithMockUser` is a Spring Security annotation that allows you to simulate a user being authenticated in your tests. You can specify roles and usernames for the simulated user.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAccessToUserPage() throws Exception {
        mockMvc.perform(get("/userPage"))
               .andExpect(status().isOk())
               .andExpect(content().string("Welcome User"));
    }

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAccessToAdminPage() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk())
               .andExpect(content().string("Welcome Admin"));
    }
}
```

*   
  **`@WithMockUser(username = "user", roles = "USER")`** simulates a user with the username `user` and the role `USER`.

* You can test different scenarios where specific users are granted or denied access to various parts of the application.

### `@WithSecurityContext` Annotation

`@WithSecurityContext` is a more advanced annotation that allows you to create a custom security context for a test method. It can be used when `@WithMockUser` doesn't provide the necessary level of customization.

#### Example:

```java
@WithSecurityContext(factory = CustomSecurityContextFactory.class)
public class MyCustomSecurityContextTest {

    @Test
    public void testAccessWithCustomSecurityContext() {
        // Custom logic for a user with specific permissions
    }
}
```

*   
  This is useful when you need to mock complex security contexts, such as a user with specific authorities or details.

### `@SpringBootTest` with `@AutoConfigureMockMvc`

If you want to test Spring Security in the context of the full application, including authentication and authorization, you can use the `@SpringBootTest` annotation combined with `@AutoConfigureMockMvc`. This sets up the Spring Boot application and the embedded server, allowing you to perform full integration tests.

#### Example:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class SecurityIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testUserPageAccess() throws Exception {
        mockMvc.perform(get("/userPage"))
               .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAdminPageAccess() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAdminPageAccessDenied() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());
    }
}
```

*   
  **`@SpringBootTest`** initializes the Spring Boot application context.

* **`@AutoConfigureMockMvc`** automatically configures `MockMvc` for your tests.

* The tests verify that users with the correct roles can access certain pages and that unauthorized users are blocked.

### `MockMvc` and Security Filters

When testing with `MockMvc`, the `SecurityContext` is automatically populated, and Spring Security filters are invoked. This means you can test URL access control, authentication, and authorization as part of the test.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(roles = "USER")
    public void testAccessWithUserRole() throws Exception {
        mockMvc.perform(get("/secureEndpoint"))
               .andExpect(status().isOk());  // Should succeed because the user has the USER role
    }

    @Test
    @WithMockUser(roles = "GUEST")
    public void testAccessWithGuestRole() throws Exception {
        mockMvc.perform(get("/secureEndpoint"))
               .andExpect(status().isForbidden());  // Should fail because the GUEST role does not have access
    }
}
```

*   
  In this example, `MockMvc` tests access to a secured endpoint based on the roles assigned to the simulated user.

---

## Testing Authentication and Authorization

Spring Security testing allows you to verify both authentication and authorization scenarios.

### Authentication Tests

Authentication tests focus on verifying that users can log in correctly with the right credentials.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class AuthenticationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testLoginSuccess() throws Exception {
        mockMvc.perform(post("/login")
                .param("username", "user")
                .param("password", "password"))
                .andExpect(status().isOk())
                .andExpect(content().string("Welcome, user"));
    }

    @Test
    public void testLoginFailure() throws Exception {
        mockMvc.perform(post("/login")
                .param("username", "user")
                .param("password", "wrongpassword"))
                .andExpect(status().isUnauthorized());
    }
}
```

*   
  **Test for login success and failure**: The tests ensure that authentication works as expected. A valid username/password combination should log the user in, while an invalid one should return an unauthorized status.

### Authorization Tests

Authorization tests focus on ensuring that users have access to the correct resources based on their roles or permissions.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class AuthorizationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(roles = "USER")
    public void testUserAccessToAdminPage() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());  // User should be denied access to the admin page
    }

    @Test
    @WithMockUser(roles = "ADMIN")
    public void testAdminAccessToAdminPage() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());  // Admin should have access to the admin page
    }
}
```

*   
  **Authorization tests** ensure that users are granted or denied access based on their roles.

---

## Custom Security Context for Testing

In some cases, you may need to test security with a custom security context. For instance, you may want to test with specific authentication tokens, authorities, or user details.

You can use **`SecurityContextHolder`** and **`SecurityContext`** to manually set the authentication details for a test.

#### Example:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class CustomSecurityContextTest {

    @Autowired
    private MockMvc mockMvc;

    @Before
    public void setUp() {
        // Manually set the custom security context for testing
        SecurityContextHolder.getContext().setAuthentication(new UsernamePasswordAuthenticationToken("user", "password", AuthorityUtils.createAuthorityList("ROLE_USER")));
    }

    @Test
    public void testWithCustomSecurityContext() throws Exception {
        mockMvc.perform(get("/secureEndpoint"))
               .andExpect(status().isOk());
    }

    @After
    public void tearDown() {
        SecurityContextHolder.clearContext();  // Clean up the security context
    }
}
```

This approach allows you to manually simulate different user details and roles during the test.

---

## Conclusion

Testing security in Spring applications is crucial to ensure that your authentication, authorization, and access control mechanisms are working correctly. Spring Security Testing tools such as `@WithMockUser`, `@SpringBootTest`, `MockMvc`, and `SecurityContext` provide the necessary tools to test various security aspects, including user login, role-based access, and custom security contexts. By writing comprehensive security tests, you can ensure your application is robust against unauthorized access and behaves as expected under different security configurations.

