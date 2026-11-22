---
tags:
  - Career
---
# Dependency injection

- A design pattern where dependencies are provided to a class instead of being created within the class.
- Promotes loose coupling, making the code more modular, testable, and maintainable.
- Reduces hard-coded dependencies and improves flexibility in managing object lifecycles.
# IOC
- A broader design principle that shifts control of object creation and flow from the application logic to a framework or container.
- Encourages decoupling by allowing an external entity (like a DI container) to manage component instantiation and dependencies.
- Improves modularity and testability of applications.

# 3 design patterns

## Decorator
- A structural design pattern that allows behavior to be dynamically added to objects without modifying their code.
- Uses composition instead of inheritance to extend functionality.
- Commonly implemented by wrapping an object with another object that provides additional features.
## Factory
- A creational design pattern that provides an interface for creating objects without specifying their concrete classes.
- Helps encapsulate object creation logic and promotes loose coupling.
## Visitor
- A behavioral design pattern that separates algorithms from the objects on which they operate.
- Enables adding new operations to existing object structures without modifying their classes.
- Uses a "visitor" class that implements operations and a "visit" method in elements to accept the visitor.

# Testing frameworks

## Spring mockmvc
- A testing framework in Spring that allows developers to test Spring MVC controllers without starting a full web server.
- Enables simulating HTTP requests and validating responses in a controlled environment.

## Mockito
- A popular Java framework for mocking dependencies in unit tests.
- Allows developers to create mock objects and define expected behavior without requiring actual implementations.
- Supports features like when(...).thenReturn(...) for stubbing methods and verify(...) for interaction verification.
- Helps isolate unit tests by replacing real dependencies, making tests more reliable and faster.
# Microservices

- An architectural style where an application is built as a collection of small, independent services.
- Each microservice handles a specific business function and can be developed, deployed, and scaled independently.
- Services communicate via APIs (e.g., REST, gRPC, or messaging queues).
- Improves scalability and flexibility but introduces challenges like service coordination, data consistency, and increased operational complexity.