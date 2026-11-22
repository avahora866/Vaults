---
tags:
  - FORMAT
  - Spring
---
Spring Security provides **Role-Based** and **Authority-Based** access control to manage user permissions within an application. These controls ensure that users can only access resources or perform actions for which they have the necessary permissions.

---

# Roles

* **Roles** are a high-level concept used to define a user's permissions within an application.  
* They often represent the user's function, like `ROLE_ADMIN`, `ROLE_USER`, or `ROLE_MANAGER`.  
* Roles are typically prefixed with `"ROLE_"` in Spring Security.

# Authorities

* **Authorities** are more granular permissions assigned to users, like `READ_PRIVILEGES`, `WRITE_PRIVILEGES`, or `DELETE_PRIVILEGES`.  
* They provide fine-grained control over what actions a user can perform.  
* Unlike roles, authorities don’t need the `"ROLE_"` prefix.

# Key Difference

* **Roles**: Broad, application-level access control (e.g., `ROLE_ADMIN`).  
* **Authorities**: Specific actions or permissions (e.g., `READ_PRIVILEGES`, `WRITE_PRIVILEGES`).

---

# Defining Roles and Authorities

In Spring Security, roles and authorities are represented using the `GrantedAuthority` interface. Typically, `SimpleGrantedAuthority` is used to store both roles and authorities.

## Example of Assigning Roles and Authorities

```java
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.List;

public class CustomUserDetails implements UserDetails {

    private String username;
    private String password;
    private boolean isActive;
    private List<String> roles;         // e.g., ["ROLE_USER", "ROLE_ADMIN"]
    private List<String> authorities;   // e.g., ["READ_PRIVILEGES", "WRITE_PRIVILEGES"]

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        List<GrantedAuthority> grantedAuthorities = roles.stream()
                .map(SimpleGrantedAuthority::new)
                .toList();
        grantedAuthorities.addAll(authorities.stream()
                .map(SimpleGrantedAuthority::new)
                .toList());
        return grantedAuthorities;
    }

    @Override
    public String getPassword() { return password; }

    @Override
    public String getUsername() { return username; }

    @Override
    public boolean isAccountNonExpired() { return true; }

    @Override
    public boolean isAccountNonLocked() { return isActive; }

    @Override
    public boolean isCredentialsNonExpired() { return true; }

    @Override
    public boolean isEnabled() { return isActive; }
}
```

* **Roles**: Prefixed with `"ROLE_"` to indicate high-level access.  
* **Authorities**: Represent specific permissions.

---

# Configuring Role-Based Access Control

## Step 1: Define Access Rules in Security Configuration

You can configure Spring Security to restrict access to different endpoints using roles or authorities.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/admin/**").hasRole("ADMIN")      // Requires ROLE_ADMIN
            .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
            .requestMatchers("/reports/**").hasAuthority("READ_PRIVILEGES")
            .anyRequest().authenticated()
        )
        .formLogin(withDefaults());
    return http.build();
}
```

### Explanation:

* **hasRole("ADMIN")**: Allows only users with `ROLE_ADMIN`.  
* **hasAnyRole("USER", "ADMIN")**: Allows both `ROLE_USER` and `ROLE_ADMIN`.  
* **hasAuthority("READ\_PRIVILEGES")**: Grants access if the user has the specific authority.

---

# Configuring Authority-Based Access Control

If you want to use more granular access control using authorities, you can replace `hasRole` with `hasAuthority`.

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/edit/**").hasAuthority("WRITE_PRIVILEGES")
    .requestMatchers("/view/**").hasAuthority("READ_PRIVILEGES")
    .anyRequest().authenticated()
);
```

* **Authorities** are not prefixed with `"ROLE_"` and are applied for specific actions.  
* You can combine multiple authorities using `.hasAnyAuthority("READ_PRIVILEGES", "WRITE_PRIVILEGES")`.

---

# Using Method-Level Security

Spring Security also allows restricting access at the method level using annotations.

## Enable Method Security

```java
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;

@EnableMethodSecurity
public class SecurityConfig {}
```

## Use Annotations for Role and Authority Control

* **@PreAuthorize**: Evaluates the provided expression before the method executes.  
* **@PostAuthorize**: Evaluates after method execution (useful for checking return values).  
* **@Secured**: Provides simple role-based security.

### Example with Roles

```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Service;

@Service
public class AdminService {

    @PreAuthorize("hasRole('ADMIN')")
    public void performAdminTask() {
        System.out.println("Admin task executed.");
    }
}
```

### Example with Authorities

```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Service;

@Service
public class ReportService {

    @PreAuthorize("hasAuthority('READ_PRIVILEGES')")
    public String generateReport() {
        return "Report generated.";
    }
}
```

---

# Combining Roles and Authorities

You can combine both roles and authorities using logical operators in annotations:

```java
@PreAuthorize("hasRole('ADMIN') or hasAuthority('MANAGE_PRIVILEGES')")
public void manageResources() {
    System.out.println("Resource management allowed.");
}
```

* `hasRole()` checks for high-level role-based access.  
* `hasAuthority()` checks for fine-grained permissions.

---

# Conclusion

* **Role-Based Access Control (RBAC)**: Suitable for managing broad access across different user roles like Admin, User, or Manager.  
* **Authority-Based Access Control (ABAC)**: Provides detailed control over specific actions using permissions like `READ_PRIVILEGES` or `WRITE_PRIVILEGES`.  
* **Spring Security** makes it easy to implement both types using annotations, configuration, or method-level security.

By combining both approaches, you can create a secure and flexible access control system tailored to your application’s needs.

