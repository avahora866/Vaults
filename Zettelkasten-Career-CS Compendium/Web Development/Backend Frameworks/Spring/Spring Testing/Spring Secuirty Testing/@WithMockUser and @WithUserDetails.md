---
tags:
  - FORMAT
  - Spring
---
# `@WithMockUser` and `@WithUserDetails` in Spring Security Testing

In Spring Security testing, **`@WithMockUser`** and **`@WithUserDetails`** are annotations used to simulate authenticated users for your test methods. These annotations help you test how your application behaves when a user with specific roles, authorities, or details interacts with secured endpoints.

## `@WithMockUser` Annotation

`@WithMockUser` is the most common and straightforward way to mock a user for authentication in Spring Security tests. It allows you to simulate a user with a specific username, roles, and other details (such as authorities or password) without needing to perform an actual login. It is typically used when you want to test authorization logic and access control for specific roles.

### Features of `@WithMockUser`:

* **Username**: Simulates a user with the specified username.

* **Roles**: Specifies the roles assigned to the mocked user (which determine access permissions).

* **Password**: By default, it sets a password to "password", but you can specify a custom password if needed.

* **Authorities**: You can specify specific authorities or privileges that the user possesses.

### Usage Example:

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
               .andExpect(status().isOk());  // User should have access to the userPage
    }

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAccessWithAdminRole() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());  // Admin should have access to the adminPage
    }

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAccessDeniedForAdmin() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());  // User should be denied access to the adminPage
    }
}
```

*   
  **`@WithMockUser(username = "user", roles = "USER")`**: Simulates a user named `user` with the role `USER`. This allows testing the `userPage` and checking if it returns the correct HTTP status (OK).

* **`@WithMockUser(username = "admin", roles = "ADMIN")`**: Simulates an admin user with the role `ADMIN`, allowing access to `adminPage`.

### Default Behavior:

* If you don’t specify `username`, `roles`, or `password`, Spring will use default values:

  * **Username**: "user"

  * **Password**: "password"

  * **Roles**: `USER`

### Key Use Cases:

* Testing authorization based on roles (`ROLE_USER`, `ROLE_ADMIN`, etc.).

* Simulating users for controller tests where the backend is secured.

---

## `@WithUserDetails` Annotation

`@WithUserDetails` is an annotation used to simulate a user by fetching user details from an underlying **user service** (i.e., a custom implementation of `UserDetailsService`). This is useful when you want to mock a real user who is retrieved from a database or another persistent store, rather than just a simple mocked user with roles and authorities.

### Features of `@WithUserDetails`:

* **UserDetailsService Integration**: It works with a `UserDetailsService` implementation, which loads user-specific details (like roles, authorities, and other properties) from the database or an in-memory store.

* **Custom User Setup**: It allows for more complex user scenarios (such as fetching users with specific roles, permissions, or other details) than `@WithMockUser`.

### Usage Example:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
@Import(TestUserDetailsService.class)  // Import the custom UserDetailsService
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithUserDetails("admin")  // Assumes "admin" is a valid username in the UserDetailsService
    public void testAccessWithAdmin() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk());  // Admin user should have access
    }

    @Test
    @WithUserDetails("user")  // Assumes "user" is a valid username in the UserDetailsService
    public void testAccessWithUser() throws Exception {
        mockMvc.perform(get("/userPage"))
               .andExpect(status().isOk());  // User should have access
    }
}
```

*   
  **`@WithUserDetails("admin")`**: Simulates a user by retrieving details for the username `admin` from the `UserDetailsService` (usually fetching the roles, authorities, and other properties defined for the `admin` user).

### How It Works:

* The `@WithUserDetails` annotation works by calling the `loadUserByUsername` method of your `UserDetailsService` (such as `JdbcUserDetailsService` or `InMemoryUserDetailsManager`).

* You can specify any username that exists in your configured `UserDetailsService` to authenticate the user and apply the appropriate authorities/roles as defined in the user details.

### Example of Custom `UserDetailsService`:

```java
@Configuration
public class TestUserDetailsService {

    @Bean
    public UserDetailsService userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();

        UserDetails adminUser = User.withUsername("admin")
                                    .password("{noop}adminpass")  // Noop password encoder for simplicity
                                    .roles("ADMIN")
                                    .build();

        UserDetails normalUser = User.withUsername("user")
                                     .password("{noop}userpass")
                                     .roles("USER")
                                     .build();

        manager.createUser(adminUser);
        manager.createUser(normalUser);

        return manager;
    }
}
```

*   
  This `TestUserDetailsService` creates two users, `admin` and `user`, with their respective roles and passwords. The `@WithUserDetails` annotation will use this service to load user details for the specified username.

### Key Use Cases:

* Testing authentication and authorization with real `UserDetails` loaded from a persistent store.

* Simulating users based on your application’s real security configuration (e.g., users with passwords, authorities, or more complex authentication mechanisms).

---

## Comparison Between `@WithMockUser` and `@WithUserDetails`

| Feature | `@WithMockUser` | `@WithUserDetails` |
| ----- | ----- | ----- |
| **User Details Source** | Simulates a mock user with basic details (username, roles, password). | Loads user details from a configured `UserDetailsService`. |
| **Customization** | Limited to roles, username, password. | Fully customizable to reflect the real user details from your database or service. |
| **Complexity** | Simple to use for most basic tests. | Requires a configured `UserDetailsService` and may involve more setup. |
| **Usage Scenario** | Suitable for testing basic access control with simple roles and permissions. | Best for testing more complex scenarios where user details are dynamically loaded (e.g., from a database). |
| **Default Values** | Defaults to `username = "user"`, `roles = "USER"`. | Requires the `UserDetailsService` to provide valid user details. |

---

## Conclusion

* **`@WithMockUser`** is great for simple tests where you need to simulate a user with a specific username, roles, and optional password. It’s easy to use for basic tests of access control.

* **`@WithUserDetails`** is more powerful and is typically used when you need to simulate a user based on a real `UserDetailsService`. It allows for testing more complex security scenarios where the user is retrieved dynamically from a persistent store (e.g., database).

Both annotations are crucial in Spring Security testing, but the choice depends on the complexity of your security setup and whether you need to work with mock users or real user data.

