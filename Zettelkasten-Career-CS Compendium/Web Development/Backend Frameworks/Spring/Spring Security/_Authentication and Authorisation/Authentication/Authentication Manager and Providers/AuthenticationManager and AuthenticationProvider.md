---
tags:
  - FORMAT
  - Spring
---
In Spring Security, authentication is a crucial process that verifies the identity of users trying to access an application. Two key components responsible for managing authentication **when logging in** are **AuthenticationManager** and **AuthenticationProvider**.

The **`AuthenticationManager`** and **`AuthenticationProvider`** are **only used during the initial login process**. When a user logs in for the first time, they provide their credentials (e.g., username and password), and the `AuthenticationManager` validates these credentials. If the authentication is successful, the server generates a **token (e.g., JWT)** or a **session ID** and returns it to the user.

Once the user has logged in and received the token or session ID, they **do not need to authenticate with their username and password again** for subsequent requests. Instead, they include the token in the `Authorization` header (e.g., `Bearer <JWT>`) or provide the session ID (if using session-based authentication).

In future requests, the server validates the token or session ID directly, bypassing the `AuthenticationManager` and `AuthenticationProvider`. These components are **not involved** in processing subsequent requests because the token or session ID serves as proof of the user's authenticated state.
# AuthenticationManager

## Overview

* The `AuthenticationManager` interface is the central point for handling authentication requests in Spring Security.  
* It is responsible for validating user credentials and deciding whether authentication is successful or not.  
* It delegates the actual authentication to one or more `AuthenticationProvider` implementations.

## Key Characteristics

* Supports multiple authentication mechanisms (e.g., username/password, OAuth2, JWT, LDAP).  
* Typically configured using Spring's `HttpSecurity` or custom configurations.  
* Returns a fully authenticated `Authentication` object upon success.  
* Throws an `AuthenticationException` if authentication fails.

## Method in AuthenticationManager

| Method | Description |
| ----- | ----- |
| `Authentication authenticate(Authentication authentication)` | Attempts to authenticate the given `Authentication` object by delegating to appropriate `AuthenticationProvider`. |

## Example of AuthenticationManager

Spring Security often uses `ProviderManager`, which is the default implementation of `AuthenticationManager`:

```java
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.ProviderManager;
import java.util.Arrays;

public class CustomAuthenticationManager implements AuthenticationManager {

    private final AuthenticationManager manager;

    public CustomAuthenticationManager(AuthenticationProvider... providers) {
        this.manager = new ProviderManager(Arrays.asList(providers));
    }

    @Override
    public Authentication authenticate(Authentication authentication) {
        return manager.authenticate(authentication);
    }
}
```

* `ProviderManager` delegates authentication to a list of `AuthenticationProvider` instances.  
* If no provider supports the authentication, an exception is thrown.

---

# AuthenticationProvider

## Overview

* `AuthenticationProvider` is responsible for validating user credentials against specific authentication mechanisms.  
* It performs the actual authentication logic like checking usernames, passwords, or validating tokens.  
* It works independently, allowing multiple providers to be configured for different authentication strategies.

## Key Characteristics

* Supports custom authentication logic by implementing the `AuthenticationProvider` interface.  
* Can validate credentials from various sources such as databases, LDAP servers, or third-party services.  
* Returns an authenticated `Authentication` object if credentials are valid.  
* Returns `null` if it cannot authenticate, allowing other providers to try.

## Key Methods in AuthenticationProvider

| Method | Description |
| ----- | ----- |
| `Authentication authenticate(Authentication authentication)` | Performs the authentication logic. |
| `boolean supports(Class<?> authentication)` | Returns `true` if the provider supports the authentication type. |

## Example of Custom AuthenticationProvider

Here’s an example of how to create a custom authentication provider for username-password authentication:

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
        String username = authentication.getName();
        String password = authentication.getCredentials().toString();

        UserDetails userDetails = userDetailsService.loadUserByUsername(username);

        if (!passwordEncoder.matches(password, userDetails.getPassword())) {
            throw new AuthenticationException("Invalid credentials") {};
        }

        return new UsernamePasswordAuthenticationToken(username, password, userDetails.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

## Explanation:

* **UserDetailsService**: Fetches user data from a data source (e.g., database).  
* **PasswordEncoder**: Verifies if the input password matches the stored encrypted password.  
* **UsernamePasswordAuthenticationToken**: Represents a successful authentication with user details and authorities.  
* **supports()**: Ensures the provider handles username-password authentication.

---

# Authentication Flow in Spring Security

Here is how Spring Security’s authentication flow typically works:

1. **Client Request**: A user submits login credentials (e.g., username and password).  
2. **Filter Interception**: The `UsernamePasswordAuthenticationFilter` intercepts the request and creates an `Authentication` object.  
3. **AuthenticationManager**: The filter passes the authentication request to the `AuthenticationManager`.  
4. **AuthenticationProvider Delegation**: The `AuthenticationManager` delegates to one or more `AuthenticationProvider` instances to validate the credentials.  
5. **User Validation**: The provider uses `UserDetailsService` to retrieve user details and validates credentials using a `PasswordEncoder`.  
6. **Authentication Success or Failure**:  
   * On success: The provider returns an authenticated `Authentication` object, which is stored in the `SecurityContext`.  
   * On failure: An `AuthenticationException` is thrown, which can be handled by an `AuthenticationEntryPoint` or an error handler.

---

# Conclusion

* **AuthenticationManager**: Serves as the entry point for authentication requests, delegating them to appropriate providers.  
* **AuthenticationProvider**: Contains the logic to validate user credentials against a data source or service.  
* Together, these components form the backbone of Spring Security’s authentication process, providing flexibility to implement various authentication mechanisms.

This architecture allows developers to customize authentication logic, integrate with external systems, and ensure secure user authentication in Spring-based applications.

