---
tags:
  - DesignPattern
---
The **Singleton pattern** is a **creational design pattern** that ensures a class has **only one instance** and provides a global point of access to that instance. It is typically used to control access to shared resources, such as configuration settings, database connections, or logging systems, where having multiple instances could cause problems.

---

# When to Use It
Use the Singleton pattern when:
- You need to ensure that **only one instance** of a class exists throughout the entire application.
- The class should have a **global access point**.
- You want to **control the creation** of the object to ensure it is created once and only when necessary.
- You want to manage a **shared resource** (e.g., database connection, logging, thread pool).

---

# Structure

## Participants

| Role            | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| `Singleton`     | The class that ensures only one instance is created and accessed.  |
| `Client`        | The class that requests the singleton instance.                    |

---

# Diagram (Text Representation)

```
   +--------------------+
   |     Singleton      |
   |--------------------|
   | - instance: Singleton (private) |
   | + getInstance(): Singleton |
   +--------------------+
             |
             v
   +--------------------+
   |     Client         |
   +--------------------+
```

---

# Java Example

## a) Singleton Class

```java
public class Singleton {
    // Private static instance that is created only once
    private static Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {}

    // Public method to access the instance, creating it if necessary
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // Lazy initialization
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("Hello from Singleton!");
    }
}
```

## b) Client Code

```java
public class Main {
    public static void main(String[] args) {
        // Access the Singleton instance
        Singleton singleton = Singleton.getInstance();
        singleton.showMessage();  // Output: Hello from Singleton!
    }
}
```

---

# Types of Singleton Implementation

## Lazy Initialization (Thread-Safe)
This approach initializes the Singleton instance only when it is needed, ensuring minimal overhead. However, it needs to handle thread safety in multi-threaded applications.

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

- **Double-Checked Locking**: The `synchronized` block is used only when `instance` is `null` to improve performance in multi-threaded environments.

## Eager Initialization
The Singleton instance is created when the class is loaded, even if it is never used. This is simpler but can lead to overhead if the instance is never needed.

```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

## Bill Pugh Singleton (Best for thread-safety and lazy loading)
This approach uses a static inner helper class to hold the instance. The instance is created only when the class is referenced.

```java
public class Singleton {
    private Singleton() {}

    private static class SingletonHelper {
        // The instance is created when the class is referenced
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

---

# Advantages

- **Controlled access** to the sole instance of a class.
- **Lazy initialization**: The instance is created only when needed, saving resources.
- **Global access point**: It provides a global point of access to the instance, ensuring consistency.
- **Thread-safe** in multi-threaded environments when implemented correctly.
- **Prevents duplicate instances**: Prevents resource waste by ensuring only one instance is created.

---

# Disadvantages

- **Global state**: Can lead to tightly coupled code if overused.
- **Testing difficulties**: Can make unit testing harder due to the global state and dependency on a single instance.
- **Hidden dependencies**: Makes it easy to introduce hidden dependencies in your code since the instance is globally accessible.

---

# Comparison with Other Patterns

| Pattern            | Use Case                                                     |
|--------------------|--------------------------------------------------------------|
| **Factory Method** | Object creation with customization                          |
| **Abstract Factory** | Creation of families of related objects                     |
| **Singleton**      | Ensure a single instance of a class throughout the application |
| **Prototype**      | Clone objects rather than creating new ones                  |

---

# Real-World Analogies

- **Database connection pool**: Only one connection pool is typically required in an application, so the Singleton pattern ensures that only one instance of the connection pool is created and used.
- **Logger**: A logging system often needs to be shared across multiple components of an application, so a Singleton can ensure only one instance of the logger is used throughout the application.

---

## Common Pitfalls

- **Multithreading**: Incorrect implementations of Singleton in multi-threaded environments can lead to the creation of multiple instances. Use synchronization or other thread-safe techniques.
- **Testing**: Singleton can make it harder to mock or substitute objects in unit tests. This can often be addressed by using Dependency Injection (DI) patterns in combination with Singleton.

---