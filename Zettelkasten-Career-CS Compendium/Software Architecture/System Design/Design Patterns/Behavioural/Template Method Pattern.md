---
tags:
  - DesignPattern
---
The **Template Method** is a **behavioral** design pattern that defines the **skeleton** of an algorithm in a base class and allows subclasses to override specific steps without changing the overall structure.

# Intent

- Define the outline (or "template") of an algorithm in a method.
- Allow subclasses to redefine certain steps of the algorithm without changing its structure.

# Motivation

When you have an algorithm that has a fixed sequence of steps, but some of those steps may vary depending on the context (e.g., business logic, user preferences, file formats), the Template Method pattern lets you avoid duplicating the sequence by abstracting the invariant parts into a base class, and allowing the variant parts to be customized by subclasses.

# Structure

1. **AbstractClass**: Defines the template method and some or all of its steps as abstract or hook methods.
2. **ConcreteClass**: Implements the specific steps of the algorithm.

```plaintext
  AbstractClass
  --------------
  + templateMethod()
  + stepOne()
  + stepTwo()
  + optionalHook()

        /\
         |
ConcreteClassA
ConcreteClassB
```

The `templateMethod()` is the final method that orchestrates the algorithm. Subclasses can override certain steps but cannot override the template method itself.

# Java Example

Let's say we are building a system to prepare beverages like tea and coffee. The steps to prepare a beverage are mostly the same, but some steps like brewing and adding condiments differ.

## Step 1: Abstract Class

```java
abstract class Beverage {
    // Template method
    public final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        if (customerWantsCondiments()) {
            addCondiments();
        }
    }

    private void boilWater() {
        System.out.println("Boiling water");
    }

    private void pourInCup() {
        System.out.println("Pouring into cup");
    }

    // Steps to be implemented by subclasses
    protected abstract void brew();
    protected abstract void addCondiments();

    // Hook: Subclasses can override to control flow
    protected boolean customerWantsCondiments() {
        return true;
    }
}
```

## Step 2: Concrete Classes

```java
class Tea extends Beverage {
    protected void brew() {
        System.out.println("Steeping the tea");
    }

    protected void addCondiments() {
        System.out.println("Adding lemon");
    }
}
```

```java
class Coffee extends Beverage {
    protected void brew() {
        System.out.println("Dripping coffee through filter");
    }

    protected void addCondiments() {
        System.out.println("Adding sugar and milk");
    }

    protected boolean customerWantsCondiments() {
        // Here, this could be based on user input
        return false; // e.g., customer declined condiments
    }
}
```

## Step 3: Client Code

```java
public class TemplateMethodDemo {
    public static void main(String[] args) {
        Beverage tea = new Tea();
        System.out.println("Preparing tea...");
        tea.prepareRecipe();

        System.out.println();

        Beverage coffee = new Coffee();
        System.out.println("Preparing coffee...");
        coffee.prepareRecipe();
    }
}
```

## Output:

```
Preparing tea...
Boiling water
Steeping the tea
Pouring into cup
Adding lemon

Preparing coffee...
Boiling water
Dripping coffee through filter
Pouring into cup
```

# Benefits

- **Code reuse**: Common logic is centralized in the abstract class.
- **Consistency**: The overall algorithm structure is consistent and cannot be altered by subclasses.
- **Flexibility**: Subclasses can customize only the steps they care about.
- **Control**: Template method enforces a consistent sequence of steps.

# Drawbacks

- **Inheritance tight coupling**: Requires inheritance, which can lead to less flexible code compared to composition-based patterns like Strategy.
- **Less readable**: If there are too many hooks and abstract steps, it may be hard to follow the logic.

# Common Use Cases

- Frameworks that define a lifecycle or pipeline (e.g., application bootstrapping).
- File parsers that share a reading process but differ in interpretation.
- GUI libraries where event handling follows a pattern (e.g., window setup).
- Unit testing frameworks (e.g., `JUnit` uses `@Before`, `@After`, `@Test` methods similar to template method design).

# Related Patterns

- **Strategy**: Template uses inheritance, Strategy uses composition. Template defines the skeleton, Strategy allows full control over the algorithm.
- **Factory Method**: Often used together to delegate object creation within template steps.