---
tags:
  - FORMAT
  - Spring
---
Spring Security provides a flexible authentication mechanism through the `AuthenticationProvider` interface. While it comes with built-in providers for common scenarios like username-password authentication, it also allows you to create **custom authentication providers** to support additional authentication mechanisms.

A **custom authentication provider** is useful when:

* You need to authenticate users using a custom database or external service.  
* You are implementing token-based authentication (e.g., JWT or OAuth2).  
* You have specific business logic that requires custom validation.

---

# Components Involved in Custom Authentication

1. **Authentication Object**: Represents the authentication request.  
   * Before authentication: Contains credentials like username and password.  
   * After authentication: Contains user details and granted authorities.  
2. **AuthenticationProvider**: Implements the authentication logic.  
   * Responsible for validating the credentials.  
   * Returns an authenticated `Authentication` object on success.  
3. **UserDetailsService**: Loads user-specific data like username, password, and roles from the database or external system.  
4. **PasswordEncoder**: Encrypts and compares passwords securely.

---

# Steps to Create a Custom AuthenticationProvider

## Step 1: Implement the AuthenticationProvider Interface

Here’s a simple example of a custom authentication provider for username-password authentication using Spring Security.

```java
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Component;

@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {

    private final UserDetailsService userDetailsService;
    private final PasswordEncoder passwordEncoder;

    public CustomAuthenticationProvider(UserDetailsService userDetailsService, PasswordEncoder passwordEncoder) {
        this.userDetailsService = userDetailsService;
        this.passwordEncoder = passwordEncoder;
    }

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        // Extract credentials from the Authentication object
        String username = authentication.getName();
        String password = authentication.getCredentials().toString();

        // Load user details using UserDetailsService
        UserDetails userDetails = userDetailsService.loadUserByUsername(username);

        // Verify the password using PasswordEncoder
        if (!passwordEncoder.matches(password, userDetails.getPassword())) {
            throw new AuthenticationException("Invalid credentials") {};
        }

        // Return a successful authentication token
        return new UsernamePasswordAuthenticationToken(userDetails, password, userDetails.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        // Specify the type of authentication this provider supports
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

## Explanation:

* **UserDetailsService**: Fetches user information from a database.  
* **PasswordEncoder**: Compares the provided password with the stored hashed password.  
* **UsernamePasswordAuthenticationToken**: Represents an authenticated user if successful.  
* **supports()**: Ensures the provider only handles `UsernamePasswordAuthenticationToken`.

---

# Step 2: Configure the AuthenticationManager

To register the custom authentication provider, configure it using Spring Security:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Component;

@Component
public class SecurityConfig {

    private final AuthenticationProvider customAuthenticationProvider;

    public SecurityConfig(CustomAuthenticationProvider customAuthenticationProvider) {
        this.customAuthenticationProvider = customAuthenticationProvider;
    }

    @Bean
    public AuthenticationManager authManager(AuthenticationManagerBuilder authBuilder) throws Exception {
        return authBuilder
                .authenticationProvider(customAuthenticationProvider)
                .build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

## Explanation:

* **AuthenticationManager**: Manages authentication by delegating to the `CustomAuthenticationProvider`.  
* **PasswordEncoder**: Ensures secure password management using `BCryptPasswordEncoder`.

---

# Step 3: Handle Authentication Exceptions

If authentication fails, Spring Security will throw an `AuthenticationException`. You can customize how exceptions are handled by adding a custom `AuthenticationEntryPoint`:

```java
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.stereotype.Component;
import java.io.IOException;

@Component
public class CustomAuthenticationEntryPoint implements AuthenticationEntryPoint {

    @Override
    public void commence(HttpServletRequest request,
                          HttpServletResponse response,
                          AuthenticationException authException) throws IOException {
        response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Unauthorized: " + authException.getMessage());
    }
}
```

* **CustomAuthenticationEntryPoint**: Provides a detailed error response for failed authentication attempts.

---

# When to Use a Custom AuthenticationProvider

* You are integrating with an external identity provider or API for authentication.  
* You need to implement two-factor or multi-factor authentication (2FA/MFA).  
* Your application uses custom tokens for authentication (e.g., JWT, API keys).  
* You have complex business logic that goes beyond simple username-password authentication.

---

# Conclusion

* **Custom AuthenticationProviders** allow Spring Security to support additional or custom authentication mechanisms.  
* Implement the `AuthenticationProvider` interface to define your own authentication logic.  
* Integrate it with `AuthenticationManager` to manage authentication requests.  
* Spring Security’s flexibility makes it easy to adapt to various authentication requirements using this approach.

