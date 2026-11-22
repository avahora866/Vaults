---
tags:
  - FORMAT
  - SoftwareDesign
  - "#SECONDARY_BRANCH"
---
**Definition**: Clients should not be forced to depend on interfaces they do not use

This principle is all about **designing focused, minimal interfaces** so that implementing classes or consuming clients **only need to worry about the functionality they actually care about**.

It’s the "I" in SOLID and exists to prevent **fat interfaces**—interfaces that try to do too much and force classes to implement irrelevant methods.

# What Is a "Fat Interface"?

A **fat interface** is one that declares a wide set of methods, often meant to serve different types of clients or scenarios. When a class is forced to implement methods it doesn't need, it leads to:

- **Empty implementations** (`return null`, `// not used`)
- **Confusing code** with meaningless responsibilities
- **Tight coupling** between unrelated parts of the system

This violates the principle of **high cohesion** and makes code more brittle.

# Example: Violating ISP

```java
public interface Machine {
    void print(Document doc);
    void scan(Document doc);
    void fax(Document doc);
}
```

This might seem fine at first glance, but what if you're building a class for a basic printer that **can only print**?

```java
public class OldSchoolPrinter implements Machine {
    public void print(Document doc) {
        // actual print logic
    }

    public void scan(Document doc) {
        // not supported
        throw new UnsupportedOperationException();
    }

    public void fax(Document doc) {
        // not supported
        throw new UnsupportedOperationException();
    }
}
```

Here, `OldSchoolPrinter` is **forced** to implement irrelevant methods. That’s a violation of ISP.

# Refactored to Follow ISP

Break the interface down into more focused ones:

```java
public interface Printer {
    void print(Document doc);
}

public interface Scanner {
    void scan(Document doc);
}

public interface Fax {
    void fax(Document doc);
}
```

Now classes only implement what they need:

```java
public class SimplePrinter implements Printer {
    public void print(Document doc) {
        // print logic
    }
}

public class MultiFunctionPrinter implements Printer, Scanner, Fax {
    public void print(Document doc) { /* ... */ }
    public void scan(Document doc) { /* ... */ }
    public void fax(Document doc)  { /* ... */ }
}
```

This makes the system **more modular, easier to extend**, and more intuitive to work with.

# Why ISP Matters

1. **Improves cohesion**: Each interface has a single responsibility.
2. **Avoids forcing empty implementations**: You no longer have methods that are irrelevant to a class’s purpose.
3. **Enhances flexibility**: You can mix and match behaviors via multiple small interfaces.
4. **Supports scalability**: Adding a new kind of capability doesn’t affect existing clients.
5. **Makes mocking and testing easier**: You can mock just the functionality you care about.
# Interface Segregation vs. Single Responsibility

- **SRP** is about class responsibilities.
- **ISP** is about interface responsibilities.

They’re both about keeping things **focused**, but at different levels of abstraction.

# Real-World Analogy

Think of a **restaurant menu**:

- A vegetarian customer shouldn’t be handed a menu with seafood and steak—they just want to see the vegetarian options.
- A bartender doesn't need to know how to cook the food.

Instead of a giant menu (interface) with everything, **each person gets a tailored, relevant subset**.

---

To sum up: **The Interface Segregation Principle helps you build clean, focused, role-specific abstractions** that avoid unnecessary dependencies and empower flexibility.