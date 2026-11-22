---
tags:
  - FORMAT
  - Spring
---
Spring Security uses **URL-based authorization** to control access to specific endpoints based on user roles or authorities. It allows you to secure parts of your application by defining rules for different URL patterns using the `HttpSecurity` configuration.

---

# Understanding URL-Based Authorization

**URL-Based Authorization** means specifying which users (based on their roles or authorities) can access certain endpoints. This is often done using methods like:

* `hasRole()`  
* `hasAuthority()`  
* `hasAnyRole()`  
* `hasAnyAuthority()`  
* `permitAll()`  
* `denyAll()`  
* `authenticated()`  
* `anonymous()`

---

# Configuring HttpSecurity for URL Authorization

You can configure URL-based authorization using the `HttpSecurity` object in a Spring Security configuration class.

## Basic Example

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/admin/**").hasRole("ADMIN")           // Only accessible to ADMIN
            .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN") // Accessible to USER or ADMIN
            .requestMatchers("/public/**").permitAll()               // Accessible to everyone
            .anyRequest().authenticated()                           // All other requests require authentication
        )
        .formLogin(withDefaults())
        .logout(logout -> logout.logoutUrl("/logout"));
    return http.build();
}
```

## Explanation:

1. **requestMatchers("/admin/**")\*\*: Restricts all URLs under `/admin/` to users with the `ROLE_ADMIN`.  
2. **hasRole()**: Checks for a specific role (Spring automatically adds `"ROLE_"` as a prefix to role names).  
3. **hasAnyRole()**: Allows multiple roles to access the endpoint.  
4. **permitAll()**: Grants access to all users, including unauthenticated ones.  
5. **authenticated()**: Ensures the user is authenticated.  
6. **formLogin()**: Enables default form-based login.  
7. **logout()**: Configures a logout endpoint.

---

# Using Authorities Instead of Roles

If you prefer using **authorities** for finer-grained control, you can use `hasAuthority()` instead of `hasRole()`.

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/reports/**").hasAuthority("READ_PRIVILEGES")
    .requestMatchers("/edit/**").hasAuthority("WRITE_PRIVILEGES")
    .anyRequest().authenticated()
);
```

* **hasAuthority("READ\_PRIVILEGES")**: Grants access if the user has the specific authority.  
* **hasAnyAuthority("READ\_PRIVILEGES", "WRITE\_PRIVILEGES")**: Grants access if the user has at least one of the specified authorities.

---

# Combining URL Authorization Rules

You can combine multiple rules using logical operations like `and()` or apply different rules for different paths.

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/admin/**").hasRole("ADMIN")
    .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
    .requestMatchers("/reports/**").hasAuthority("READ_PRIVILEGES")
    .requestMatchers("/login", "/register", "/home").permitAll()
    .anyRequest().authenticated()
);
```

* Multiple rules ensure different sections of the app are properly secured.  
* `anyRequest().authenticated()` ensures all unspecified endpoints require authentication.

---

# Allowing or Denying All Access

* **permitAll()**: Allows unrestricted access.  
* **denyAll()**: Blocks access for all users.

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/health").permitAll()  // Open access to health checks
    .requestMatchers("/admin/**").denyAll()  // No access for anyone
);
```

---

# Securing Specific HTTP Methods

You can apply different authorization rules based on the HTTP method (GET, POST, PUT, DELETE).

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers(HttpMethod.GET, "/products/**").permitAll()            // Allow all GET requests
    .requestMatchers(HttpMethod.POST, "/products/**").hasRole("ADMIN")      // Only ADMIN can POST
    .requestMatchers(HttpMethod.DELETE, "/products/**").hasAuthority("DELETE_PRIVILEGES")
    .anyRequest().authenticated()
);
```

* **HttpMethod.GET**: Applies rules specifically for GET requests.  
* **HttpMethod.POST/DELETE**: Limits specific methods to certain roles or authorities.

---

# Customizing Access Denied Handling

If unauthorized users try to access a protected resource, Spring Security provides an error response by default. You can customize this behavior using an `AccessDeniedHandler`.

```java
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.web.access.AccessDeniedHandler;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CustomAccessDeniedHandler implements AccessDeniedHandler {

    @Override
    public void handle(HttpServletRequest request,
                        HttpServletResponse response,
                        AccessDeniedException accessDeniedException) throws IOException {
        response.sendError(HttpServletResponse.SC_FORBIDDEN, "Access Denied: " + accessDeniedException.getMessage());
    }
}
```

Then configure it in your `HttpSecurity`:

```java
http.exceptionHandling(eh -> eh.accessDeniedHandler(new CustomAccessDeniedHandler()));
```

---

# Conclusion

* **URL-Based Authorization** with Spring Security is an effective way to control access to resources using `HttpSecurity`.  
* You can use `hasRole()` and `hasAuthority()` to enforce security rules based on roles and permissions.  
* `permitAll()`, `denyAll()`, and `authenticated()` provide broad control over public and private endpoints.  
* Fine-tune your security by restricting access based on HTTP methods.  
* Customize access denial responses using a custom `AccessDeniedHandler`.

With these capabilities, you can build a secure and well-structured authentication and authorization system for your Spring Boot application.

