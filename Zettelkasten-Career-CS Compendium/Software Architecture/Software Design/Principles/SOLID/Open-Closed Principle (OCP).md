---
tags:
  - FORMAT
  - SoftwareDesign
  - "#SECONDARY_BRANCH"
---
**Definition**: Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

In simpler terms, you should be able to **add new functionality** to a class **without changing its existing code**.

# What Does "Open for Extension, Closed for Modification" Mean?

- **Open for extension**: You should be able to change the behavior of a module (e.g., add new features or handle new cases).
- **Closed for modification**: You should not have to modify the existing source code of that module to do so.

This is important because **modifying existing code is risky**. It can introduce bugs into otherwise stable systems. By designing code that can be extended rather than changed, you keep older, tested logic intact while still allowing for growth.

# How to Achieve OCP in Practice

The most common way to follow OCP is by using **abstraction**:

- Define behaviour in **interfaces or abstract classes**
    
- Depend on those abstractions in your logic
    
- Create **new implementations** of those abstractions when behaviour needs to change
    

You avoid touching the original class, and instead **inject new behaviour** via polymorphism.

## Example: Violating OCP

Suppose you have a `DiscountCalculator` class:

```java
public class DiscountCalculator {
    public double calculateDiscount(String customerType) {
        if (customerType.equals("Regular")) {
            return 0.1;
        } else if (customerType.equals("VIP")) {
            return 0.2;
        } else {
            return 0.0;
        }
    }
}
```

If you want to support a new type like "SuperVIP", you have to **modify** the existing code. This violates OCP.

## Refactored with OCP

```java
public interface DiscountStrategy {
    double getDiscount();
}

public class RegularDiscount implements DiscountStrategy {
    public double getDiscount() {
        return 0.1;
    }
}

public class VIPDiscount implements DiscountStrategy {
    public double getDiscount() {
        return 0.2;
    }
}

public class SuperVIPDiscount implements DiscountStrategy {
    public double getDiscount() {
        return 0.3;
    }
}

public class DiscountCalculator {
    public double calculateDiscount(DiscountStrategy strategy) {
        return strategy.getDiscount();
    }
}
```

Now, if you want to support new types like `StudentDiscount` or `HolidayDiscount`, you don’t touch the existing `DiscountCalculator` logic—you just **add a new class** that implements `DiscountStrategy`.

# Why OCP Matters

1. **Reduces Risk**: You don’t have to tamper with tested code to introduce new functionality.
2. **Supports Scalability**: As requirements evolve, you extend behaviour without rewriting core components.
3. **Improves Code Reusability**: New behaviour lives in isolated, reusable units (classes or modules).
4. **Enables Plug-and-Play Architecture**: You can inject new behaviour dynamically at runtime.
# Common Techniques Used

- Interfaces and abstract classes
- Dependency Injection
- Strategy Pattern
- Template Method Pattern
- Factory Pattern
- Polymorphism
# Real-World Analogy

Think of a **smartphone operating system**:

- The OS doesn’t need to be modified every time a new app is added.
- Instead, the OS provides an interface (APIs), and developers can **extend the system** by writing apps that interact with those APIs.
- The core system stays **closed for modification**, but the functionality is **open for extension** via apps.

---

The Open-Closed Principle is about building **flexible software** that can grow without collapsing under the weight of change.