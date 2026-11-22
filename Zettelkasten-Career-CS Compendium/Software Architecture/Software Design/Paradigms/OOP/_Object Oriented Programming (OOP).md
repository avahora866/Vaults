---
tags:
  - OOP
  - "#TRUNK"
---
The four principles of object-oriented programming (abstraction, inheritance, encapsulation, and polymorphism) are features that - if used properly - can help us write more testable, flexible, and maintainable code.
 
 Object design is actually comprised of three parts:

- Analysis: where we learn a domain, discover functional and non-functional requirements, and turn them into use cases
- Design: where we utilize a design method like Responsibility-Driven Design to convert requirements into roles, responsibilities, and collaborations, we consider [[Design Patterns|design patterns]] and their trade-offs, we make architectural decisions - deciding on architectural styles and patterns
- Programming: where we map designs into tested, flexible, maintainable code

![https://d33wubrfki0l68.cloudfront.net/0f0761c4c043de58d16240000824d98bdef84240/66972/img/blog/object-oriented/programming/4-principles/object-design.svg](https://d33wubrfki0l68.cloudfront.net/0f0761c4c043de58d16240000824d98bdef84240/66972/img/blog/object-oriented/programming/4-principles/object-design.svg)

Object design is comprised of three parts: Analysis, Design, and Programming

Only learning the programming aspect, I found myself confused about what good design looked like, how to turn requirements into objects, and when and why to use concepts like abstraction, encapsulation, inheritance, and polymorphism. Yes, I could somewhat remember what these concepts were, but knowing when and why to use them was unclear to me.

![The four main principles of object-oriented programming (abstraction, inheritance, encapsulation, and polymorphism). The core principle is abstraction. Without it, the others couldn't exist.](https://d33wubrfki0l68.cloudfront.net/1efe60cb6e778c3aa6d3606307f89ef44cfa8786/4b3ca/img/blog/object-oriented/programming/4-principles/principles-of-oo.png)

The four main principles of object-oriented programming (abstraction, inheritance, encapsulation, and polymorphism). The core principle is abstraction. Without it, the others couldn't exist.

# Abstraction
[[Software Architecture/Software Design/Paradigms/OOP/Abstraction|Abstraction]] is a concept in programming and design that involves simplifying complex systems by hiding unnecessary details and exposing only the essential features. It allows users to interact with a system at a high level without needing to understand its inner workings. By focusing on what something does rather than how it does it, abstraction enhances usability, maintainability, and flexibility. This approach reduces complexity, promotes clear communication, and enables easier modifications and extensions, as changes in implementation do not affect the overall system's functionality.

# Inheritance

[[Inheritance]] is a programming concept where a new class is created based on an existing class, inheriting its properties and behaviors. This promotes code reuse and establishes a hierarchical relationship between classes, allowing shared functionality to be defined once and extended or modified in derived classes. Inheritance enhances maintainability and consistency, but excessive use can lead to tight coupling and complexity.

# Encapsulation
[[Software Architecture/Software Design/Paradigms/OOP/Encapsulation|Encapsulation]] is the principle of bundling data and methods that operate on that data within a single unit, typically a class. It restricts direct access to some components, ensuring data integrity and protecting the internal state of an object. By exposing only necessary information through public methods, encapsulation enhances security, maintainability, and flexibility, enabling controlled modifications without affecting other parts of the system.

# Polymorphism
[[Software Architecture/Software Design/Paradigms/OOP/Polymorphism|Polymorhism]] is the ability of different objects to be treated as instances of the same class through a common interface. It allows methods to be defined in multiple forms, enabling the same operation to behave differently on different classes. Polymorphism enhances flexibility and maintainability by promoting loose coupling and enabling dynamic method binding, allowing programs to be more adaptable to changes and extensions without modifying existing code.


# Conclusion
Abstraction, encapsulation, inheritance, and polymorphism are four of the main principles of object-oriented programming.
- Abstraction lets us selectively focus on the high-level and abstract way the low-level details.
- Inheritance is about code reuse, not hierarchies.
- Encapsulation keeps state private so that we can better enforce business rules, protect model invariants, and develop a single source of truth for related data and logic.
- Polymorphism provides the ability for us to design for dynamic runtime behavior, easy extensibility, and substitutability