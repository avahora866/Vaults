---
tags:
  - DesignPattern
---
The **Decorator** pattern is a **structural** design pattern that allows you to **dynamically add behavior or responsibilities** to individual objects **without modifying their code**. It wraps the original object in a set of decorator classes, each adding new behavior.

# Intent

- Attach additional responsibilities to an object dynamically.
- Provide a flexible alternative to subclassing for extending functionality.

# Motivation

Imagine you’re designing a coffee shop application. A `Coffee` might be plain or come with milk, sugar, whipped cream, etc. If you model every combination using inheritance, you’d end up with classes like `CoffeeWithMilk`, `CoffeeWithMilkAndSugar`, `CoffeeWithCreamAndSugar`, and so on — a **combinatorial explosion**.

The Decorator pattern solves this by letting you wrap a `Coffee` object in decorator classes that each add one piece of behavior (e.g., milk, sugar, etc.), dynamically and flexibly.

# Structure

```plaintext
         Component
        +----------+
        | operation|
        +----------+
             ^
             |
    +-----------------+
    |                 |
ConcreteComponent  Decorator
  (e.g., Coffee)   +----------+
                  | operation| (delegates to component)
                  +----------+
                        ^
                        |
             +--------------------+
             | ConcreteDecoratorA |
             | ConcreteDecoratorB |
```

## Participants

- **Component**: Interface or abstract class defining the original object’s interface.
- **ConcreteComponent**: The object to which additional responsibilities can be added.
- **Decorator**: Implements the component interface and holds a reference to a `Component`.
- **ConcreteDecorator**: Adds responsibilities to the component.

# Java Example

Let’s model a customizable coffee system.

## Step 1: Component Interface

```java
interface Coffee {
    String getDescription();
    double getCost();
}
```

## Step 2: ConcreteComponent

```java
class SimpleCoffee implements Coffee {
    public String getDescription() {
        return "Simple Coffee";
    }

    public double getCost() {
        return 2.0;
    }
}
```

## Step 3: Decorator Base Class

```java
abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee decoratedCoffee) {
        this.decoratedCoffee = decoratedCoffee;
    }

    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

## Step 4: Concrete Decorators

```java
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    public double getCost() {
        return decoratedCoffee.getCost() + 0.5;
    }
}

class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Sugar";
    }

    public double getCost() {
        return decoratedCoffee.getCost() + 0.3;
    }
}
```

## Step 5: Client

```java
public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " $" + coffee.getCost());

        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);
        coffee = new SugarDecorator(coffee);  // double sugar

        System.out.println(coffee.getDescription() + " $" + coffee.getCost());
    }
}
```

## Output

```
Simple Coffee $2.0
Simple Coffee, Milk, Sugar, Sugar $3.1
```

# Benefits

- **Flexible extension**: Add functionality without modifying the original class.
- **Open/Closed Principle**: Classes are open for extension but closed for modification.
- **Runtime composition**: Behavior can be composed at runtime.

# Drawbacks

- **Lots of small classes**: Every new feature requires a new decorator.
- **Harder to debug**: Multiple layers of wrappers can obscure what's actually happening.

# Use Cases

- UI frameworks: scrollbars, borders, shadows around components.
- Logging: add logging behavior to service classes.
- Security: add role-based access control dynamically.
- Streams in Java: `BufferedInputStream`, `DataInputStream`, etc., are classic decorator examples.

# Related Patterns

- **Adapter**: Changes the interface; Decorator preserves it but adds behavior.
- **Composite**: Works with whole-part structures; Decorator is more about embellishing individual objects.
- **Proxy**: Controls access; Decorator adds behavior.
- **Chain of Responsibility**: Similar structure, but each handler decides whether to pass along the request.