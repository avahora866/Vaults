---
tags:
  - Spring
  - SECONDARY_BRANCH
---
# Core Spring Expression Language (SpEL)

SpEL is a **powerful expression language** that allows **dynamic evaluation of expressions** in Spring applications. It is part of the **Spring Core module** and is used for:

✅ **Bean property access**  
 ✅ **Method invocation**  
 ✅ **Mathematical and logical operations**  
 ✅ **Collection manipulation**  
 ✅ **Dynamic evaluation in Spring configurations**

## Basic SpEL Syntax & Examples

```java
ExpressionParser parser = new SpelExpressionParser();

// Literal values
String hello = parser.parseExpression("'Hello World'").getValue(String.class); // "Hello World"

// Mathematical operations
int sum = parser.parseExpression("10 + 20").getValue(Integer.class); // 30

// Boolean expressions
boolean isValid = parser.parseExpression("10 > 5").getValue(Boolean.class); // true

// Accessing Java methods
String substring = parser.parseExpression("'Spring'.substring(0,3)").getValue(String.class); // "Spr"
```

---

# Spring Security Expression Language

Spring Security **extends SpEL** to introduce security-specific expressions. It enables **access control at method-level, URL-level, and permission evaluation**. This is **not part of Spring Core** but rather the **Spring Security module**.

## Key Features of Spring Security SpEL

- **User authentication-based expressions** (`isAuthenticated()`, `principal.username`)  
- **Role-based access control** (`hasRole()`, `hasAuthority()`)  
- **Context-aware security checks** (`authentication`, `permitAll()`, `denyAll()`)  
 Custom security service methods (`@securityService.hasAccess(authentication, #id)`)

## Common Use Cases & Examples

### Method-Level Security using SpEL

Annotations like `@PreAuthorize` and `@PostAuthorize` allow access control at the **method level**.

```java
@PreAuthorize("hasRole('ADMIN')")
public void adminOnlyMethod() {
    // Only accessible to ADMIN role users
}

@PreAuthorize("isAuthenticated()")
public void authenticatedUsersOnly() {
    // Accessible to any authenticated user
}

@PreAuthorize("authentication.name == 'john.doe'")
public void specificUserOnly() {
    // Only 'john.doe' can call this method
}
```

### URL-Based Security using SpEL

Spring Security allows **SpEL-based access control** in **HTTP security configuration**.

```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/admin/**").access("hasRole('ADMIN')")
    .requestMatchers("/user/**").access("isAuthenticated()")
);
```

### Custom Permission Evaluators

Spring Security SpEL allows calling **custom security services** for **dynamic access control**.

```java
PreAuthorize("@securityService.canAccess(authentication, #resourceId)")
public void secureMethod(Long resourceId) {
    // Uses a custom security service for fine-grained access control
}
```

---

# Security Risks & Mitigations

- **Expression Injection Attack** 
	- If user input is directly evaluated in SpEL expressions, it can lead to vulnerabilities.  
	- **Mitigation**: Never allow user-controlled values to be evaluated in SpEL.

- **Overuse of `@PostAuthorize`**  
	- Since it allows execution before permission checks, sensitive actions may still be executed.  
	- **Mitigation**: Prefer `@PreAuthorize` over `@PostAuthorize` where possible.

 - **Complex SpEL Expressions**  
	 - Overcomplicated SpEL expressions can introduce errors and security gaps.  
	 - **Mitigation**: Keep expressions simple and well-documented.

---

 Advanced Use of SpEL for Bean Property Injection and Conditional Bean Creation

Spring allows dynamic evaluation of expressions within **Spring bean configurations**, enabling **property injection** and **conditional bean creation**.

---

# Advanced Bean Property Injection with SpEL

SpEL can inject **dynamic values** into beans from:

- **Externalized configurations** (properties files, system environment)  
- **Computed or derived values** (math operations, date calculations)  
- **Context-aware bean property assignment**

## Injecting Environment Variables and Properties


```java
Value("#{systemProperties['user.name']}")
private String systemUser;  // Injects the system's logged-in user

Value("#{T(java.lang.Math).random()  100}")
private double randomNumber;  // Injects a random number between 0 and 100

Value("#{environment['app.name'] ?: 'DefaultApp'}")  
private String appName;  // Injects a value from application properties or falls back to "DefaultApp"
```

## Injecting Bean References and Method Calls

```java
@Component
public class ConfigService { 
    public int getMaxUsers() { 
        return 50;
    }
}

@Component
public class UserService {
   @Value("#{configService.getMaxUsers()}")  
    private int maxUsers;  // Retrieves the value from ConfigService 
}
```

---

# Conditional Bean Creation with SpEL

## Using `@ConditionalOnExpression`

```
feature.x.enabled=true
```

 ```java
@Bean 
@ConditionalOnExpression("${feature.x.enabled} == true")
public SomeService someService() {
    return new SomeService();
}
 ```


## Using `@Bean` with SpEL in Configuration Classes

```java
@Bean
ublic MyService myService(@Value("#{systemProperties['os.name'].contains('Windows')}") boolean isWindows) {  
    return isWindows ? new WindowsService() : new UnixService();
}
```

## Using SpEL in XML Configurations

```xml
<bean id="featureService" class="com.example.FeatureService"
      factory-method="getInstance">
   <constructor-arg value="#{environment['feature.enabled'] == 'true'}"/>
</bean>
```

---

# Best Practices for Using SpEL

- **Use `@Value` for property injections** (e.g., system properties, environment variables).  
- **Use `@ConditionalOnExpression` for feature toggles** and environment-based bean creation.  
- **Avoid overly complex SpEL expressions** that make code hard to read.  
- **Prefer Java-based logic** over large SpEL expressions for better maintainability.

---

# Conclusion

* **SpEL (Core)** enables **dynamic property evaluation** in Spring applications.  
* **Spring Security SpEL** extends SpEL for **access control in Spring Security**.  
* **SpEL for bean property injection** allows **configurable, maintainable, and flexible applications**.  
* **SpEL for conditional bean creation** ensures **optimized resource allocation** and **dynamic feature management**.

