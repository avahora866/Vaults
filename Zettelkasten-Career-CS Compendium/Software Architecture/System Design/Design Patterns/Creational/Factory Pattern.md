---
tags:
  - DesignPattern
---
The **Factory Pattern** is a **creational design pattern** used in software development to create objects without specifying the exact class of the object that will be created. It provides a way to delegate the instantiation logic to subclasses or separate components.

# Core Idea

Instead of instantiating classes directly using `new`, you use a **factory method** or **class** that decides which subclass or type to instantiate based on input parameters or internal logic.

---

# Why Use It

- **Encapsulation of object creation**: Keeps the creation logic separate from the business logic.
- **Scalability**: Easy to add new types without changing existing code.
- **Decoupling**: Code depends on interfaces/abstract classes, not concrete implementations.

---

# Structure

## Participants:

- **Product**: Common interface or abstract class for all created objects.
- **Concrete Products**: Implement the Product interface.
- **Creator**: Declares the factory method.
- **Concrete Creators**: Implement the factory method.

---

# Types

1. **Simple Factory** (not a GoF pattern but common)
2. **Factory Method** (GoF pattern)
3. **Abstract Factory** (GoF pattern)

---

# Java Example – Factory Method Pattern

Let’s say you’re building a shape drawing tool.

```java
// Product interface
interface Shape {
    void draw();
}

// Concrete Products
class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Square implements Shape {
    public void draw() {
        System.out.println("Drawing Square");
    }
}

// Factory (Creator)
class ShapeFactory {
    public Shape getShape(String type) {
        if (type == null) return null;
        return switch (type.toLowerCase()) {
            case "circle" -> new Circle();
            case "square" -> new Square();
            default -> throw new IllegalArgumentException("Unknown shape type");
        };
    }
}
```

Usage:

```java
public class Main {
    public static void main(String[] args) {
        ShapeFactory factory = new ShapeFactory();
        Shape shape1 = factory.getShape("circle");
        shape1.draw();  // Output: Drawing Circle

        Shape shape2 = factory.getShape("square");
        shape2.draw();  // Output: Drawing Square
    }
}
```

---

# Abstract Factory (Advanced Version)

Use when you need to create **families of related objects** without knowing their concrete classes.

## Example Use Case

UI toolkit with themes:

- **DarkThemeFactory** → DarkButton, DarkScrollbar
- **LightThemeFactory** → LightButton, LightScrollbar

---

# When to Use

- Code should not depend on how objects are created.
- You have multiple classes implementing the same interface, and the client code should not know the difference.
- You want to centralize and control object creation.

---

# Pros

- Decouples code from concrete classes
- Centralizes creation logic
- Makes code easier to test (you can inject mock factories)

---

# Cons

- Can introduce unnecessary complexity if overused
- Might lead to many small classes or factory classes
