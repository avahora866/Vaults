---
tags:
  - FORMAT
  - Spring
---
# Testing Method-Level Security with `@PreAuthorize` and `@PostAuthorize`

In Spring Security, method-level security is an important feature that allows developers to protect methods with access control rules based on roles, authorities, or other security-related conditions. The annotations `@PreAuthorize` and `@PostAuthorize` are used to define authorization rules at the method level, ensuring that only authorized users can invoke specific methods.

## `@PreAuthorize` Annotation

`@PreAuthorize` is used to secure methods before they are executed. The security check happens **before** the method runs, meaning the access control is evaluated prior to entering the method. This is useful for applying security rules based on the current user's roles, authorities, or other attributes.

### Syntax:

```java
@PreAuthorize("expression")
```

*   
  **expression**: This is a SpEL (Spring Expression Language) expression that defines the security condition. If the expression evaluates to `true`, the method will be executed. If it evaluates to `false`, access will be denied.

### Example:

```java
@Service
public class MyService {

    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        // Delete user logic
    }

   @PreAuthorize("hasPermission(#userId, 'delete')")
    public void deleteUserWithPermission(Long userId) {
        // Delete user logic with permission check
    }
}
```

*   
  The `deleteUser()` method can only be called by a user with the role `ADMIN`.

* The `deleteUserWithPermission()` method checks for a custom permission on the user being deleted.

### Testing `@PreAuthorize`

When testing methods secured with `@PreAuthorize`, we simulate different users with various roles or permissions to verify that the security rules are correctly enforced.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class SecurityTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(roles = "ADMIN")
    public void testDeleteUserWithAdminRole() throws Exception {
        mockMvc.perform(delete("/deleteUser/1"))
               .andExpect(status().isOk());  // Admin has access
    }

    @Test
    @WithMockUser(roles = "USER")
    public void testDeleteUserWithUserRole() throws Exception {
        mockMvc.perform(delete("/deleteUser/1"))
               .andExpect(status().isForbidden());  // User does not have access
    }
}
```

*   
  In the above example:

  * The first test uses `@WithMockUser(roles = "ADMIN")` to simulate an admin user. The test expects the method to succeed.

  * The second test uses `@WithMockUser(roles = "USER")` to simulate a user without the necessary `ADMIN` role. The test expects a `Forbidden` status.

---

## `@PostAuthorize` Annotation

`@PostAuthorize` is used to secure methods after they are executed. The security check happens **after** the method has run, which allows you to evaluate the result of the method execution before deciding whether access should be granted.

### Syntax:

```java
@PostAuthorize("expression")
```

*   
  **expression**: This is a SpEL (Spring Expression Language) expression that is evaluated **after** the method executes. If the expression evaluates to `true`, the method's result is returned to the caller. If the expression evaluates to `false`, an exception (such as `AccessDeniedException`) is thrown.

### Example:

```java
@Service
public class MyService {

    @PostAuthorize("returnObject.username == authentication.name")
    public User getUserByUsername(String username) {
        // Fetch user logic
        return userRepository.findByUsername(username);
    }
}
```

*   
  In this example, the method `getUserByUsername()` returns a `User` object, and the access control is applied **after** the method is executed. The `@PostAuthorize` annotation ensures that the returned `User`'s username matches the current authenticated user’s username (`authentication.name`).

### Testing `@PostAuthorize`

Testing methods with `@PostAuthorize` involves simulating users with different roles or usernames and checking if access is granted based on the result of the method execution.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class SecurityTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "admin")
    public void testGetUserByUsernameWithMatchingUsername() throws Exception {
        mockMvc.perform(get("/getUser/admin"))
               .andExpect(status().isOk());  // Access granted because the username matches
    }

    @Test
    @WithMockUser(username = "user")
    public void testGetUserByUsernameWithNonMatchingUsername() throws Exception {
        mockMvc.perform(get("/getUser/admin"))
               .andExpect(status().isForbidden());  // Access denied because the username doesn't match
    }
}
```

*   
  In the first test, the method will return the `User` object for `admin` because the authenticated username matches the username in the method.

* In the second test, the method will deny access because the authenticated user is `user`, and the requested `admin` username doesn’t match.

---

## Differences Between `@PreAuthorize` and `@PostAuthorize`

| Feature | `@PreAuthorize` | `@PostAuthorize` |
| ----- | ----- | ----- |
| **When Security is Applied** | Before the method executes. | After the method has executed. |
| **Usage Scenario** | To check if the user has the necessary roles/permissions before invoking the method. | To check conditions on the result of the method (e.g., based on the return value). |
| **Typical Use Case** | Protecting methods where access needs to be checked before execution, e.g., checking roles/permissions before performing an action. | Protecting methods where access should depend on the returned data, e.g., ensuring users can only view their own data. |
 Expression Example | `hasRole('ADMIN')`, `hasPermission(#userId, 'read')` | `returnObject.username == authentication.name` |
| **Security Evaluation** | Based on the method parameters or the current user. | Based on the method result (e.g., object returned). |

## Testing Both `@PreAuthorize` and `@PostAuthorize`

Both annotations can be used to secure methods in a service class, and you can combine them to have more control over your method-level security. You can test them using `@WithMockUser` or other relevant annotations to simulate users with different roles or attributes.

```java
@Service
public class UserService {

    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        // Method to delete user
    }

    @PostAuthorize("returnObject.username == authentication.name")
    public User getUserByUsername(String username) {
        // Method to fetch user details
        return userRepository.findByUsername(username);
    }
}
```

*   
  **`deleteUser`**: Only accessible by users with the `ADMIN` role. You can test with `@WithMockUser(roles = "ADMIN")` and `@WithMockUser(roles = "USER")`.

* **`getUserByUsername`**: Only allows users to fetch their own user data based on the username. You can test this with `@WithMockUser(username = "admin")` or other usernames.

---

## Conclusion

* **`@PreAuthorize`** is used to perform security checks **before** the method executes, making it suitable for verifying if a user has the necessary permissions or roles to perform an action.

* **`@PostAuthorize`** is used to perform security checks **after** the method has executed, which is useful for ensuring that the results of a method call meet certain security criteria (e.g., returning data specific to the authenticated user).

* Both annotations provide powerful ways to apply method-level security to your Spring applications, and by using them in your tests, you can verify that the security constraints are correctly enforced based on the current user’s roles, permissions, or data returned by the method.

