---
tags:
  - Spring
---
- **Aspect:** module of code for a cross-cutting concern (logging, security, ..)
- **Advice:** What action is taken and when it should be applied
- **Join Point:** When to apply code during program execution
- **Pointcut:** A predicate expression for where advice should be applied
# Advice Types
- **Before advice**: run before the method
- **After finally advice**: run after the method (finally)
- **After returning advice**: run after the method (successful execution)
- **After throwing advice**: run after method (If exception is thrown)
- **Around advice**: run before and after method
# Weaving
Connecting aspects to target objects to create an advised object.
Different types of weaving:
- Compile-time
- Load-time
- Run-time
Performance: Run-time weaving is the slowest
# AOP Farmworkers
## Spring AOP
Spring provides AOP support. It is a key component of Spring and is used in Security, Transactions, Caching, etc. It uses run-time weaving of aspects
### Advantages
- Simpler to use than AspectJ
- Uses [[Proxy Pattern|Proxy]] pattern
- Can migrate to AspectJ when using the `@AspectJ` annotation
### Disadvantages
- Only supports method-level join points
- Can only apply aspects to beans created by the Spring app context
- minor performance cost for aspect execution (run-time weaving)
# AspectJ
Original AOP framework and provides complete support for AOP. Rich Support for:
- Join points: method-level, constructors
- code weaving: compile time, post-compile time and load time
### Advantages
- Supports all join points
- Works with any POJO, not just beans from app context
- Faster performance compared to Spring AOP
- Complete AOP support
### Disadvantages
- Compile-time weaving requires extra compilation step
- AspectJ pointcut syntax can become complex