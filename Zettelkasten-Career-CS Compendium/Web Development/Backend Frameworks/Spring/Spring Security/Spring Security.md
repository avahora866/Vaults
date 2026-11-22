---
tags:
  - "#TRUNK"
  - SpringSecurity
  - Java
  - Spring
---

# Spring Security: Internals & Implementation

Spring Security is often treated as a magic black box — developers copy-paste a JWT filter from a tutorial, it works, and they move on. The danger appears when requirements deviate from that tutorial. Understanding the *internal architecture* is what separates an engineer who configures Spring Security from one who **controls** it. This note dissects the framework layer by layer, from the core identity model to a production-grade stateless JWT setup and OAuth2 social login.

---

# Phase 1: Spring Security Internals

## Module 1 — The Core Identity: The `Authentication` Object

Every security decision in Spring Security is built around a single question: "Who is currently using this application?" The answer is always expressed through one interface:

```java
org.springframework.security.core.Authentication
```

No matter which mechanism is used — Basic Auth, JWT, OAuth2, or a biometric scanner — Spring converts the input into an implementation of this interface. It is the **common currency** of the entire framework.

### 1.1 The `Authentication` Contract

```java
public interface Authentication extends Principal, Serializable {
    // 1. The Permissions — what this user may do
    Collection<? extends GrantedAuthority> getAuthorities();

    // 2. The Proof — usually the password (erased after verification)
    Object getCredentials();

    // 3. Request metadata — IP address, session details
    Object getDetails();

    // 4. The Identity — who the user IS
    Object getPrincipal();

    // 5. The Gate — is this token verified?
    boolean isAuthenticated();
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}
```

The three fields you will work with constantly:

| **Field** | **Before Verification** | **After Verification** |
|---|---|---|
| `getPrincipal()` | Raw username string | Full `UserDetails` object (from DB) |
| `getCredentials()` | Raw password | `null` — erased immediately for security |
| `isAuthenticated()` | `false` | `true` |

> **The Crucial Detail — Input/Output Container:** The `Authentication` object serves a dual role. Before verification, it is the *input* — carrying what the user claims. After verification, it is the *output* — carrying the confirmed identity. The `AuthenticationManager` (see Module 3) accepts an unverified `Authentication` and returns a verified one. This is why we need an `Authentication` object *before* we have authenticated the user — the engine demands a standardised input type.

### 1.2 `SecurityContext` and ThreadLocal Storage

Once the `Authentication` is verified, it must be accessible anywhere in the application — controllers, services, repositories — without passing it through every method signature. Spring Security solves this with **ThreadLocal storage**.

**The Hierarchy:**
```
SecurityContextHolder  (static, global access point)
  └── SecurityContext  (container for the Authentication)
        └── Authentication  (the verified identity)
```

**Accessing the current user anywhere in your code:**
```java
// Works in any @Service, @Controller, or @Repository
SecurityContext context = SecurityContextHolder.getContext();
Authentication authentication = context.getAuthentication();
String username = authentication.getName();
```

**Why ThreadLocal works:** In a standard servlet application, each HTTP request is handled by exactly one thread. ThreadLocal stores data that is scoped to that specific thread. Thread A (handling User A) has its own private `SecurityContext`. Thread B (handling User B) cannot see Thread A's context. This isolation is the mechanism that makes per-request security data safe.

> ⚠️ **The Async Trap — The Empty Pocket Problem:** ThreadLocal has a critical failure mode with asynchronous code. If you spawn a new thread inside a controller (e.g., via `CompletableFuture.runAsync()` or `@Async`), the new thread has a **completely empty** `SecurityContext`. Calling `SecurityContextHolder.getContext().getAuthentication()` on that background thread returns `null`, causing a `NullPointerException`.
>
> **Why:** Think of a thread as a person wearing a coat. ThreadLocal is that person's coat pocket. When Thread A spawns Thread B, Thread B arrives in its own coat with empty pockets. The security ID card was never transferred.
>
> **Fix:** Configure the `SecurityContextHolder` strategy to `MODE_INHERITABLETHREADLOCAL`. This instructs Spring to copy Thread A's context into Thread B when spawned. Use with caution in thread-pool environments where threads are reused, as stale security data can leak between users.

---

## Module 2 — The Interception Layer: The Filter Chain

Authentication objects are useful only if a request is intercepted *before* reaching your controller. Spring Security achieves this via the **Servlet Filter Chain**.

### 2.1 How Spring Hooks into Tomcat

The Servlet container (Tomcat) and the Spring application context are separate worlds. Spring bridges them with a single standard filter that Tomcat knows how to use:

