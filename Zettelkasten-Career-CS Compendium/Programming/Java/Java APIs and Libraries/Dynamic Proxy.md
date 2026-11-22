---
tags:
  - FORMAT
  - Java
---
# Dynamic Proxy in Java

### What is a Dynamic Proxy?

A **dynamic proxy** in Java is a mechanism that allows you to create a proxy object **at runtime** instead of writing a separate class manually. It is mainly used to **intercept method calls** and perform additional operations like logging, security checks, caching, and more.

Dynamic proxies in Java are implemented using the `java.lang.reflect.Proxy` class and `InvocationHandler` interface.

---

## When to Use Dynamic Proxy?

Dynamic proxies are useful when:

* You want to **intercept method calls** (e.g., for logging, performance monitoring, or security checks).  
* You need to **create proxy objects dynamically** without writing separate classes.  
* You want to implement **AOP (Aspect-Oriented Programming)** features like method interception.  
* You need to work with **interfaces dynamically** at runtime.

---

## How Dynamic Proxy Works

A dynamic proxy works by:

1. Creating a **proxy instance** that implements one or more interfaces.  
2. Routing method calls to an **InvocationHandler**.  
3. Executing custom logic **before or after** invoking the actual method.

---

## Example: Creating a Dynamic Proxy

Let's create a dynamic proxy that intercepts method calls and logs them.

### Step 1: Define an Interface

```java
interface Service {
    void performAction();
}
```

### Step 2: Implement the Interface

```java
class RealService implements Service {
    public void performAction() {
        System.out.println("Executing real service action...");
    }
}
```

### Step 3: Create an Invocation Handler

The `InvocationHandler` intercepts method calls and adds additional behavior.

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

class LoggingHandler implements InvocationHandler {
    private final Object target;

    public LoggingHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method: " + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("After method: " + method.getName());
        return result;
    }
}
```

### Step 4: Create the Dynamic Proxy

```java
public class DynamicProxyExample {
    public static void main(String[] args) {
        Service realService = new RealService();

        // Creating a dynamic proxy
        Service proxyInstance = (Service) Proxy.newProxyInstance(
            realService.getClass().getClassLoader(),
            new Class[]{Service.class},
            new LoggingHandler(realService)
        );

        // Calling method via proxy
        proxyInstance.performAction();
    }
}
```

### Output

```
Before method: performAction
Executing real service action...
After method: performAction
```

---

## How It Works

1. **Proxy.newProxyInstance()** creates a proxy object that implements the `Service` interface.  
2. Calls to `proxyInstance.performAction()` are forwarded to `LoggingHandler.invoke()`.  
3. The `invoke()` method logs messages before and after calling the real method.

---

## Advanced Use Cases of Dynamic Proxy

1. **Security Checks**

   * Use proxies to enforce authentication or authorization before calling methods.  
2. **Transaction Management**

   * Wrap database operations in transactions dynamically.  
3. **Performance Monitoring**

   * Log execution time of methods.  
4. **Lazy Loading**

   * Delay object initialization until required.

---

## Limitations of Dynamic Proxy

* Works **only with interfaces** (not concrete classes).  
* For concrete classes, use **CGLIB (Code Generation Library)** instead.  
* Slight performance overhead due to method interception.

---

## Summary

* **Dynamic proxies** allow method interception at runtime using `Proxy` and `InvocationHandler`.  
* Useful for **logging, security, AOP, performance monitoring**, etc.  
* Works only with **interfaces** (for concrete classes, use **CGLIB**).  
* Provides **cleaner and more flexible code** compared to manually writing proxy classes.

By using dynamic proxies, you can **enhance functionality without modifying existing classes**, making your Java applications more **modular and maintainable**\! 🚀

