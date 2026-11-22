---
tags:
  - Theory
  - LEAF
---
A web framework like Spring is a set of libraries and tools that help you build web applications faster and with less boilerplate code. Instead of writing everything from scratch—like handling HTTP requests, routing, dependency management, database connections, or security—you use the framework’s prebuilt components.

Here’s what that means in practice with Spring (specifically **Spring Framework** and its web module **Spring MVC**):

- **Request handling**: Spring maps HTTP requests (like `GET /users`) to specific Java methods in your code, so you don’t have to manually parse and route them.
- **Dependency injection**: Instead of creating and wiring up objects manually (e.g., services, repositories), Spring does it for you. This makes your code easier to test and maintain.
- **Configuration management**: You can define settings (e.g., database connections) in a central place instead of scattering them throughout your code.
- **Database integration**: Spring provides support for connecting to databases, managing transactions, and using ORMs like Hibernate.
- **Security**: Spring Security helps you add authentication and authorization without building them from scratch.
- **Modularity**: You can pick and choose only the parts you need (e.g., web, data, security).

So at a high level:

- Without a framework → you’d be writing raw HTTP handling code, setting up servers, managing object lifecycles, and wiring everything together manually.
- With Spring → you focus on writing your application logic (controllers, services, models), and the framework takes care of the plumbing.