```
Tomcat Filter Chain
  └── DelegatingFilterProxy  (standard Servlet filter — Tomcat can see this)
        └── FilterChainProxy  (Spring bean — handles all actual security logic)
              └── [15-20 Security Filters in strict order]
```

- **`DelegatingFilterProxy`** — To Tomcat it looks like a normal filter. Its only job is to hand off control to Spring's `FilterChainProxy`.
- **`FilterChainProxy`** — The brain. Manages the ordered list of security filters. This is what you are configuring when you write a `SecurityFilterChain` bean.

### 2.2 The Critical Filters (In Execution Order)

You do not need to memorise all 15-20 filters, but these five control the entire security lifecycle:

| **Order** | **Filter** | **Job** |
|---|---|---|
| First | `SecurityContextPersistenceFilter` | "Is this user already logged in?" — loads existing `SecurityContext` from `HttpSession` |
| Middle | `UsernamePasswordAuthenticationFilter` | "Is the user *trying* to log in right now?" — intercepts `POST /login` only |
| *Custom position* | `JwtAuthenticationFilter` (yours) | "Does this request carry a valid JWT?" — validates token and populates context |
| Late | `ExceptionTranslationFilter` | "Did anything go wrong?" — catches `AccessDeniedException` (403) and `AuthenticationException` (401) and decides the response |
| Last | `FilterSecurityInterceptor` | "Is this user *allowed* to access this resource?" — the final bouncer |

### 2.3 Placing a Custom JWT Filter

> ⚠️ **Placement matters.** When implementing JWT authentication, your custom filter must be inserted **before** `UsernamePasswordAuthenticationFilter`, not after it.

**Why before:**
1. **Identity must be established early.** In a stateless architecture, the JWT *replaces* the session. Just as `SecurityContextPersistenceFilter` restores identity from a session at the very start of the chain, your JWT filter must restore identity from the token before any other filter runs.
2. **Fail fast.** An invalid or expired token should reject the request immediately, before it traverses any further security logic.

```java
// In your SecurityFilterChain configuration:
.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class)
```

---

## Module 3 — The Authentication Architecture: Managers & Providers

Intercepting the request is only step one. The framework still needs to *verify* the credentials. This is handled by a three-level delegation hierarchy.

### 3.1 The Hierarchy

```
Filter
  └── AuthenticationManager (interface — the entry point)
        └── ProviderManager (the standard implementation)
              ├── DaoAuthenticationProvider  → handles Username/Password
              ├── JwtAuthenticationProvider  → handles JWT tokens (custom)
              └── LdapAuthenticationProvider → handles LDAP
                        └── UserDetailsService (data fetcher — loads user from DB)
```

**The contract at each level:**

- **`AuthenticationManager`** — one method: `authenticate(Authentication)`. Returns a verified `Authentication` or throws an exception.
- **`ProviderManager`** — loops through its list of `AuthenticationProvider`s until one says "I can handle this token type."
- **`AuthenticationProvider`** — contains the actual verification logic. Each provider declares which token type it supports via `supports(Class<?> authentication)`.
- **`UserDetailsService`** — purely a data fetcher. Its only job: `UserDetails loadUserByUsername(String username)`. It does not check passwords.

### 3.2 The Standard Login Flow

```
1. Filter creates UsernamePasswordAuthenticationToken(username, rawPassword)  [isAuthenticated = false]
2. Passes it to ProviderManager.authenticate()
3. ProviderManager delegates to DaoAuthenticationProvider (it supports this token type)
4. DaoAuthenticationProvider calls userDetailsService.loadUserByUsername(username)
5. UserDetailsService returns UserDetails (with BCrypt hash from DB)
6. DaoAuthenticationProvider runs: passwordEncoder.matches(rawPassword, storedHash)
7. Match → returns UsernamePasswordAuthenticationToken(userDetails, null, authorities)  [isAuthenticated = true]
8. Filter stores verified Authentication in SecurityContext
```

> **The Crucial Detail — Provider Extensibility:** The beauty of this hierarchy is that adding a new authentication method does not require touching existing code. To add "Login with Retina Scan", you create a `RetinaAuthenticationProvider` and register it alongside the existing `DaoAuthenticationProvider`. The `ProviderManager` will route retina tokens to the new provider and password tokens to the old one. Each provider handles exactly one token type in isolation.

---

## Module 4 — Authorization: Access Control

Authentication answers "Who are you?" Authorization answers "What are you allowed to do?" Spring Security handles both, but they are distinct systems.

### 4.1 Authorities vs. Roles

