---
tags:
  - SoftwareDesign
---
# God Object

A **God Object** is when a single class or module takes on too much responsibility.  
Instead of following the principle of **separation of concerns**, one "god-like" object knows everything and controls everything.  
This makes the code hard to maintain, debug, and extend because **one change can accidentally affect everything else**.

Problem it causes:

- Tight coupling
    
- Fragile codebase
    
- Hard to understand and test
    

# Spaghetti Code

**Spaghetti Code** is unstructured, messy code with no clear flow or organization.  
Functions and classes are tangled together, often with lots of unexpected dependencies and side effects.  
It’s called "spaghetti" because, like a plate of noodles, everything is twisted together with no clear beginning or end.

Problem it causes:

- Very hard to read or modify
    
- High risk of introducing bugs when changing anything
    
- Difficult to reuse code
    

# Magic Numbers

**Magic Numbers** are unexplained numeric constants that appear directly in the code without context.  
Instead of giving the number a meaningful name, it just shows up randomly, making the code hard to understand.

Example:  
Instead of writing

```java
if (userAge > 18) { /*...*/ }
```

better to write

```java
final int LEGAL_ADULT_AGE = 18;
if (userAge > LEGAL_ADULT_AGE) { /*...*/ }
```

Problem it causes:

- Makes code harder to read and maintain
    
- Harder to update if the number needs to change later
    
- Increases the chance of bugs from misunderstanding what the number means
    