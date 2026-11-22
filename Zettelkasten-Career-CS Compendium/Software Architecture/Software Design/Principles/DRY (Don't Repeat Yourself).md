---
tags:
  - FORMAT
  - SoftwareDesign
  - "#SECONDARY_BRANCH"
---
**Definition**:  Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

In simple terms, DRY emphasizes the idea that **repetition in code should be avoided**. Instead of writing the same code multiple times, you should extract common functionality into a single place, making your code easier to maintain, extend, and debug.

The core idea behind DRY is **eliminating redundancy** by ensuring that each piece of knowledge or logic is written only once in the system. When a change is required, you should modify it in only one place.

# Why DRY is Important

1. **Easier Maintenance**: When you need to change behavior or logic, you only need to change it in one location. If you repeat code, you’ll have to find all instances and update them, which increases the risk of introducing errors or inconsistencies.
    
2. **Consistency**: Keeping a single source of truth ensures that functionality is always consistent. If logic is repeated in multiple places and one instance is updated, it could lead to bugs, as other instances would be outdated.
    
3. **Readability**: Removing repetition improves the clarity of the code. It makes the codebase more concise and understandable, reducing noise and focusing on what’s important.
    
4. **Testability**: When functionality is abstracted into a single, reusable place, it’s easier to write unit tests for it, reducing the chances of redundant tests across different code sections.
    
5. **Extensibility**: DRY allows new features to be added easily because modifications only need to happen in one place, and everything else using that functionality will automatically benefit from it.
    

# DRY in Practice

## Example: Without DRY

Consider this simple scenario where you need to calculate discounts for customers. In your code, you might have the same logic duplicated in multiple places.

```java
public class RegularCustomer {
    public double calculateDiscount(double price) {
        return price * 0.10; // 10% discount
    }
}

public class VIPCustomer {
    public double calculateDiscount(double price) {
        return price * 0.20; // 20% discount
    }
}

public class SeniorCustomer {
    public double calculateDiscount(double price) {
        return price * 0.15; // 15% discount
    }
}
```

In this example, the **discount calculation logic** is repeated in multiple places, which violates DRY. If the calculation method needs to change (e.g., you decide that discounts should be based on different tiers or change their formula), you’ll have to update it in multiple places. This increases maintenance complexity.

## Refactored with DRY

To follow the DRY principle, you could abstract the discount logic into a reusable method or class:

```java
public interface DiscountStrategy {
    double calculateDiscount(double price);
}

public class RegularCustomerDiscount implements DiscountStrategy {
    public double calculateDiscount(double price) {
        return price * 0.10;
    }
}

public class VIPCustomerDiscount implements DiscountStrategy {
    public double calculateDiscount(double price) {
        return price * 0.20;
    }
}

public class SeniorCustomerDiscount implements DiscountStrategy {
    public double calculateDiscount(double price) {
        return price * 0.15;
    }
}

public class DiscountCalculator {
    private DiscountStrategy discountStrategy;

    public DiscountCalculator(DiscountStrategy discountStrategy) {
        this.discountStrategy = discountStrategy;
    }

    public double calculateDiscount(double price) {
        return discountStrategy.calculateDiscount(price);
    }
}
```

In this version, the logic for calculating discounts is **delegated** to strategy objects, and the `DiscountCalculator` class can work with any of the discount strategies. The **discount calculation logic** is now **reusable and centralized**, following DRY principles.

## Key Techniques to Implement DRY

1. **Abstraction**: Extract repeated code into common methods, classes, or modules.
    
2. **Functions and Methods**: Encapsulate repeated logic into reusable functions or methods.
    
3. **Design Patterns**: Use design patterns like Strategy, Factory, or Template Method to extract common logic and make it reusable.
    
4. **Data Structures**: Use arrays, lists, maps, or other data structures to represent data that might be repeated in various places.
    
5. **Refactoring**: Regularly review and refactor your code to eliminate duplication.
    

# Common Pitfalls of DRY

While DRY is an important principle, there are also situations where it can lead to unintended complexity or harm:

1. **Premature Optimization**: Don’t over-engineer solutions by abstracting things too early. Sometimes, a small amount of repetition is okay in early stages, especially if it improves clarity.
    
2. **Not All Duplication Should Be Eliminated**: In some cases, different implementations may warrant some duplication. If two sections of the code solve **different problems** (even though the code is similar), merging them could lead to confusion or overly complex abstractions.
    
3. **Over-Abstraction**: In an attempt to avoid duplication, developers can fall into the trap of over-abstracting the code. This might introduce complexity and make the code harder to understand. Use abstraction thoughtfully and avoid trying to abstract code that isn't used in multiple places.
    

# DRY vs. KISS (Keep It Simple, Stupid)

While **DRY** focuses on **eliminating redundancy**, the **KISS principle** is about keeping code **simple and easy to understand**. Overusing DRY can sometimes lead to **over-complexity**. It's important to find a balance between removing redundancy and not making the code overly abstract and hard to understand.

# DRY in Real-World Analogies

Think of a **cookbook**:

- If each recipe (method) repeats the same steps for basic tasks like chopping, sautéing, or boiling, that’s redundant.
    
- Instead, you can write a single step for chopping and reuse it in every recipe. That’s a **DRY** approach—reuse common instructions while maintaining clarity.
    

---

In summary, **DRY** is about reducing redundancy, making code easier to maintain, extend, and understand. It's not just about **reducing lines of code** but rather ensuring that your codebase is **organized, consistent, and modular**.