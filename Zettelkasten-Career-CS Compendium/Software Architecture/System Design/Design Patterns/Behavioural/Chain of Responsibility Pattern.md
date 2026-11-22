---
tags:
  - DesignPattern
---
The Chain of Responsibility (CoR) pattern is a **behavioral** design pattern used to process a request through a chain of handlers. Each handler decides either to process the request or to pass it to the next handler in the chain.

# Intent

To **decouple** the sender of a request from its receiver by giving multiple objects a chance to handle the request. The request is passed along a chain until one of the objects handles it.

# Structure

At a high level, the pattern consists of:

1. **Handler (abstract class or interface):**
    
    - Declares a method for handling requests.
    - Optionally holds a reference to the next handler.
2. **ConcreteHandlers:**
    
    - Implement the request-handling method.
    - Decide whether to process the request or pass it along the chain.
3. **Client:**
    
    - Sends the request to the first handler in the chain.

```plaintext
Client → Handler1 → Handler2 → Handler3 → ...
```

# Java Example

Let’s implement a simple chain for processing support requests based on severity.

```java
// Step 1: Abstract handler
abstract class SupportHandler {
    protected SupportHandler next;

    public void setNext(SupportHandler next) {
        this.next = next;
    }

    public abstract void handleRequest(String issue, int level);
}
```

```java
// Step 2: Concrete handlers
class LevelOneSupport extends SupportHandler {
    public void handleRequest(String issue, int level) {
        if (level <= 1) {
            System.out.println("LevelOneSupport handled: " + issue);
        } else if (next != null) {
            next.handleRequest(issue, level);
        }
    }
}

class LevelTwoSupport extends SupportHandler {
    public void handleRequest(String issue, int level) {
        if (level <= 2) {
            System.out.println("LevelTwoSupport handled: " + issue);
        } else if (next != null) {
            next.handleRequest(issue, level);
        }
    }
}

class LevelThreeSupport extends SupportHandler {
    public void handleRequest(String issue, int level) {
        if (level <= 3) {
            System.out.println("LevelThreeSupport handled: " + issue);
        } else {
            System.out.println("Issue not handled: " + issue);
        }
    }
}
```

```java
// Step 3: Client
public class SupportSystem {
    public static void main(String[] args) {
        SupportHandler level1 = new LevelOneSupport();
        SupportHandler level2 = new LevelTwoSupport();
        SupportHandler level3 = new LevelThreeSupport();

        level1.setNext(level2);
        level2.setNext(level3);

        level1.handleRequest("Password reset", 1);
        level1.handleRequest("Network issue", 2);
        level1.handleRequest("Database down", 3);
        level1.handleRequest("Data center offline", 4);
    }
}
```

## Output:

```
LevelOneSupport handled: Password reset
LevelTwoSupport handled: Network issue
LevelThreeSupport handled: Database down
Issue not handled: Data center offline
```

# Benefits

- **Loose coupling** between sender and receivers.
- **Flexible**: You can add or reorder handlers dynamically.
- Promotes **Single Responsibility Principle**: Each handler addresses a specific type of request.

# Drawbacks

- The request may **go unhandled** if no handler deals with it.
- Can make debugging or tracing flow harder in complex chains.

# Common Uses

- UI event handling systems
- Logging frameworks (e.g., different log levels)
- Middleware or request-processing pipelines
- Permission or validation chains