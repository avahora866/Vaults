---
tags:
  - DesignPattern
---
The **Proxy** pattern is a **structural** design pattern that provides a **placeholder** or **surrogate** for another object to control access to it. The proxy implements the same interface as the real object and adds additional behavior like **lazy loading**, **access control**, **logging**, or **caching**.

# Intent

- Provide a surrogate or placeholder for another object to control access to it.
- Use when accessing the real object is **costly**, **risky**, or **requires additional logic**.

# Motivation

Imagine you have a high-resolution image viewer. Loading and displaying a high-res image might be time-consuming or resource-heavy. To improve performance, you can create a **proxy** that only loads the image when it's actually needed (lazy loading).

The **proxy** acts like the image and implements the same interface but defers heavy operations until necessary.

# Types of Proxy

1. **Virtual Proxy**: Defers object creation until needed (lazy loading).
2. **Protection Proxy**: Controls access to the object (e.g., user permissions).
3. **Remote Proxy**: Represents an object in a different address space (e.g., on a remote server).
4. **Smart Reference**: Adds extra behavior like reference counting, logging, or locking.

# Structure

```plaintext
Client --> Proxy --> RealSubject
             |
        (same interface)
```

- **Subject**: Interface shared by RealSubject and Proxy.
- **RealSubject**: The actual object that does the work.
- **Proxy**: Controls access to RealSubject.

# Java Example: Virtual Proxy for Lazy Loading Images

## Step 1: Subject Interface

```java
interface Image {
    void display();
}
```

## Step 2: RealSubject

```java
class RealImage implements Image {
    private final String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();  // Simulate expensive operation
    }

    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }

    public void display() {
        System.out.println("Displaying " + filename);
    }
}
```

## Step 3: Proxy

```java
class ProxyImage implements Image {
    private final String filename;
    private RealImage realImage;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);  // lazy initialization
        }
        realImage.display();
    }
}
```

## Step 4: Client Code

```java
public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image image = new ProxyImage("high_res_photo.jpg");

        System.out.println("Image created, but not loaded yet.");

        image.display();  // loads and displays
        image.display();  // displays without loading
    }
}
```

## Output

```
Image created, but not loaded yet.
Loading high_res_photo.jpg
Displaying high_res_photo.jpg
Displaying high_res_photo.jpg
```

# Benefits

- **Improves performance** by delaying expensive operations (virtual proxy).
- **Adds security** by controlling access (protection proxy).
- **Adds logging or audit capabilities** transparently.
- **Simplifies networking** with remote proxies.

# Drawbacks

- **Adds complexity**: More classes to manage.
- **Can introduce overhead**: If not needed, proxies may slow down the system.

# Use Cases

- Lazy loading resources (e.g., files, images).
- Access control for secure resources.
- Network stubs in RMI (Java Remote Method Invocation).
- Smart pointers or reference-counted objects.

# Real-World Analogies

- **Credit card**: Acts as a proxy for your bank account.
- **TV remote**: Controls a device indirectly.
- **Proxy server**: Intercepts requests to a real server, adding caching, access control, etc.

# Related Patterns

- **Decorator**: Also wraps an object but adds new behavior. Proxy focuses on controlling access.
- **Adapter**: Changes interface; Proxy preserves it.
- **Facade**: Simplifies a subsystem; Proxy controls access to an object.