Both are represented as strings implementing `GrantedAuthority`. The only mechanical difference is a naming convention.

| **Type** | **Naming Convention** | **Example** | **Check Method** |
|---|---|---|---|
| **Authority** | Any string | `READ_PRICING`, `EDIT_INVOICE` | `.hasAuthority("READ_PRICING")` |
| **Role** | Must start with `ROLE_` prefix | `ROLE_ADMIN`, `ROLE_MANAGER` | `.hasRole("ADMIN")` ← Spring auto-adds the prefix |

> **The Crucial Detail:** `.hasRole("ADMIN")` and `.hasAuthority("ROLE_ADMIN")` are **identical**. Spring's `.hasRole()` method automatically prepends `ROLE_` before checking. This is the source of a common bug where developers add `ROLE_ADMIN` to the user object *and* call `.hasRole("ROLE_ADMIN")`, resulting in Spring looking for `ROLE_ROLE_ADMIN`.

### 4.2 Two Layers of Authorization

**Layer A — URL Level (Filter Chain):** Configured in your `SecurityFilterChain` bean. This is your first line of defence.

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/admin/**").hasRole("ADMIN")       // Role-gated route
    .requestMatchers("/api/public/**").permitAll()       // Open to all
    .anyRequest().authenticated()                        // Everything else requires login
);
```

**Layer B — Method Level (Annotations):** More granular. Enabled by adding `@EnableMethodSecurity` to a configuration class.

```java
@PreAuthorize("hasAuthority('EDIT_INVOICE')")
public void updateInvoice(Invoice invoice) { ... }
```

> ⚠️ **The Self-Invocation Proxy Trap:** Method security works via Spring AOP — a *proxy* wraps your class and intercepts external calls to run the security check. If you call an `@PreAuthorize`-annotated method from *within the same class* using `this.methodName()`, you bypass the proxy entirely. You are calling the real object directly, never passing through the security interceptor.
>
> ```java
> // BROKEN — security check is bypassed
> public void processAll() {
>     this.deleteInvoice();  // 'this' = real object, not the proxy
> }
>
> // FIXED — call through the injected proxy
> @Autowired @Lazy
> private InvoiceService self;
>
> public void processAll() {
>     self.deleteInvoice();  // 'self' = the proxy → security check runs
> }
> ```
>
> This applies to all Spring AOP annotations: `@Transactional`, `@Cacheable`, and `@PreAuthorize`.

---

# Phase 2: Applied Implementation

## Module 5 — Implementing Stateless JWT Security

### Step 1: The Security Configuration Shell

In Spring Security 6+, the legacy `WebSecurityConfigurerAdapter` is removed. All configuration is now done via `@Bean` methods.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            // 1. Disable CSRF — safe for stateless APIs (see note below)
            .csrf(csrf -> csrf.disable())

            // 2. Tell Spring to never create an HttpSession
            .sessionManagement(session ->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))

            // 3. Define access rules
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            );

        return http.build();
    }
}
```

> **Why CSRF can be disabled for stateless APIs:** CSRF attacks exploit the browser's automatic cookie attachment. A malicious third-party page tricks the browser into sending your session cookie to a target site. In a JWT setup, the browser does not automatically attach the token — your JavaScript must explicitly add it to the `Authorization` header. A malicious page cannot read your JavaScript's local storage (same-origin policy), so it cannot steal or replay the token. CSRF protection is therefore redundant and adds noise.

### Step 2: The JWT Filter

The filter's job: intercept each request, extract the token, validate it, and if valid, populate the `SecurityContext`.

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
                                    throws ServletException, IOException {

        final String authHeader = request.getHeader("Authorization");

        // 1. Guard clause — no token present, continue the chain unauthenticated
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response);
            return;
        }

        // 2. Extract the raw JWT (strip "Bearer " prefix)
        String jwt = authHeader.substring(7);
        String userEmail = jwtService.extractUsername(jwt);

        // 3. Only proceed if user is not already authenticated
        if (userEmail != null && SecurityContextHolder.getContext().getAuthentication() == null) {

            UserDetails userDetails = userDetailsService.loadUserByUsername(userEmail);

            if (jwtService.isTokenValid(jwt, userDetails)) {

                // 4. Create a verified Authentication token (3-arg constructor = isAuthenticated: true)
                UsernamePasswordAuthenticationToken authToken =
                    new UsernamePasswordAuthenticationToken(
                        userDetails,
                        null,               // credentials null — JWT was the proof, no need to hold it
                        userDetails.getAuthorities()
                    );
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                // 5. Store in the SecurityContext for this request's thread
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }

        // 6. Pass the baton — always continue the filter chain
        filterChain.doFilter(request, response);
    }
}
```

> **Why `UsernamePasswordAuthenticationToken` for a JWT flow?** It seems semantically wrong, but it is correct for a pragmatic reason. This class has two constructors with specific behaviours:
> - **2-arg constructor** `new UsernamePasswordAuthenticationToken(user, pass)` → `isAuthenticated()` returns `false` (a login *attempt*)
> - **3-arg constructor** `new UsernamePasswordAuthenticationToken(user, null, authorities)` → `isAuthenticated()` returns `true` (a verified identity)
>
> By using the 3-arg constructor *after* validating the JWT, we are stamping the passport "Verified." The class is a generic identity container; the name is just a label from its original use case.

### Step 3: Application Configuration (Provider & Manager)

```java
@Configuration
@RequiredArgsConstructor
public class ApplicationConfig {

    private final UserRepository repository;

    // 1. Data Fetcher — retrieves the user record from your DB
    @Bean
    public UserDetailsService userDetailsService() {
        return username -> repository.findByEmail(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found"));
    }

    // 2. Provider — wires together the data source and password hasher
    @Bean
    public AuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService());
        provider.setPasswordEncoder(passwordEncoder());
        return provider;
    }

    // 3. Manager — exposes the authentication engine to your login controller
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config)
            throws Exception {
        return config.getAuthenticationManager();
    }

    // 4. Password Encoder — BCrypt is the industry standard (one-way hash)
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### Step 4: Wiring the Filter

```java
// In SecurityConfig, inject and register the filter:
private final JwtAuthenticationFilter jwtAuthFilter;
private final AuthenticationProvider authenticationProvider;

// Inside filterChain():
.addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class)
.authenticationProvider(authenticationProvider);
```

### Step 5: Performance Optimisation — Eliminate the DB Lookup

The default implementation calls `userDetailsService.loadUserByUsername()` on **every single request**, hitting the database 100 times per 100 requests. This negates the primary advantage of JWT (zero-state server).

**The fix:** Embed the user's roles directly in the JWT payload when issuing the token.

```json
{
  "sub": "user@example.com",
  "roles": ["ROLE_USER", "ROLE_ADMIN"],
  "exp": 1714000000
}
```

Then, in the filter, extract authorities from the token instead of the database:

```java
// Replace the DB lookup with token-based authority extraction:
Claims claims = jwtService.extractAllClaims(jwt);
String username = claims.getSubject();

List<String> roles = claims.get("roles", List.class);
var authorities = roles.stream()
    .map(SimpleGrantedAuthority::new)
    .toList();

var authToken = new UsernamePasswordAuthenticationToken(username, null, authorities);
```

> ⚠️ **The Revocation Trade-off:** Eliminating the DB lookup gains performance but loses real-time authority sync. If you ban a user in the database, the API will continue accepting their requests until the JWT expires. Short token expiry (15–60 minutes) combined with refresh token rotation is the standard mitigation.

---

## Module 6 — Implementing OAuth2 Social Login (Authorization Code Flow)

### Step 1: Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

### Step 2: Configuration (`application.yml`)

Spring Boot already knows Google's OAuth2 endpoint URLs. You only supply credentials from the Google Cloud Console.

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: "YOUR_GOOGLE_CLIENT_ID"
            client-secret: "YOUR_GOOGLE_CLIENT_SECRET"
            scope:
              - email
              - profile
              - https://www.googleapis.com/auth/calendar.readonly
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
```

> ⚠️ **Redirect URI must match exactly.** Copy `http://localhost:8080/login/oauth2/code/google` and paste it verbatim into the "Authorized Redirect URIs" section of your Google Cloud Console project. Any mismatch — trailing slash, wrong port — will cause Google to reject the callback.

### Step 3: Security Configuration

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/", "/login").permitAll()
            .anyRequest().authenticated()
        )
        .oauth2Login(oauth2 -> oauth2
            .loginPage("/login")
            .defaultSuccessUrl("/dashboard", true)
            .successHandler(oAuth2LoginSuccessHandler) // Override for JWT issuance
        );
    return http.build();
}
```

Adding `.oauth2Login()` automatically registers `OAuth2LoginAuthenticationFilter`, which intercepts the callback URL, exchanges the authorization code for tokens, and populates the `SecurityContext`.

### Step 4: Accessing the Authenticated User

After a successful Google login, the `Authentication` in the `SecurityContext` is an `OAuth2AuthenticationToken`. The principal is an `OAuth2User`.

```java
@GetMapping("/dashboard")
public String dashboard(@AuthenticationPrincipal OAuth2User principal) {
    String email = principal.getAttribute("email");
    String name  = principal.getAttribute("name");
    return "Welcome, " + name;
}
```

### Step 5: The Hybrid Pattern — OAuth2 Login + Your Own JWT

Using `.oauth2Login()` with default settings creates a stateful session (`JSESSIONID` cookie). This conflicts with a stateless API architecture.

**The correct pattern for SPAs and mobile apps:**
1. User authenticates with Google
2. Spring processes the OAuth2 callback and verifies the identity
3. Your `AuthenticationSuccessHandler` fires
4. You generate *your own* JWT using the Google user's details
5. You redirect the client to the frontend with the token

**Why issue your own token instead of forwarding Google's:**
- **Decoupling** — Your API only validates *your* tokens. Adding GitHub login later requires no changes to authorization logic.
- **Scope separation** — Google's token carries Google permissions. Your token carries your app's permissions.

```java
@Component
@RequiredArgsConstructor
public class OAuth2LoginSuccessHandler extends SimpleUrlAuthenticationSuccessHandler {

    private final JwtService jwtService;
    private final UserRepository userRepository;

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request,
                                        HttpServletResponse response,
                                        Authentication authentication)
                                        throws IOException, ServletException {

        OAuth2AuthenticationToken oauthToken = (OAuth2AuthenticationToken) authentication;
        OAuth2User googleUser = oauthToken.getPrincipal();

        // 1. Extract identity from the Google ID Token claims
        String email = googleUser.getAttribute("email");
        String name  = googleUser.getAttribute("name");

        // 2. Upsert to your own database (create if first login, load if returning)
        User user = userRepository.findByEmail(email)
            .orElseGet(() -> {
                User newUser = new User();
                newUser.setEmail(email);
                newUser.setName(name);
                newUser.setSource(RegistrationSource.GOOGLE);
                return userRepository.save(newUser);
            });

        // 3. Generate YOUR application JWT
        String jwt = jwtService.generateToken(user);

        // 4. Redirect frontend with token
        // Production: use a secure HttpOnly cookie instead of a URL parameter
        String targetUrl = "http://localhost:3000/oauth2/redirect?token=" + jwt;
        getRedirectStrategy().sendRedirect(request, response, targetUrl);
    }
}
```

---

# Quick Reference: Key Classes and Their Roles

| **Class / Interface** | **Role** | **Your Action** |
|---|---|---|
| `Authentication` | The identity container (pre- and post-verification) | Read via `SecurityContextHolder` |
| `SecurityContextHolder` | Static access point to the current user | Call `.getContext().getAuthentication()` |
| `UserDetailsService` | Fetches user data from storage | Implement and register as `@Bean` |
| `UserDetails` | DTO for user data (username, password hash, authorities) | Implement, often via your `User` entity |
| `AuthenticationProvider` | Contains verification logic for one token type | Implement for custom auth methods |
| `ProviderManager` | Routes tokens to the correct `AuthenticationProvider` | Standard implementation — configure, don't replace |
| `DaoAuthenticationProvider` | Handles username/password auth via `UserDetailsService` | Configure with your `UserDetailsService` + `PasswordEncoder` |
| `OncePerRequestFilter` | Base class for custom filters | Extend for `JwtAuthenticationFilter` |
| `UsernamePasswordAuthenticationToken` | The standard `Authentication` implementation | 2-arg = unauthenticated; 3-arg = authenticated |
| `GrantedAuthority` | Represents a single permission or role | `SimpleGrantedAuthority("ROLE_ADMIN")` |
| `BCryptPasswordEncoder` | One-way password hashing | Always use for stored passwords |
| `OAuth2AuthenticationToken` | The `Authentication` object after OAuth2 login | Cast from `Authentication` in handlers |
| `OAuth2AuthorizedClientService` | Retrieves stored OAuth2 tokens (e.g., Google Access Token) | Inject and call `loadAuthorizedClient()` |

---

# Architecture Decision Summary

| **Scenario** | **Recommended Approach** |
|---|---|
| Traditional monolith, server-rendered UI | Session-based authentication |
| Microservices / distributed system | Stateless JWT |
| "Login with Google" for a REST API | OAuth2 social login → issue your own JWT in success handler |
| Protecting individual service methods | `@EnableMethodSecurity` + `@PreAuthorize` |
| Calling a secured method from within the same class | Inject `self` with `@Lazy`, call through the proxy |
| High-frequency API, performance-critical | Embed roles in JWT payload; eliminate per-request DB lookup |
| Need to instantly revoke user access | Maintain a token deny-list (adds state) or use short expiry + session |
