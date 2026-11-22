---
tags:
  - FORMAT
  - Spring
---
Spring Security's architecture is built on a series of core components that work together to provide authentication, authorization, and protection against security threats. Understanding these components is essential for grasping how Spring Security handles application security.

# Core Components of Spring Security

## Security Filter Chain

* **Purpose**: Spring Security uses a chain of filters to intercept incoming HTTP requests and apply security checks.  
* **Component**: The `SecurityFilterChain` is a series of filters that include authentication, authorization, and other security tasks.  
* **Customization**: Developers can customize the filter chain using `HttpSecurity` in Spring Security configurations.  
* **Example Filters**:  
  * `UsernamePasswordAuthenticationFilter`: Handles form login authentication.  
  * `BasicAuthenticationFilter`: Supports basic HTTP authentication.  
  * `ExceptionTranslationFilter`: Translates exceptions into appropriate HTTP responses.  
  * `LogoutFilter`: Manages user logout.

## Authentication Manager

* **Purpose**: Responsible for authenticating users by delegating authentication requests to a list of authentication providers.  
* **Component**: The `AuthenticationManager` interface is the primary entry point for authentication.  
* **Delegation**: It typically uses multiple `AuthenticationProvider` implementations to support different authentication mechanisms.

## Authentication Provider

* **Purpose**: Processes authentication requests and validates credentials.  
* **Component**: Implementations of `AuthenticationProvider` handle specific authentication types, like username/password or OAuth2.  
* **Custom Providers**: Developers can create custom authentication providers for unique authentication requirements.

## UserDetailsService

* **Purpose**: Loads user-specific data during authentication.  
* **Component**: The `UserDetailsService` interface provides user information by implementing its `loadUserByUsername()` method.  
* **Custom User Management**: Allows integration with databases, LDAP, or external authentication providers.

## Authentication Object

* **Purpose**: Represents the authentication state of a user.  
* **Component**: The `Authentication` interface contains details like principal (user), credentials (password), and granted authorities (roles).  
* **State Management**: It is created after successful authentication and stored in the security context.

## Security Context

* **Purpose**: Holds the authenticated user’s details and associated security information.  
* **Component**: The `SecurityContext` interface stores the `Authentication` object.  
* **Persistence**: Maintained throughout the request lifecycle using `SecurityContextHolder`.

## Access Decision Manager

* **Purpose**: Makes authorization decisions based on user roles, permissions, and other conditions.  
* **Component**: The `AccessDecisionManager` evaluates if a user has the necessary permissions to access a resource.  
* **Voting Mechanism**: It uses a voting strategy with multiple `AccessDecisionVoter` implementations to make decisions.

## Exception Translation

* **Purpose**: Handles security-related exceptions and converts them into appropriate HTTP responses.  
* **Component**: The `ExceptionTranslationFilter` manages exceptions like `AccessDeniedException` and `AuthenticationException`.  
* **Error Handling**: It redirects unauthorized users to login pages or sends error responses.

# Spring Security Workflow Overview

1. **Request Interception**: The `SecurityFilterChain` intercepts all incoming requests.  
2. **Authentication**: The request is passed to the `AuthenticationManager`, which delegates to the appropriate `AuthenticationProvider`.  
3. **User Validation**: The `UserDetailsService` loads user information for validation.  
4. **Security Context Management**: Upon successful authentication, the authenticated user details are stored in the `SecurityContextHolder`.  
5. **Authorization**: The `AccessDecisionManager` evaluates whether the authenticated user has permission to access the requested resource.  
6. **Exception Handling**: If authentication or authorization fails, exceptions are handled by the `ExceptionTranslationFilter`.

# Conclusion

Spring Security’s architecture is modular and flexible, allowing developers to customize components as needed. The security filter chain, authentication providers, user management systems, and security context work in harmony to ensure secure application access and protect against threats. Understanding these components will help in building and maintaining secure Spring applications.

