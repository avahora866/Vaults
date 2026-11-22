---
tags:
  - FORMAT
  - Spring
---
Spring Security provides a way to manage access control at a more granular level—**at the method level**—using annotations like `@PreAuthorize` and `@Secured`. This allows you to define authorization rules for specific methods rather than entire HTTP requests. This fine-grained access control is useful when you want to restrict access to individual business operations, regardless of the URL or HTTP request.

---

# Enabling Method-Level Security

Before using annotations like `@PreAuthorize` or `@Secured`, you must enable method-level security in your Spring Security configuration. This is done by annotating your configuration class with `@EnableMethodSecurity`.

## Example:

```java
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableMethodSecurity
public class SecurityConfig {
    // Other security configurations
}
```

`@EnableMethodSecurity` enables support for method-level security annotations, allowing Spring Security to intercept method calls and evaluate access control rules.

---

# Method-Level Security Annotations

## `@PreAuthorize`

The `@PreAuthorize` annotation allows you to specify an **SpEL (Spring Expression Language)** expression that will be evaluated before the method is executed. The expression can check the user's roles, authorities, or any other condition.

### Example Usage:

```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    // Only users with 'ROLE_ADMIN' can execute this method
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        System.out.println("Deleting user with ID: " + userId);
    }

    // Only users with 'WRITE_PRIVILEGES' authority can execute this method
    @PreAuthorize("hasAuthority('WRITE_PRIVILEGES')")
    public void updateUser(Long userId, String userData) {
        System.out.println("Updating user with ID: " + userId);
    }

    // Allows both 'ROLE_ADMIN' and 'ROLE_MANAGER' to execute this method
    @PreAuthorize("hasAnyRole('ADMIN', 'MANAGER')")
    public void viewSensitiveData() {
        System.out.println("Viewing sensitive data...");
    }
}
```

### Key Points:

* **SpEL Expressions**: The annotations support Spring Expression Language (SpEL), which lets you access security details and roles dynamically.  
* **hasRole() and hasAuthority()**: Used to check the user's roles or authorities. `hasRole()` is typically used for roles (which are prefixed with "ROLE\_"), and `hasAuthority()` is used for granular permissions like `READ_PRIVILEGES` or `WRITE_PRIVILEGES`.  
* **hasAnyRole()**: Allows access if any of the specified roles are granted.  
* **hasAnyAuthority()**: Allows access if the user has any of the specified authorities.

---

## `@Secured`

The `@Secured` annotation is simpler than `@PreAuthorize` and checks if a user has a specific role. It is used to restrict access to methods based on roles.

### Example Usage:

```java
import org.springframework.security.access.annotation.Secured;
import org.springframework.stereotype.Service;

@Service
public class AdminService {

    // Only users with 'ROLE_ADMIN' can access this method
    @Secured("ROLE_ADMIN")
    public void performAdminTask() {
        System.out.println("Admin task performed.");
    }

    // Multiple roles can access this method
    @Secured({"ROLE_ADMIN", "ROLE_MANAGER"})
    public void performManagerTask() {
        System.out.println("Manager task performed.");
    }
}
```

### Key Points:

* **Roles Only**: Unlike `@PreAuthorize`, `@Secured` works only with roles, not authorities.  
* **Simplicity**: It’s more concise and easier to use but less flexible than `@PreAuthorize` because it doesn’t support SpEL expressions.

---

# Combining Annotations for Fine-Grained Control

You can combine `@PreAuthorize` and `@Secured` in the same application, depending on your needs. Typically, `@PreAuthorize` is preferred for more complex or dynamic conditions, while `@Secured` works well for simple role-based access control.

## Example Combining Both

```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.security.access.annotation.Secured;
import org.springframework.stereotype.Service;

@Service
public class DocumentService {

    // Allows 'ROLE_ADMIN' and 'ROLE_MANAGER' to delete documents
    @Secured({"ROLE_ADMIN", "ROLE_MANAGER"})
    public void deleteDocument(Long documentId) {
        System.out.println("Deleting document with ID: " + documentId);
    }

    // Allows users with 'WRITE_PRIVILEGES' authority to update documents
    @PreAuthorize("hasAuthority('WRITE_PRIVILEGES')")
    public void updateDocument(Long documentId, String content) {
        System.out.println("Updating document with ID: " + documentId);
    }

    // Allows only users who are the owner of the document
   @PreAuthorize("hasPermission(#documentId, 'DOCUMENT', 'DELETE')")
    public void deleteOwnedDocument(Long documentId) {
        System.out.println("Deleting document owned by the user...");
    }
}
```

* `@Secured` checks if the user has specific roles.  
* `@PreAuthorize` allows more complex access control using SpEL expressions.  
* You can also use **custom permissions** with `hasPermission()` for more advanced scenarios (e.g., object-level security).

---

# More Advanced Use Cases

## Object-Level Security

You can secure methods based on the user's permissions on specific objects using `hasPermission()`:

```java
PreAuthorize("hasPermission(#document, 'WRITE_PRIVILEGES')")
public void updateDocument(Document document) {
    System.out.println("Document updated.");
}
```

\#document refers to the method parameter.  
* **'WRITE\_PRIVILEGES'** checks if the user has permission to perform a certain action on the document object.

---

## Combining Method-Level Security with URL-Based Security

Method-level security complements URL-based security. You can define broader security rules using `HttpSecurity` (like URL-based access) and fine-tune access control at the method level.

Example:

```java
// URL-based security
http.authorizeHttpRequests()
    .requestMatchers("/admin/**").hasRole("ADMIN")
    .anyRequest().authenticated();

// Method-level security
@Service
public class AdminService {
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        System.out.println("Deleting user with ID: " + userId);
    }
}
```

* **URL-based security** handles access control for endpoints.  
* **Method-level security** gives you finer control over specific business methods.

---

# Conclusion

* **Method-level security** allows you to define authorization rules for individual methods using annotations like `@PreAuthorize` and `@Secured`.  
* **`@PreAuthorize`** supports complex, dynamic conditions using SpEL, making it more flexible and powerful.  
* **`@Secured`** is simpler and used for role-based access control, with less flexibility than `@PreAuthorize`.  
* Combining **method-level** and **URL-based** security gives you complete control over access to your application’s endpoints and business logic.

This approach provides a high level of granularity and flexibility, allowing you to manage access control effectively at both the endpoint and method levels in your application.

