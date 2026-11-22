---
tags:
  - Spring
  - SECONDARY_BRANCH
---
Spring's event system allows different parts of an application to communicate in a loosely coupled way. It follows the **Publisher-Subscriber** pattern, where components can publish and listen to application-wide events without being directly dependent on each other.

---

# 1️. Key Components of the Spring Event System

## 1 ApplicationEvent (The Event Object)

* Represents an event in Spring.  
* Can be a built-in Spring event (e.g., `ContextRefreshedEvent`) or a custom-defined event.  
* Since Spring 4.2, you can use any POJO (Plain Old Java Object) as an event, so extending `ApplicationEvent` is **optional**.

**Example of a Custom Event:**

```java
public class UserCreatedEvent {
    private final String username;
    
    public UserCreatedEvent(String username) {
        this.username = username;
    }
    
    public String getUsername() {
        return username;
    }
}
```

---

## 2 ApplicationEventPublisher (The Publisher)

* Used to publish (send) events in Spring.  
* Can be injected into any Spring-managed component (e.g., service, controller).

**Example: Publishing an Event**

```java
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Service;

@Service
public class UserService {
    private final ApplicationEventPublisher eventPublisher;

    public UserService(ApplicationEventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }

    public void registerUser(String username) {
        // Business logic for user registration
        System.out.println("User registered: " + username);
        
        // Publish event
        eventPublisher.publishEvent(new UserCreatedEvent(username));
    }
}
```

---

## 3 @EventListener (The Listener)

* Listens for specific events and handles them asynchronously or synchronously.  
* Declared as a Spring `@Component`.

**Example: Listening to the Custom Event**

```java
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class UserEventListener {
    
    @EventListener
    public void handleUserCreatedEvent(UserCreatedEvent event) {
        System.out.println("Handling user created event for: " + event.getUsername());
    }
}
```

---

# 2️ Asynchronous Event Handling (Optional)

By default, event listeners are synchronous. If you want them to be executed asynchronously, you can enable async processing:

## Step 1: Enable Async Execution

Add `@EnableAsync` to your Spring Boot application class:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;

@SpringBootApplication
@EnableAsync
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

## Step 2: Mark the Listener as Async

```java
import org.springframework.context.event.EventListener;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Component;

@Component
public class AsyncUserEventListener {
    
    @Async
    @EventListener
    public void handleUserCreatedEvent(UserCreatedEvent event) {
        System.out.println("Handling asynchronously: " + event.getUsername());
    }
}
```

---

# 3️ When to Use Spring Events?

* **Decoupling Components** → Services don’t need to directly call each other.  
* **Cross-cutting Concerns** → Logging, auditing, notifications, etc.  
* **Asynchronous Processing** → Running background tasks without blocking main logic.

---

# 4️ Built-in Spring Events (Bonus)

Spring provides several built-in events, such as:

* `ContextRefreshedEvent` → Triggered when the application starts/restarts.  
* `ContextClosedEvent` → Triggered when the application shuts down.  
* `ApplicationReadyEvent` → Triggered when the application is fully ready.