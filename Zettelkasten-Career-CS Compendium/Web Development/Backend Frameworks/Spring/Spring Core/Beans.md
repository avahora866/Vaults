---
tags:
  - Spring
  - LEAF
---
Spring Framework Definition:
"_In Spring, the objects that form the backbone of your application and that are managed by the [[IoC (Inversion of Control) and Dependency Injection (DI)|Spring IoC container]] are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a [[IoC (Inversion of Control) and Dependency Injection (DI)|Spring IoC container]]._"

# Scope
The number of bans instantiated by the IoC container is by default one (Singleton) but can be modified by the `@Scope` annotation

- Scope("singleton") → Your bean will be a singleton, there will only be one instance.
- Scope("prototype") → Every time someone needs a reference to your bean, Spring will create a new one. (There’s a couple of caveats here, like injecting prototypes in singletons, though).
- Scope("session") → There will be one bean created for every user HTTP session.
- etc.

The `@Bean` annotation is typically used on methods inside the `@Configuration` class that return an instantiated Object

## Lazy Initialized
#TODO 