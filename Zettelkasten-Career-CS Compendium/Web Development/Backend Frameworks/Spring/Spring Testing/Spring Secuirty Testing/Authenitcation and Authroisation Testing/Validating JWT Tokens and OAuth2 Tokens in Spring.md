---
tags:
  - FORMAT
  - Spring
---
# Validating JWT Tokens and OAuth2 Tokens in Spring

## Validating JWT Tokens in Spring

In a Spring-based application, JWT tokens are typically validated using **Spring Security** along with a few libraries that help in parsing and verifying the integrity of JWTs.

Here’s how you can validate JWT tokens in Spring:

### 1 Steps to Validate JWT Tokens in Spring Security

1. **Add Dependencies:**

    To handle JWT validation in a Spring Boot project, you need the following dependencies:

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.11.5</version> <!-- or any compatible version -->
</dependency>
```

2.   
   **Create a JWT Utility Class:**

    This class will handle the parsing and validation of JWTs using the `io.jsonwebtoken` library.

```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import java.util.Date;

public class JwtUtil {
    private String secretKey = "your-secret-key"; // secret key for signing and validating tokens

    // Extract the username (or subject) from the token
    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    // Extract any claim
    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        return claimsResolver.apply(claims);
    }

    // Extract all claims from the token
    private Claims extractAllClaims(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(secretKey)
                .build()
                .parseClaimsJws(token)
                .getBody();
    }

    // Validate the token (check if expired, signature valid)
    public Boolean isTokenExpired(String token) {
        return extractExpiration(token).before(new Date());
    }

    // Extract the expiration date of the token
    public Date extractExpiration(String token) {
        return extractClaim(token, Claims::getExpiration);
    }

    // Check if the token is valid
    public Boolean validateToken(String token, String username) {
        return (username.equals(extractUsername(token)) && !isTokenExpired(token));
    }
}
```

3.   
   **Configure JWT Filter to Intercept Requests:**

    To validate the JWT for every request, you can create a custom filter that intercepts incoming requests and validates the token.

```java
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private JwtUtil jwtUtil;

    public JwtAuthenticationFilter(JwtUtil jwtUtil) {
        this.jwtUtil = jwtUtil;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                     javax.servlet.FilterChain chain) throws ServletException, IOException {

        final String authorizationHeader = request.getHeader("Authorization");

        String username = null;
        String jwt = null;

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            jwt = authorizationHeader.substring(7); // Remove "Bearer " prefix
            username = jwtUtil.extractUsername(jwt);
        }

        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            if (jwtUtil.validateToken(jwt, username)) {
                // Create the Authentication object
                SecurityContextHolder.getContext().setAuthentication(new UsernamePasswordAuthenticationToken(
                        username, null, new ArrayList<>()));
            }
        }

        chain.doFilter(request, response);
    }
}
```

4.   
   **Add the Filter to Security Configuration:**

    Register the filter in your Spring Security configuration.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeRequests()
            .antMatchers("/public/**").permitAll()
            .anyRequest().authenticated();

        http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);
    }
}
```

## Validating OAuth2 Tokens in Spring

Spring Security provides comprehensive support for **OAuth2 authentication**. OAuth2 tokens, usually access tokens, can be validated automatically through Spring Security’s OAuth2 client and resource server modules.

### 1 Add Dependencies:

For OAuth2 support, you need to include the relevant Spring Security OAuth2 dependencies in your `pom.xml`.

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>
```

This dependency will enable Spring Security's built-in support for OAuth2 resource servers.

### 2 Configure OAuth2 Resource Server:

In Spring Security, OAuth2 validation is done automatically by configuring the application to act as an OAuth2 **resource server**. You can do this by providing a `JwtDecoder` bean and enabling the OAuth2 resource server.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.oauth2.jwt.JwtDecoder;
import org.springframework.security.oauth2.jwt.NimbusJwtDecoder;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.oauth2ResourceServer()
            .jwt()
            .decoder(jwtDecoder())
            .and()
            .authorizeRequests()
            .anyRequest().authenticated();
    }

    @Bean
    public JwtDecoder jwtDecoder() {
        // You can use JWT decoder from a public key or a JWK set URL
        return NimbusJwtDecoder.withPublicKey(publicKey()).build();
    }

    public RSAPublicKey publicKey() {
        // Example method to load the public key (you can get this from your authorization server)
        return (RSAPublicKey) KeyFactory.getInstance("RSA").generatePublic(new X509EncodedKeySpec(Base64.getDecoder().decode(publicKeyString)));
    }
}
```

In this configuration:

* Spring Security automatically configures OAuth2 token validation.

* The `JwtDecoder` is configured to decode and verify the JWT tokens issued by your OAuth2 provider (e.g., an authorization server).

The `JwtDecoder` is responsible for:

* Verifying the signature using the public key.

* Validating the token's claims, such as expiration and audience.

### 3 OAuth2 Token Validation with JWT:

If the OAuth2 access token is a **JWT**, the token will be decoded and validated using the configured `JwtDecoder` (as shown in the previous code snippet).

### 4 OAuth2 Token Validation with Opaque Tokens:

If your OAuth2 tokens are opaque (i.e., not in JWT format), Spring Security will make a request to the authorization server to validate the token. This requires configuring a `OAuth2TokenValidator` and `OAuth2TokenIntrospectionClient`.

```java
import org.springframework.security.oauth2.client.registration.ClientRegistration;
import org.springframework.security.oauth2.client.registration.ClientRegistrationRepository;
import org.springframework.security.oauth2.client.token.IntrospectionUriTokenIntrospectionClient;
import org.springframework.security.oauth2.client.token.OAuth2TokenValidator;

@Configuration
public class OAuth2SecurityConfig {

    @Bean
    public OAuth2TokenValidator tokenValidator() {
        // Use introspection to validate opaque tokens
        return new OAuth2TokenValidator(new IntrospectionUriTokenIntrospectionClient("https://authorization-server.com/introspect"));
    }
}
```

This configuration is for scenarios where the token is opaque (i.e., not self-contained) and the resource server needs to query the authorization server to validate the token.

## Conclusion

* **JWT Token Validation**: In Spring Security, JWT tokens can be validated using a custom filter, `JwtDecoder`, and utility classes that handle parsing, expiration checks, and signature validation.

* **OAuth2 Token Validation**: OAuth2 tokens are validated automatically by Spring Security if the app is configured as a resource server, either using JWT decoding or token introspection for opaque tokens.

Both methods ensure the integrity, authenticity, and validity of tokens before allowing access to secured resources.