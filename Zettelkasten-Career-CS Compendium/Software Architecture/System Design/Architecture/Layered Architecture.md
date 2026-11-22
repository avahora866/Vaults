---
tags:
  - Architecture
---
**Layered architecture** (also called **n-tier architecture**) is a software design pattern that organizes the system into **layers**, each with a specific role and responsibility. It’s one of the most traditional and widely used architectural patterns, especially in enterprise applications.

# Structure of Layered Architecture

Typically, a layered application has **4 main layers**:

1. **Presentation Layer (UI Layer)**
    
    - Handles user interaction (e.g., HTML/CSS, React, Android UI).
    - Validates user input and displays results.
    - Forwards requests to the next layer.
2. **Application Layer (Service Layer)**
    
    - Orchestrates the application logic.
    - Coordinates data from multiple sources (e.g., services, DB).
    - Applies business rules and workflows.
3. **Business Logic Layer (Domain Layer)**
    
    - Contains core business rules and logic.
    - Encapsulates decision-making and domain operations.
    - Doesn’t depend on UI or database.
4. **Data Access Layer (Persistence Layer)**
    
    - Handles data storage and retrieval.
    - Talks to the database or external storage.
    - Includes ORM code (e.g., JPA, Hibernate).

Each layer interacts **only with the layer directly beneath it**. For example, the UI layer talks to the application layer, which talks to the business layer, and so on.

# Example in a Java Web App

Suppose you’re building a user registration system.

### Presentation Layer (Controller)

```java
@PostMapping("/register")
public ResponseEntity<?> registerUser(@RequestBody UserDto userDto) {
    userService.register(userDto);
    return ResponseEntity.ok("User registered");
}
```

### Application Layer (Service)

```java
public void register(UserDto userDto) {
    if (userRepository.existsByEmail(userDto.getEmail())) {
        throw new RuntimeException("Email already in use");
    }
    User user = new User(userDto.getEmail(), userDto.getPassword());
    userRepository.save(user);
}
```

### Data Access Layer (Repository)

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    boolean existsByEmail(String email);
}
```

# Benefits

- **Separation of concerns**: Each layer has a clear responsibility.
- **Maintainability**: Easier to update logic without affecting unrelated layers.
- **Testability**: You can test each layer independently.
- **Reusability**: Logic in service or business layers can be reused across UIs or APIs.

# Drawbacks

- **Tight coupling between layers**: Changes in one layer might ripple through others.
- **Can become rigid**: Too much layering for small apps can feel heavy or boilerplate-heavy.
- **Performance overhead**: Extra layer hops can impact performance in high-throughput systems.

# When to Use

- Enterprise applications
- Systems with clearly defined roles and responsibilities
- Projects needing clean separation for long-term maintainability

# Alternatives

- **Hexagonal Architecture (Ports and Adapters)**: More flexible, treats external interfaces equally.
- **Clean Architecture**: Emphasizes independence of business logic from frameworks or tools.
- **Microservices**: Often use layered structure _within_ each service.
