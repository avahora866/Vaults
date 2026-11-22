---
tags:
  - FORMAT
  - Spring
---
In Spring Security, the **Security Filter Chain** is a crucial component that handles security-related tasks for incoming HTTP requests. It is essentially a chain of filters that process requests before they reach your application's controllers or endpoints. Each filter in the chain is responsible for a specific security task, such as authentication, authorization, CSRF protection, etc.

---

# What is the Security Filter Chain?
- The Security Filter Chain is a sequence of **Servlet Filters** that are applied to every incoming HTTP request.
- These filters are responsible for enforcing security rules, such as checking if a user is authenticated, authorized, or if the request is valid (e.g., CSRF token validation).
- The chain ensures that requests are processed in a specific order, and each filter can either allow the request to proceed to the next filter or block it.

---

# How is the Security Filter Chain Configured?
- In Spring Security, the filter chain is configured using the `HttpSecurity` class, typically in a class that extends `WebSecurityConfigurerAdapter` (in older versions) or by defining a `SecurityFilterChain` bean (in newer versions, starting with Spring Security 5.7+).
- Example of configuring the filter chain in a `SecurityFilterChain` bean:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/public/**").permitAll()
            .requestMatchers("/admin/**").hasRole("ADMIN")
            .anyRequest().authenticated()
        )
        .formLogin(form -> form
            .loginPage("/login")
            .permitAll()
        )
        .logout(logout -> logout
            .permitAll()
        )
        .csrf(csrf -> csrf.disable()); // Example: Disabling CSRF protection
    return http.build();
}
```

---

# Key Filters in the Security Filter Chain
Spring Security's filter chain consists of multiple filters, each with a specific responsibility. Some of the most important filters include:

1. **`ChannelProcessingFilter`**: Ensures requests are served over the correct channel (e.g., HTTP vs HTTPS).
2. **`SecurityContextPersistenceFilter`**: Restores the `SecurityContext` (authentication information) from the session for each request.
3. **`UsernamePasswordAuthenticationFilter`**: Handles form-based login authentication.
4. **`BasicAuthenticationFilter`**: Handles HTTP Basic Authentication.
5. **`FilterSecurityInterceptor`**: Enforces authorization rules (e.g., checking if a user has the required roles to access a resource).
6. **`ExceptionTranslationFilter`**: Handles security-related exceptions (e.g., redirecting to a login page if authentication is required).
7. **`CsrfFilter`**: Protects against Cross-Site Request Forgery (CSRF) attacks.
8. **`AnonymousAuthenticationFilter`**: Assigns an anonymous authentication token if the user is not authenticated.
9. **`SessionManagementFilter`**: Manages user sessions and session fixation protection.
10. **`LogoutFilter`**: Handles logout requests.

---

# Order of Filters
- Filters in the Security Filter Chain are executed in a specific order. For example:
  1. `ChannelProcessingFilter`
  2. `SecurityContextPersistenceFilter`
  3. `UsernamePasswordAuthenticationFilter`
  4. `BasicAuthenticationFilter`
  5. `CsrfFilter`
  6. `FilterSecurityInterceptor`
  7. `ExceptionTranslationFilter`
  8. `AnonymousAuthenticationFilter`
  9. `SessionManagementFilter`
  10. `LogoutFilter`

- The order is important because some filters depend on the work done by previous filters (e.g., `FilterSecurityInterceptor` relies on the authentication information set by `UsernamePasswordAuthenticationFilter`).

---

# Customizing the Security Filter Chain
You can customize the Security Filter Chain by:
- Adding or removing filters.
- Changing the order of filters.
- Adding custom filters.

Example of adding a custom filter:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .addFilterBefore(new CustomFilter(), UsernamePasswordAuthenticationFilter.class)
        .authorizeHttpRequests(auth -> auth
            .anyRequest().authenticated()
        );
    return http.build();
}
```

In this example, `CustomFilter` will be executed before the `UsernamePasswordAuthenticationFilter`.

---

# How the Security Filter Chain Works in Practice
1. When a request arrives, it is passed through the Security Filter Chain.
2. Each filter processes the request in sequence:
   - If a filter determines the request is invalid or unauthorized, it can block the request (e.g., by throwing an exception or redirecting the user).
   - If the request passes all filters, it reaches the application's controller or endpoint.
3. After the request is processed by the application, the response is sent back through the filter chain in reverse order.

---

# Debugging the Security Filter Chain
If you want to see the filters in the Security Filter Chain, you can enable debug logging for Spring Security:

```properties
logging.level.org.springframework.security=DEBUG
```

This will log details about the filters and their execution order.

---

# Summary
The **Security Filter Chain** in Spring Security is a powerful mechanism for enforcing security rules in your application. It consists of a series of filters that handle tasks like authentication, authorization, CSRF protection, and more. By understanding and customizing the filter chain, you can tailor Spring Security to meet your application's specific security requirements.

> Note: If you want to implement JWT into your application you will need a custom filter that extracts the JWT, validates it, and sets the authentication in the `SecurityContext`