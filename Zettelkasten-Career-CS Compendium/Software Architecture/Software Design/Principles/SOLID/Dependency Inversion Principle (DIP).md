---
tags:
  - FORMAT
  - SoftwareDesign
  - "#SECONDARY_BRANCH"
---
**Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

DIP is the "D" in SOLID and a foundational concept behind **loose coupling** in object-oriented design. It encourages building systems where high-level logic (like business rules) doesn’t directly rely on low-level implementations (like database access or logging). Instead, **both depend on an abstract interface**, allowing the system to be more flexible, testable, and maintainable.

# What Are High-Level and Low-Level Modules?

- **High-level modules**: Contain core logic and business rules. They orchestrate operations and decision-making.
- **Low-level modules**: Handle system-specific operations like file I/O, database access, networking, etc.

Without DIP, high-level modules often directly use low-level implementations, creating **tight coupling**. That means:
- Changing the low-level code may break the high-level logic.
- Testing becomes difficult.
- Swapping implementations (e.g., switching from MySQL to MongoDB) requires changes in the high-level code.
# DIP in Practice: Inversion of Dependencies

Instead of the high-level module depending directly on a low-level module, **both should depend on an interface** (abstraction). Then, the low-level module implements that interface.

This “inverts” the usual flow of dependencies. Rather than high-level calling low-level, the low-level provides behaviour that the high-level consumes through an abstraction.

# Example Without DIP

```java
public class FileLogger {
    public void log(String message) {
        // Write message to a file
    }
}

public class UserService {
    private FileLogger logger = new FileLogger();

    public void createUser(String name) {
        // Create user
        logger.log("User created: " + name);
    }
}
```

`UserService` is **tightly coupled** to `FileLogger`. You can't easily:

- Replace the logger
- Test `UserService` without doing real file I/O
- Extend functionality
# Refactored With DIP

Define an abstraction:

```java
public interface Logger {
    void log(String message);
}
```

Provide implementations:

```java
public class FileLogger implements Logger {
    public void log(String message) {
        // File logging logic
    }
}

public class ConsoleLogger implements Logger {
    public void log(String message) {
        System.out.println(message);
    }
}
```

Update the high-level module to depend on the abstraction:

```java
public class UserService {
    private Logger logger;

    public UserService(Logger logger) {
        this.logger = logger;
    }

    public void createUser(String name) {
        // Create user
        logger.log("User created: " + name);
    }
}
```

Now, `UserService` doesn’t care how logging happens. It just depends on the `Logger` interface. You can inject any implementation, including a mock for testing.

# How to Apply DIP

1. **Identify dependencies** between high-level and low-level components.
2. **Extract interfaces or abstract classes** for those dependencies.
3. **Program to interfaces**, not concrete classes.
4. **Use Dependency Injection (DI)** to provide implementations at runtime (via constructor, setter, or framework).

# DIP vs Dependency Injection

- **DIP** is a design principle: _depend on abstractions, not concretions._
- **Dependency Injection (DI)** is a technique (or pattern) used to apply DIP: you _inject_ the dependency rather than create it internally.

They go hand in hand, but DIP is the architectural goal, while DI is the mechanism.

# Why DIP Matters

1. **Loose Coupling**: Modules are easier to change independently.
2. **Testability**: You can easily mock dependencies.
3. **Flexibility**: Swap implementations with minimal changes.
4. **Scalability**: Large systems benefit from clear separation of concerns.
5. **Maintainability**: You reduce the ripple effect of changes.

# Real-World Analogy

Think of **electrical plugs and sockets**:

- Appliances (like your phone charger) don’t depend on the exact design of a power plant.
- They just expect a consistent **interface**: the socket.
- Whether power comes from coal, wind, or solar doesn’t matter—the appliance uses the **abstraction** (the plug/socket interface).

---

In summary, the Dependency Inversion Principle helps you write **resilient, decoupled code** where high-level business logic is independent of low-level implementation details.  