---
tags:
  - Architecture
---
Hexagonal Architecture is a software design pattern proposed by **Alistair Cockburn** to create applications that are **independent of delivery mechanisms and external systems**. It promotes a **clean separation** between the **core application logic** and the **outside world** (UIs, databases, APIs, etc.).

The goal is to make the **application core highly testable, adaptable, and maintainable**, by isolating it from external dependencies.

# Core Concepts

Hexagonal architecture consists of:

- **Application Core**: Contains the domain logic, use cases, and application rules.
- **Ports**: Abstract interfaces representing operations the application core can perform or expects to be performed.
- **Adapters**: Implementations of those ports, allowing interaction with the outside world (e.g., databases, web APIs, UIs, messaging systems).
- **Inward-facing ports**: Interfaces through which external actors (e.g., users or APIs) invoke application logic.
- **Outward-facing ports**: Interfaces the core uses to call external services (e.g., a database or a payment gateway).

The name _"hexagonal"_ comes from a diagram convention that uses hexagons to visualize how the application core is surrounded by adapters via ports. The shape is symbolic—the actual number of sides is unimportant.

# Structure Breakdown

## Application Core

- **Domain Entities**: The business objects (e.g., `User`, `Order`, `Invoice`)
- **Use Cases / Services**: Encapsulate business processes (e.g., `RegisterUser`, `ProcessPayment`)
- **Ports (Interfaces)**: Define contracts for interactions

## Adapters

- **Primary (Driving)**: Invoke application logic
    - UI (Web, CLI, mobile)
    - APIs (HTTP Controllers)
- **Secondary (Driven)**: Are called by the application
    - Database (Repositories)
    - External APIs (Payment gateways, email services)

# Example Scenario: User Registration System

## Core Domain

```java
// Port: defines application input
public interface UserRegistrationUseCase {
    void registerUser(UserDto userDto);
}
```

```java
// Domain service implementing the use case
public class UserRegistrationService implements UserRegistrationUseCase {

    private final UserRepository userRepository; // This is an output port

    public UserRegistrationService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public void registerUser(UserDto userDto) {
        if (userRepository.existsByEmail(userDto.getEmail())) {
            throw new RuntimeException("Email already exists");
        }
        User user = new User(userDto.getEmail(), userDto.getPassword());
        userRepository.save(user);
    }
}
```

## Adapter: Web (Driving Adapter)

```java
@RestController
public class UserController {

    private final UserRegistrationUseCase registrationUseCase;

    public UserController(UserRegistrationUseCase registrationUseCase) {
        this.registrationUseCase = registrationUseCase;
    }

    @PostMapping("/register")
    public ResponseEntity<String> register(@RequestBody UserDto dto) {
        registrationUseCase.registerUser(dto);
        return ResponseEntity.ok("User registered");
    }
}
```

## Adapter: Persistence (Driven Adapter)

```java
@Repository
public class JpaUserRepository implements UserRepository {

    @Autowired
    private SpringDataUserRepository springDataRepo;

    @Override
    public boolean existsByEmail(String email) {
        return springDataRepo.existsByEmail(email);
    }

    @Override
    public void save(User user) {
        springDataRepo.save(user);
    }
}
```

## Port: Repository Interface

```java
public interface UserRepository {
    boolean existsByEmail(String email);
    void save(User user);
}
```

In this setup:

- `UserController` is a **driving adapter** (calls into the application)
- `JpaUserRepository` is a **driven adapter** (called by the application)
- `UserRegistrationService` is the application core
- `UserRepository` and `UserRegistrationUseCase` are **ports**

# Benefits

- **Testability**: The application core can be tested without any infrastructure (mock the ports).
- **Maintainability**: Easy to replace external systems (e.g., swap databases).
- **Decoupling**: Domain logic is isolated from frameworks and infrastructure.
- **Flexibility**: Supports different interfaces (web, CLI, REST, [[gRPC|gRPC]]) without changing business logic.

# Comparison to Layered Architecture

# When to Use

Use hexagonal architecture when:

- You want a clean separation of concerns
- Your domain logic is valuable and must be independent
- The system interacts with multiple external systems (e.g., DB, queues, APIs)
- You want better testability and long-term flexibility

Avoid it in:

- Very small applications where the overhead is not justified

# Summary

Hexagonal architecture:

- Emphasizes **inversion of control** and **port-adapter separation**
- Keeps your **business logic clean and independent**
- Allows flexibility to **swap or add interfaces** (CLI, REST, DBs, etc.)
- Makes **testing and evolution easier**