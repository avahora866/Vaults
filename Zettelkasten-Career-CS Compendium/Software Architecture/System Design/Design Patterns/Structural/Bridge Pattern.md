---
tags:
  - DesignPattern
---
The **Bridge** pattern is a **structural** design pattern used to **decouple an abstraction from its implementation**, so that the two can vary independently.

It’s useful when both the abstraction and its implementation may evolve separately, and you don’t want a tight coupling between them.

# Intent

- Decouple abstraction from implementation.
- Enable independent evolution of abstraction and implementation hierarchies.
- Avoid explosion of subclasses that would otherwise result from combining multiple dimensions of variation.

# Motivation

Suppose you're designing a GUI framework that supports different platforms (e.g., Windows, Linux) and also different shapes (e.g., Circle, Square). Without Bridge, you might end up with:

- `WindowsCircle`, `LinuxCircle`, `WindowsSquare`, `LinuxSquare`, etc.

This leads to **combinatorial explosion**. The Bridge pattern solves this by separating the shape abstraction from the platform-specific rendering implementation.

# Structure

```plaintext
    Abstraction                      Implementor
+----------------+           +--------------------------+
|   Shape        |<>-------->|  DrawingAPI              |
+----------------+           +--------------------------+
| draw()         |           | drawCircle(x, y, r) etc. |
+----------------+           +--------------------------+
         ^                             ^
         |                             |
+----------------+          +---------------------------+
| Circle, Square |          | DrawingAPI1, DrawingAPI2  |
+----------------+          +---------------------------+
```

## Participants

- **Abstraction**: Defines the high-level control interface. Maintains a reference to the Implementor.
- **RefinedAbstraction**: Extends the interface defined by Abstraction.
- **Implementor**: Defines the interface for implementation classes.
- **ConcreteImplementor**: Implements the Implementor interface.

# Java Example

Let’s model a scenario where we draw shapes using different drawing APIs.

## Step 1: Implementor interface

```java
interface DrawingAPI {
    void drawCircle(double x, double y, double radius);
}
```

## Step 2: Concrete Implementors

```java
class DrawingAPI1 implements DrawingAPI {
    public void drawCircle(double x, double y, double radius) {
        System.out.println("API1.circle at (" + x + ", " + y + ") with radius " + radius);
    }
}

class DrawingAPI2 implements DrawingAPI {
    public void drawCircle(double x, double y, double radius) {
        System.out.println("API2.circle at (" + x + ", " + y + ") with radius " + radius);
    }
}
```

## Step 3: Abstraction

```java
abstract class Shape {
    protected DrawingAPI drawingAPI;

    protected Shape(DrawingAPI drawingAPI) {
        this.drawingAPI = drawingAPI;
    }

    public abstract void draw();             // high-level operation
    public abstract void resizeByPercentage(double pct); // abstraction behavior
}
```

## Step 4: Refined Abstraction

```java
class CircleShape extends Shape {
    private double x, y, radius;

    public CircleShape(double x, double y, double radius, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    public void draw() {
        drawingAPI.drawCircle(x, y, radius);
    }

    public void resizeByPercentage(double pct) {
        radius *= (1 + pct / 100.0);
    }
}
```

## Step 5: Client

```java
public class BridgePatternDemo {
    public static void main(String[] args) {
        Shape circle1 = new CircleShape(5, 10, 2, new DrawingAPI1());
        Shape circle2 = new CircleShape(15, 20, 5, new DrawingAPI2());

        circle1.draw();
        circle2.draw();

        circle1.resizeByPercentage(50);
        circle1.draw();
    }
}
```

## Output

```
API1.circle at (5.0, 10.0) with radius 2.0
API2.circle at (15.0, 20.0) with radius 5.0
API1.circle at (5.0, 10.0) with radius 3.0
```

# Benefits

- **Decouples interface from implementation**: Changes in one don't affect the other.
- **Increased flexibility**: You can vary abstraction and implementation independently.
- **Avoids class explosion**: Better alternative to subclassing when there are multiple dimensions of variation.

# Drawbacks

- **Increased complexity**: Adds layers and indirection.
- **Sometimes over-engineering**: Not needed if the abstraction-implementation split isn't required.

# When to Use

- When you want to avoid a permanent binding between an abstraction and its implementation.
- When both abstractions and implementations are likely to change independently.
- When you have a cross-platform or multi-format application (e.g., printers, renderers, network protocols).

# Related Patterns

- **Adapter**: Makes two incompatible interfaces work together, but Bridge is used to design classes for extensibility.
- **Strategy**: Encapsulates behavior; Bridge encapsulates implementation.
- **Abstract Factory**: Can be used with Bridge to create related implementations.