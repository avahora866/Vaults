---
tags:
  - FORMAT
  - Spring
---
# Role-Based and Permission-Based Testing

In the context of Spring Security, **Role-Based Testing** and **Permission-Based Testing** are methods of ensuring that the security configurations of an application are functioning as expected. They focus on testing whether access to certain resources or functionality is granted or denied based on the user's roles or specific permissions.

## Role-Based Testing

Role-based testing focuses on **roles** assigned to users to determine their access levels to various resources in the application. Roles are often used to group users with similar authority or permissions. For example, a "USER" role might have access to standard resources, while an "ADMIN" role might have access to more sensitive resources.

In Spring Security, roles are generally granted to users after authentication and are used to control access to different parts of the application. For example, certain controllers or endpoints might be restricted to users with specific roles.

### Example of Role-Based Testing:

Imagine you have a system where there are two roles: `USER` and `ADMIN`.

* Users with the `USER` role should only be able to access basic resources.

* Users with the `ADMIN` role should be able to access both basic and admin-level resources.

### Controller Example:

```java
@RestController
public class MyController {

    @GetMapping("/userPage")
    @PreAuthorize("hasRole('USER')")
    public String userPage() {
        return "This page is for users.";
    }

    @GetMapping("/adminPage")
    @PreAuthorize("hasRole('ADMIN')")
    public String adminPage() {
        return "This page is for admins.";
    }
}
```

### Role-Based Test with `@WithMockUser`:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    // Test for a user with ROLE_USER
    @Test
    @WithMockUser(roles = "USER")
    public void testUserPage() throws Exception {
        mockMvc.perform(get("/userPage"))
               .andExpect(status().isOk())
               .andExpect(content().string("This page is for users."));
    }

    // Test for a user with ROLE_ADMIN
    @Test
    @WithMockUser(roles = "ADMIN")
    public void testAdminPage() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isOk())
               .andExpect(content().string("This page is for admins."));
    }

    // Test for a user without the correct role
    @Test
    @WithMockUser(roles = "USER")
    public void testAdminPageUnauthorized() throws Exception {
        mockMvc.perform(get("/adminPage"))
               .andExpect(status().isForbidden());
    }
}
```

### Explanation:

* **Test for `USER` role**: The `@WithMockUser(roles = "USER")` annotation mocks a user with the `USER` role. The test checks whether they can access the `/userPage` endpoint and verifies that the correct response is returned.

* **Test for `ADMIN` role**: The `@WithMockUser(roles = "ADMIN")` annotation mocks a user with the `ADMIN` role. The test ensures that the `/adminPage` endpoint is accessible for the `ADMIN` role.

* **Unauthorized access**: A user with the `USER` role is tested for access to the `/adminPage`. Since they don't have the `ADMIN` role, they are expected to receive a `403 Forbidden` response.

## Permission-Based Testing

Permission-based testing focuses on **permissions** granted to users. Permissions are often more granular than roles and define specific actions a user can perform (e.g., "read", "write", "delete"). Unlike roles, which group users into categories with broad access levels, permissions allow you to define fine-grained access control.

In Spring Security, permissions are typically granted through authorities or custom permission models. Permission-based access control is often used in combination with roles but provides more flexibility when assigning access rights.

### Example of Permission-Based Testing:

Imagine that you have an application where users with specific permissions can access different features, like `READ_USER` and `WRITE_USER`. This is different from roles, where users are grouped by their general access level (e.g., `USER`, `ADMIN`).

### Controller Example with Permissions:

```java
@RestController
public class MyController {

    @GetMapping("/viewUser")
    @PreAuthorize("hasAuthority('READ_USER')")
    public String viewUser() {
        return "This page is for viewing user details.";
    }

    @PostMapping("/editUser")
    @PreAuthorize("hasAuthority('WRITE_USER')")
    public String editUser() {
        return "This page is for editing user details.";
    }
}
```

In this example:

* `READ_USER` permission allows the user to view user details.

* `WRITE_USER` permission allows the user to edit user details.

### Permission-Based Test with `@WithMockUser`:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    // Test for user with READ_USER permission
    @Test
    @WithMockUser(authorities = "READ_USER")
    public void testViewUserPage() throws Exception {
        mockMvc.perform(get("/viewUser"))
               .andExpect(status().isOk())
               .andExpect(content().string("This page is for viewing user details."));
    }

    // Test for user with WRITE_USER permission
    @Test
    @WithMockUser(authorities = "WRITE_USER")
    public void testEditUserPage() throws Exception {
        mockMvc.perform(post("/editUser"))
               .andExpect(status().isOk())
               .andExpect(content().string("This page is for editing user details."));
    }

    // Test for user without the correct permission
    @Test
    @WithMockUser(authorities = "READ_USER")
    public void testEditUserPageForbidden() throws Exception {
        mockMvc.perform(post("/editUser"))
               .andExpect(status().isForbidden());
    }
}
```

### Explanation:

* **Test for `READ_USER` permission**: The `@WithMockUser(authorities = "READ_USER")` annotation mocks a user with the `READ_USER` permission. The test checks whether they can access the `/viewUser` endpoint.

* **Test for `WRITE_USER` permission**: The `@WithMockUser(authorities = "WRITE_USER")` annotation mocks a user with the `WRITE_USER` permission. The test ensures that the user can access the `/editUser` endpoint.

* **Unauthorized access**: A user with the `READ_USER` permission is tested for access to the `/editUser` endpoint. Since they don't have the `WRITE_USER` permission, they are expected to receive a `403 Forbidden` response.

## Role-Based vs Permission-Based Testing

| Aspect | Role-Based Testing | Permission-Based Testing |
| ----- | ----- | ----- |
| **Granularity** | Roles are broad categories (e.g., `ADMIN`, `USER`) for access control. | Permissions are more granular and define specific actions (e.g., `READ_USER`, `WRITE_USER`). |
| **Use Case** | Used when access control is based on general roles. | Used for more fine-grained control where actions are permission-based. |
| **Typical Use** | Granting or restricting access to entire resources (e.g., admin pages, user pages). | Granting or restricting specific actions within resources (e.g., editing, viewing). |
| **Test Focus** | Testing access based on roles assigned to users. | Testing access based on specific permissions granted to users. |

## Conclusion

* **Role-Based Testing** focuses on testing access based on roles assigned to users (e.g., `USER`, `ADMIN`). It is useful for testing broader access control mechanisms where the user’s role determines their permissions across the system.

* **Permission-Based Testing** focuses on testing access based on specific permissions granted to users (e.g., `READ_USER`, `WRITE_USER`). It is useful for more fine-grained control and testing scenarios where actions on resources are protected by individual permissions rather than roles.

Both testing strategies are crucial for ensuring that your application’s security configuration is robust and performs as expected. Depending on your application’s security model, you may use one or both types of testing to ensure that users can access only the resources and actions they are authorized to use.

