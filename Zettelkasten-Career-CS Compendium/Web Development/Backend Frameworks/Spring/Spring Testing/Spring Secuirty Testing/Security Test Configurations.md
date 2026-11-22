---
tags:
  - FORMAT
  - Spring
---
# Security Test Configurations in Spring

When writing tests for Spring Security, it’s crucial to properly configure your security settings to ensure that your tests simulate real-world scenarios accurately. **Security test configurations** help set up security contexts, authentication, authorization, and other security mechanisms that your tests require. Spring Security provides several tools and annotations to manage these configurations effectively during testing.

## Key Concepts in Security Test Configurations

Security test configurations are used to simulate the security environment for your tests, including the authentication and authorization settings. In a real application, users have various roles, authorities, and privileges, and your test configurations should account for these factors.

### Core Areas of Security Test Configurations:

* **Security Context**: It stores the details of the current authentication (e.g., the logged-in user, their roles, authorities).

* **Authentication**: How a user logs in or is authenticated (e.g., via username/password, OAuth2 tokens, etc.).

* **Authorization**: Configuring roles or authorities that define what a user can access or perform.

* **Security Filters**: Ensuring that security filters (e.g., CSRF protection, CORS, session management) are applied correctly during tests.

## Common Techniques for Configuring Security in Tests

### `@TestConfiguration`

`@TestConfiguration` is used to define test-specific configurations for security-related beans. This annotation allows you to customize your security configuration during testing while keeping the main configuration separate.

#### Example:

```java
@TestConfiguration
public class SecurityTestConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            .and()
            .formLogin()
            .permitAll();
        return http.build();
    }
}
```

*   
  This `SecurityTestConfig` will be applied during tests, allowing you to specify a custom security filter chain just for the test environment.

### `@EnableWebSecurity`

If you want to apply your security configuration globally, you can use the `@EnableWebSecurity` annotation, which is typically used in the main security configuration of the application. However, when testing, it can be used to configure custom security rules.

#### Example:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            .and()
            .formLogin();
    }
}
```

*   
  This configuration ensures that only users with the "ADMIN" role can access `/admin/**`, and users with the "USER" role can access `/user/**`.

### `@WithMockUser` for Authentication

One of the simplest and most commonly used annotations in Spring Security testing is `@WithMockUser`. It allows you to simulate a user with specific roles or authorities for a test method, eliminating the need to authenticate a real user.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAccessWithUserRole() throws Exception {
        mockMvc.perform(get("/userPage"))
               .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAccessWithAdminRole() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAccessDeniedForAdminPage() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());
    }
}
```

*   
  **`@WithMockUser`** simulates an authenticated user with the specified username and roles. In this example, the tests simulate two users: a `USER` and an `ADMIN`, testing access to `/userPage` and `/adminPage`.

### `@WithSecurityContext` for Custom Security Context

Sometimes you may need more control over the security context, such as simulating more complex authentication scenarios. In such cases, you can use `@WithSecurityContext` to define a custom security context for your tests.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class CustomSecurityContextTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithSecurityContext(factory = CustomSecurityContextFactory.class)
    public void testCustomSecurityContext() throws Exception {
        mockMvc.perform(get("/secureEndpoint"))
               .andExpect(status().isOk());
    }
}
```

*   
  **`@WithSecurityContext(factory = CustomSecurityContextFactory.class)`** uses a custom factory to set up the security context, allowing for more customized authentication, such as setting authorities, user details, or even simulating token-based authentication.

### `MockMvc` for Testing Secured Endpoints

When you are testing secured endpoints, `MockMvc` is a great tool for simulating HTTP requests. It allows you to perform various HTTP methods (GET, POST, etc.) and assert that the security configurations (e.g., authentication, authorization) are applied correctly.

#### Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class SecurityTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAccessDeniedForAdmin() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());  // Should be forbidden for non-admin users
    }

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAccessGrantedForAdmin() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());  // Should be accessible for admin users
    }
}
```

*   
  In this example, `MockMvc` is used to simulate HTTP requests, and the `@WithMockUser` annotation sets up the user with specific roles. The tests check if the correct HTTP status codes (`OK`, `Forbidden`) are returned based on the user’s role.

### `@SpringBootTest` with Custom Security Configurations

For integration tests, you might need to ensure the security configuration is applied throughout the whole Spring Boot application. This is where `@SpringBootTest` combined with `@AutoConfigureMockMvc` becomes useful.

#### Example:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class SecurityIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAdminAccess() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testUserAccessDenied() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());
    }
}
```

*   
  **`@SpringBootTest`** loads the full application context, ensuring that the security filters and other configurations are applied as they would be in production.

* **`@AutoConfigureMockMvc`** configures `MockMvc` to work with the embedded server, enabling you to perform HTTP requests against your controllers and test security constraints.

---

## Common Security Test Configurations

1. **Custom Security Configuration for Tests**:

   * Use `@TestConfiguration` to define security settings that are only used during tests, allowing you to mock authentication or authorization as needed.

2. **Testing Authentication**:

   * Simulate different authentication mechanisms (e.g., username/password, OAuth2) using `@WithMockUser` or a custom security context.

3. **Testing Authorization**:

   * Ensure that access control (e.g., role-based access, authority checks) is properly applied using `@WithMockUser`, `MockMvc`, and custom security contexts.

4. **CSRF Protection**:

   * When testing endpoints that are protected by CSRF (Cross-Site Request Forgery), you may need to disable CSRF in your test configuration or simulate a CSRF token.

5. **Session Management**:

   * If you are testing session management (e.g., timeouts, invalid sessions), you can configure session settings in the `SecurityFilterChain` and test how your application responds to session-based requests.

6. **OAuth2 and JWT Authentication**:

   * If your application uses OAuth2 or JWT-based authentication, configure the test to mock authentication tokens or simulate OAuth2 flows.

---

## Conclusion

Configuring security for tests in Spring applications is essential to ensure that your authentication, authorization, and other security mechanisms are working correctly. **Security test configurations** involve setting up custom security filters, contexts, and authentication details to simulate real-world scenarios during testing. With tools like `@TestConfiguration`, `@WithMockUser`, `@WithSecurityContext`, and `MockMvc`, you can easily test security aspects like user roles, permissions, and access control, ensuring that your application is secure and functions as expected.

