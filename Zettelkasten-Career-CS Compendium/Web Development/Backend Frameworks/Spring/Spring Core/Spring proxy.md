---
tags:
  - Spring
  - LEAF
---
# Spring Proxies in Beans and Beyond

Spring uses proxies extensively to enable features like **AOP (Aspect-Oriented Programming)**, **transaction management**, and **lazy initialization**. Understanding how these proxies work is essential for debugging and optimizing Spring applications.

---

# What Are Proxies in Spring?

A **proxy** is an object that acts as a substitute for another object, intercepting method calls and adding additional behavior before delegating to the real object.

Spring uses proxies to:

* **Enable AOP features (e.g., `@Transactional`, `@Cacheable`)**  
* **Apply security restrictions (`@Secured`)**  
* **Manage bean lifecycle and lazy initialization**

---

# Types of Proxies in Spring

Spring supports two primary proxy mechanisms:

* **JDK Dynamic Proxies** (Interface-based)  
* **CGLIB Proxies** (Class-based)

## A. JDK Dynamic Proxies

* Used when a bean implements **at least one interface**.  
* The proxy implements the interface and delegates calls to the actual implementation.

### Example: JDK Proxy in Spring

```java
public interface MyService {
    void perform();
}

@Service
public class MyServiceImpl implements MyService {
    public void perform() {
        System.out.println("Executing business logic...");
    }
}
```

If `@Transactional` is used, Spring will generate a **JDK proxy** for `MyService` instead of directly using `MyServiceImpl`.

---

## B. CGLIB (Code Generation Library) Proxies

* Used when there is **no interface** in the bean.  
* CGLIB creates a **subclass** of the target class and overrides its methods.

### Example: CGLIB Proxy in Spring

```java
@Service
public class MyService {
    public void perform() {
        System.out.println("Executing business logic...");
    }
}
```

Since `MyService` does **not** implement an interface, Spring will use **CGLIB** to proxy it.

---

# How Spring Chooses a Proxy Type

By default, Spring chooses:

* **JDK Proxy** if the bean implements an interface.  
* **CGLIB Proxy** if no interface is present.

## Forcing CGLIB Proxy

Even if a bean implements an interface, you can force CGLIB by setting:

```java
@EnableAspectJAutoProxy(proxyTargetClass = true)
```

This forces Spring to use class-based CGLIB proxies instead of JDK proxies.

---

# Spring Proxy Usage Scenarios

## A. Proxy in `@Transactional`

Spring proxies help manage transactions by **wrapping** the bean and intercepting method calls.

### Example: `@Transactional` with Proxy

```java
@Service
public class PaymentService {

    @Transactional
    public void processPayment() {
        System.out.println("Processing payment...");
    }
}
```

* Spring creates a **proxy** around `PaymentService`.  
* Calls to `processPayment()` are intercepted, and Spring manages the transaction.

💡 **Key Rule**: If a method inside the same class calls `processPayment()`, **Spring won’t intercept it\!** (Self-invocation issue)

### Fix for Self-Invocation Issue

Use **`ApplicationContext` or `AopContext`** to call the proxied version.

```java
@Autowired
private ApplicationContext context;

public void callTransactionalMethod() {
    PaymentService proxy = context.getBean(PaymentService.class);
    proxy.processPayment(); // This works!
}
```

---

## B. Proxy in AOP (`@Aspect`)

Spring AOP (Aspect-Oriented Programming) relies on proxies to apply **cross-cutting concerns** like logging or security.

### Example: AOP Using Proxy

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Before method: " + joinPoint.getSignature().getName());
    }
}
```

* Spring **creates a proxy** for target beans and injects `logBefore()` **before** method execution.

---

## C. Lazy Initialization with Proxies

Spring uses proxies for **lazy initialization** to delay object creation.

### Example: Lazy Initialization Proxy

```java
@Bean
@Lazy
public MyService myService() {
    return new MyService();
}
```

* Spring creates a **proxy** that initializes `MyService` **only when needed**.

---

# Common Pitfalls with Spring Proxies

| Issue | Cause | Solution |
| ----- | ----- | ----- |
| **Self-invocation issue** (method inside the same class does not trigger proxy) | Calls within the same class bypass the proxy. | Use `ApplicationContext.getBean()` to retrieve the proxy. |
| **Final methods are not proxied** | CGLIB cannot override `final` methods. | Avoid `final` methods in proxied beans. |
| **Direct object instantiation bypasses proxy** | `new MyService()` does not use Spring context. | Always inject beans using Spring (`@Autowired`). |
| **JDK proxies only work with interfaces** | Class-based beans need CGLIB. | Use `@EnableAspectJAutoProxy(proxyTargetClass = true)`. |

---

# Conclusion

Spring proxies are a powerful mechanism for enabling **AOP, transactions, and lazy loading**. By understanding how **JDK dynamic proxies** and **CGLIB proxies** work, you can optimize performance, avoid pitfalls, and leverage Spring’s capabilities effectively. 🚀

