---
tags:
  - CHECKLIST
---
Here is the list formatted as a checklist:

- [ ] **DaoAuthenticationProvider**: Authenticates users against a database using a `UserDetailsService`.
- [ ] **LdapAuthenticationProvider**: Authenticates users using LDAP (Lightweight Directory Access Protocol).
- [ ] **ActiveDirectoryLdapAuthenticationProvider**: Authenticates users specifically against an Active Directory server using LDAP.
- [ ] **PreAuthenticatedAuthenticationProvider**: Used when authentication details are already known, typically from an external system.
- [ ] **JwtAuthenticationProvider**: Authenticates users based on JSON Web Tokens (JWT).
- [ ] **OAuth2LoginAuthenticationProvider**: Handles authentication via OAuth 2.0 login, commonly used for social logins.
- [ ] ~~**CasAuthenticationProvider**: Supports authentication using the Central Authentication Service (CAS) protocol for single sign-on (SSO).~~
- [x] **Custom AuthenticationProvider**: Allows you to create a custom authentication provider by implementing the `AuthenticationProvider` interface.
- [ ] **RememberMeAuthenticationProvider**: Works with "remember me" functionality to keep users authenticated across sessions.