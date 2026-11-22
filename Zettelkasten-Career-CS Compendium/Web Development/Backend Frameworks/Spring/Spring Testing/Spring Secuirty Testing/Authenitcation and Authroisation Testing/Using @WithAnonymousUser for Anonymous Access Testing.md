---
tags:
  - FORMAT
  - Spring
---
# Using `@WithAnonymousUser` for Anonymous Access Testing

In Spring Security, `@WithAnonymousUser` is an annotation that is used to simulate an **anonymous** user in tests. An anonymous user is a user who is not authenticated or logged in, and is usually treated as a guest who has limited access to the application. This is commonly used for testing scenarios where certain endpoints are publicly accessible or restricted based on whether the user is logged in.

When testing web applications, you often need to check if certain resources are accessible to anonymous users. The `@WithAnonymousUser` annotation allows you to simulate a scenario where no authenticated user is present, making it a useful tool for testing endpoints that are supposed to be accessible by unauthenticated or anonymous users.

## What is an Anonymous User?

An **anonymous user** in the context of web applications is typically a user who has not provided any authentication credentials (like username and password) to the system. They might still be able to access public resources, but they won't have access to secure or restricted endpoints that require proper authentication.

In Spring Security, an anonymous user is treated as a user who is not authenticated and is often assigned an empty `Authentication` object. They might have restricted permissions compared to authenticated users.

## The `@WithAnonymousUser` Annotation

The `@WithAnonymousUser` annotation is a Spring Security test annotation that simulates an anonymous user for the purposes of testing. It can be used with unit tests or integration tests to verify how the application behaves when a user is not logged in. This annotation automatically sets the `SecurityContext` with an anonymous user before running the test.

## How Does `@WithAnonymousUser` Work?

When `@WithAnonymousUser` is used in a test method, Spring Security automatically sets an **anonymous authentication token** in the `SecurityContext`. This token indicates that the user is not authenticated, and typically the authorities of this user are empty.

* The `Authentication` object for an anonymous user has a `null` principal and a `null` set of granted authorities.

* If you need to test whether an endpoint is accessible to an anonymous user, the application can check whether the `Authentication` is `null` or if the user has the appropriate authorities for accessing a particular resource.

## Example Use Case: Testing Anonymous Access

Consider the following example where we have a controller with some endpoints that are accessible by anyone (i.e., anonymous users) and others that require authentication.

### Controller Example:

```java
@RestController
public class MyController {

    @GetMapping("/publicPage")
    public String publicPage() {
        return "This page is publicly accessible.";
    }

    @GetMapping("/securedPage")
    @PreAuthorize("hasRole('USER')")
    public String securedPage() {
        return "This page is secured and requires authentication.";
    }
}
```

In this example:

* `/publicPage` should be accessible by **anyone**, even without authentication.

* `/securedPage` is protected by the `@PreAuthorize` annotation and can only be accessed by users with the `USER` role.

### Test for Anonymous Access:

We can use `@WithAnonymousUser` to test the behavior of these endpoints for anonymous users.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithAnonymousUser
    public void testPublicPageForAnonymousUser() throws Exception {
        mockMvc.perform(get("/publicPage"))
               .andExpect(status().isOk())  // Anonymous user should have access to this page
               .andExpect(content().string("This page is publicly accessible."));
    }

    @Test
    @WithAnonymousUser
    public void testSecuredPageForAnonymousUser() throws Exception {
        mockMvc.perform(get("/securedPage"))
               .andExpect(status().isUnauthorized());  // Anonymous user should be denied access to secured pages
    }
}
```

## Explanation:

* **First Test (`testPublicPageForAnonymousUser`)**:

  * The `@WithAnonymousUser` annotation simulates an anonymous user.

  * When performing a request to `/publicPage`, it should return an HTTP status of `200 OK` because this page is accessible by anyone, including anonymous users.

* **Second Test (`testSecuredPageForAnonymousUser`)**:

  * The `@WithAnonymousUser` annotation is again used to simulate an anonymous user.

  * When performing a request to `/securedPage`, it should return an HTTP status of `401 Unauthorized` because this page is protected and requires authentication (or at least a role of `USER`).

## When to Use `@WithAnonymousUser`

You should use `@WithAnonymousUser` when:

* You want to test the behavior of your application for **unauthenticated** or **anonymous** users.

* You have endpoints that should be accessible to users without authentication (e.g., public pages, login pages).

* You need to verify that access to protected resources is properly restricted to authenticated users or those with specific roles/permissions.

## Combining with Other Annotations

* **`@WithMockUser`**: While `@WithAnonymousUser` simulates an unauthenticated user, `@WithMockUser` simulates an authenticated user with specific roles or authorities. You can use both in separate tests to verify the correct behavior for authenticated and unauthenticated users.

```java
@Test
@WithAnonymousUser
public void testAnonymousUserAccess() {
    // Test anonymous user access
}

@Test
@WithMockUser(username = "admin", roles = "ADMIN")
public void testAuthenticatedUserAccess() {
    // Test authenticated user access with roles
}
```

## Clearing Security Context

In some cases, you might want to clear the `SecurityContext` between tests to ensure that there is no residual state from previous tests. This can be done using `SecurityContextHolder.clearContext()`.

```java
SecurityContextHolder.clearContext();  // Clears the SecurityContext between tests
```

This is especially important when you are using `@WithAnonymousUser` and `@WithMockUser` annotations in different tests to prevent cross-test contamination.

## Limitations of `@WithAnonymousUser`

While `@WithAnonymousUser` is convenient for mocking anonymous users, there are some limitations:

* **No custom authentication**: Since it simulates an unauthenticated user, you can't specify custom user details, roles, or permissions.

* **Cannot be used for authenticated users**: If you need to simulate authenticated users with specific roles or permissions, you'll need to use annotations like `@WithMockUser` or manually configure authentication in the test.

## Conclusion

`@WithAnonymousUser` is a useful tool for testing scenarios where you need to simulate an anonymous user (i.e., an unauthenticated user) in your Spring Security-based tests. By using this annotation, you can ensure that certain public endpoints are accessible without authentication, and that protected resources are properly secured and inaccessible to anonymous users.

Key points:

* Simulate anonymous users in tests.

* Useful for verifying the behavior of public endpoints and ensuring proper security configuration.

* Works well in conjunction with other Spring Security annotations like `@WithMockUser` for testing authenticated users.

