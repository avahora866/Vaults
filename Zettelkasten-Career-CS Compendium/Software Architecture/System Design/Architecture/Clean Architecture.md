---
tags:
  - Architecture
---
**Clean Architecture** is a software design approach that emphasizes **separation of concerns** and **independence** between components. The core idea is to organize code in concentric circles, where:

- **The innermost layers are the most abstract and stable (core business logic)**.
- **The outer layers are more concrete and volatile (UI, frameworks, databases, APIs)**.
- **Dependencies always point inward**, from outer layers to inner ones.

The goal is to create systems that are:

- **Independent of frameworks**
- **Testable**
- **UI-independent**
- **Database-independent**
- **External-service-independent**

This makes the system **flexible, maintainable, and adaptable to change**.

# Layers in Clean Architecture

Imagine your system as a set of concentric circles. From innermost to outermost:

## Entities (Enterprise Business Rules)

- Core business logic.
- High-level policies that are **independent of use cases, frameworks, or UI**.
- These are pure Java/Kotlin classes in a typical Java application.

**Example**:  
`User`, `Invoice`, `BankAccount` with methods like `withdraw()`, `activate()`

## Use Cases (Application Business Rules)

- Encapsulate specific **application-level logic or workflows**.
- Orchestrate interactions between entities to fulfill a request.
- Do **not depend on UI, databases, or frameworks**.

**Example**:  
`RegisterUser`, `TransferFunds`, `GenerateInvoice`

## Interface Adapters

- Translates data between the outer layers and the use cases.
- Contains **Controllers**, **Presenters**, **Gateways**, **Repositories**.
- Implements interfaces defined by the inner layers.
- Acts as the glue between core logic and infrastructure.

**Example**:  
Spring `@RestController`, UI presenters, database mappers

## Frameworks and Drivers

- The outermost layer. Contains frameworks and tools like:
    - Spring, Hibernate, Django
    - Web frameworks, DB drivers
    - UI frameworks, cloud SDKs

This layer is allowed to depend on the other layers but **no inner layer can depend on this layer**.

# Dependency Rule

This is the most important rule: **"Dependencies must point inward."**  
Each layer can only depend on layers **more central** than itself. Never the other way around.

To enforce this, you use **interfaces** in the inner layers and **implementations** in the outer layers.

---

# Visual Diagram

```
[ Frameworks & Drivers ]
        ↑
[ Interface Adapters ]
        ↑
[ Use Cases ]
        ↑
[ Entities ]
```

Each arrow represents the direction of dependency.

---

# Code Example (Java)

## Entity Layer

```java
public class User {
    private String email;
    private boolean active;

    public void activate() {
        this.active = true;
    }

    public boolean isActive() {
        return active;
    }
}
```

## Use Case Layer

```java
public interface UserRepository {
    User findByEmail(String email);
    void save(User user);
}
```

```java
public class ActivateUserUseCase {

    private final UserRepository repository;

    public ActivateUserUseCase(UserRepository repository) {
        this.repository = repository;
    }

    public void activate(String email) {
        User user = repository.findByEmail(email);
        user.activate();
        repository.save(user);
    }
}
```

## Interface Adapter Layer

```java
@RestController
public class UserController {

    private final ActivateUserUseCase useCase;

    public UserController(ActivateUserUseCase useCase) {
        this.useCase = useCase;
    }

    @PostMapping("/activate")
    public ResponseEntity<Void> activate(@RequestParam String email) {
        useCase.activate(email);
        return ResponseEntity.ok().build();
    }
}
```

## Framework Layer

```java
@Repository
public class JpaUserRepository implements UserRepository {
    // Uses Spring Data JPA to implement findByEmail/save
}
```

---

# Benefits

- **Independent of UI/Database**: You can switch from Spring MVC to CLI or swap SQL with NoSQL easily.
- **High testability**: Core logic can be tested without the need for web servers or DBs.
- **Decoupled code**: Clear separation of concerns.
- **Maintainability**: Easier to update specific parts of the system without breaking others.

# Challenges

- **Complexity for small apps**: Might feel heavy for simple CRUD systems.
- **Initial setup**: More upfront planning and boilerplate.
- **More interfaces**: Increased indirection can add verbosity.

---

# Summary

**Clean Architecture**:

- Centralizes the **business rules**
- Uses **inward-pointing dependencies**
- Separates the **application core from frameworks, UIs, and DBs**
- Encourages **interfaces in the core, implementations in the outer layers**
- Maximizes **flexibility, testability, and maintainability**