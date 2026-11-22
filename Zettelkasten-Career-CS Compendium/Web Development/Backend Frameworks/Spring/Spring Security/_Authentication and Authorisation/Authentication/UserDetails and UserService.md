---
tags:
  - FORMAT
  - Spring
---
In Spring Security, the `UserDetails` and `UserDetailsService` interfaces play a key role in managing user authentication. They are essential for representing user information and retrieving user details from a data source such as a database, LDAP server, or external API.

# UserDetails

## Overview

* The `UserDetails` interface represents a user in Spring Security.  
* It contains user-related information like username, password, and authorities (roles or permissions).  
* It also provides additional user state information such as account status, lock status, and credentials expiration.

## Key Methods in UserDetails

| Method | Description |
| ----- | ----- |
| `getUsername()` | Returns the username of the user. |
| `getPassword()` | Returns the encrypted password of the user. |
| `getAuthorities()` | Returns the roles or permissions assigned to the user as a collection of `GrantedAuthority`. |
| `isAccountNonExpired()` | Checks if the user account has expired. |
| `isAccountNonLocked()` | Checks if the user account is locked. |
| `isCredentialsNonExpired()` | Checks if the user’s credentials (password) have expired. |
| `isEnabled()` | Checks if the user is enabled or disabled. |

## Example Implementation of UserDetails

Here is a simple example of how `UserDetails` can be implemented:

```java
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.stream.Collectors;

public class CustomUserDetails implements UserDetails {

    private String username;
    private String password;
    private boolean isActive;
    private Collection<? extends GrantedAuthority> authorities;

    public CustomUserDetails(User user) {
        this.username = user.getUsername();
        this.password = user.getPassword();
        this.isActive = user.isActive();
        this.authorities = user.getRoles().stream()
                .map(role -> new SimpleGrantedAuthority(role.getName()))
                .collect(Collectors.toList());
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return isActive;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return isActive;
    }
}
```

* This class takes a `User` entity from the database and adapts it to Spring Security's `UserDetails`.

---

# UserDetailsService

## Overview

* The `UserDetailsService` interface is responsible for fetching user details from a data source during authentication.  
* It is a single-method interface that Spring Security uses to load user information by username.

## Method in UserDetailsService

| Method | Description |
| ----- | ----- |
| `UserDetails loadUserByUsername(String username)` | Loads user data from the database using the provided username and returns a `UserDetails` object. |

## Example Implementation of UserDetailsService

Here is an example of how `UserDetailsService` can be implemented using a `UserRepository`:

```java
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    private final UserRepository userRepository;

    public CustomUserDetailsService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("User not found with username: " + username));
        return new CustomUserDetails(user);
    }
}
```

## Explanation:

* **`UserRepository`**: A simple Spring Data JPA repository to fetch user data from the database.  
* **`UsernameNotFoundException`**: Thrown if no user is found for the given username.  
* **`CustomUserDetails`**: The `UserDetails` implementation that wraps the `User` object.

---

# Integration in Spring Security

To use `UserDetailsService`, you can configure Spring Security like this:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import static org.springframework.security.config.Customizer.withDefaults;

@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/login", "/register").permitAll()
            .anyRequest().authenticated()
        )
        .formLogin(withDefaults());
    return http.build();
}

@Bean
public AuthenticationManager authManager(HttpSecurity http, CustomUserDetailsService userDetailsService)
        throws Exception {
    return http.getSharedObject(AuthenticationManagerBuilder.class)
            .userDetailsService(userDetailsService)
            .passwordEncoder(passwordEncoder())
            .and()
            .build();
}

@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

* **Password Encoder**: `BCryptPasswordEncoder` is commonly used to securely hash passwords.  
* **Form Login**: Provides a default login form for authentication.

---

# Conclusion

* **UserDetails** represents user information for Spring Security, including username, password, and roles.  
* **UserDetailsService** acts as the bridge between Spring Security and the application's data source, fetching user information by username during authentication.  
* Together, they form the foundation for managing authentication in Spring Security, ensuring that only valid users are granted access to secured resources.

