---
tags:
  - FORMAT
  - Spring
---
**In-memory authentication** is a simple authentication method suitable for testing, prototyping, or small applications where user credentials don't need to be stored in a database or external system. With in-memory authentication, user details (such as usernames, passwords, roles, and authorities) are stored directly within the application's memory using the `InMemoryUserDetailsManager`.

This method is easy to set up and can be useful during the initial stages of development or when you need a quick way to test security functionality without integrating with a full authentication backend.

---

# Setting Up In-Memory Authentication

Spring Security provides the `InMemoryUserDetailsManager` class to handle user authentication with data stored in memory. You can configure this in your Spring Security configuration class to define users, their roles, and passwords.

## Example Configuration for In-Memory Authentication

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            .and()
            .formLogin()
                .permitAll();
    }

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();

        // Defining users with roles
        manager.createUser(User.withUsername("admin")
                .password("{noop}adminpassword") // No encoding for simplicity
                .roles("ADMIN")
                .build());
        
        manager.createUser(User.withUsername("user")
                .password("{noop}userpassword") // No encoding for simplicity
                .roles("USER")
                .build());

        return manager;
    }
}
```

---

# Key Points of In-Memory Authentication

## UserDetailsService

* The `UserDetailsService` interface is used to load user-specific data, and in the case of in-memory authentication, we provide an implementation via `InMemoryUserDetailsManager`.  
* The `InMemoryUserDetailsManager` stores user details (like username, password, roles, and authorities) in memory for quick access.

## User Creation

* Users are created using `User.withUsername()`, where you specify the username, password (you can use `{noop}` to disable password encoding for testing purposes), and roles.  
* The `roles()` method assigns roles to the user, which are prefixed with "ROLE\_" by default when checked for authorization.

## Password Encoding

* In this example, `{noop}` is used to indicate that the password is stored in plain text, which is fine for development or testing but should **never** be used in a production environment.  
* In production, you should always use proper password encoding with `BCryptPasswordEncoder` or another encoder.

## Access Control

* You can define which URLs are accessible based on the roles assigned to users. In this example, users with the `ROLE_ADMIN` can access `/admin/**` URLs, and users with the `ROLE_USER` can access `/user/**` URLs.  
* Other requests require the user to be authenticated.

---

# Advantages of In-Memory Authentication

## Simple and Fast Setup

* In-memory authentication is quick to set up, making it ideal for prototyping or small applications that don't require persistent user storage.  
* No need to configure a database or external authentication provider.

## Useful for Testing

* It is often used for testing purposes, allowing developers to quickly test different security configurations without needing to set up a complex authentication system.

## No External Dependencies

* There are no external dependencies like a database or LDAP server required to store user data, so it can be used in situations where external systems are unavailable or unnecessary.

---

# Limitations of In-Memory Authentication

## Not Suitable for Production

* In-memory authentication is not designed to be used in a production environment, as user data is lost when the application is restarted. For persistent user management, a database or an external authentication provider should be used.  
* It doesn't support features like user account management, password reset functionality, or multi-factor authentication (MFA).

## Limited Scalability

* Because the user data is stored in the memory of a single server, it doesn't scale well across multiple instances of your application (e.g., in a distributed environment or microservices architecture).  
* In large applications, you'll need a more robust solution like database-backed authentication or integration with an identity provider (e.g., OAuth2 or LDAP).

---

# Example Use Case

In-memory authentication is ideal for:

* **Prototyping**: Quickly setting up authentication for a small-scale app.  
* **Testing**: Running unit or integration tests for your security configuration without the need for a database.  
* **Learning and Demos**: Providing a simple and quick demonstration of Spring Security concepts.

---

# Conclusion

**In-memory authentication** with Spring Security is a quick and easy way to handle authentication for small applications or testing environments. It provides the basic functionality to define users, assign roles, and secure URLs using an `InMemoryUserDetailsManager`. However, it is **not suitable for production applications** where persistent storage and scalable solutions are required. For production-grade authentication, integrating a database or external authentication provider is recommended.

