---
tags:
  - DesignPattern
---
The **Prototype pattern** is a **creational design pattern** used to **create new objects by copying existing ones (prototypes)**, rather than creating them from scratch. This is especially useful when object creation is expensive or complex.

---

# When to Use It
Use the Prototype pattern when:
- Object creation is **costly** (e.g., involves I/O, network, DB access).
- Objects are **similar** and differ only slightly.
- You want to **avoid subclassing factories** for minor variations.
- You need to add or remove objects at runtime (e.g. in a registry).

---

# Structure

## Participants

| Role             | Description                                                  |
|------------------|--------------------------------------------------------------|
| `Prototype`      | Declares a `clone()` method                                  |
| `ConcretePrototype` | Implements the `clone()` method to copy itself             |
| `Client`         | Calls `clone()` to get a new object without depending on its class |

---

# Diagram (Text Representation)

```
Client
  |
  v
Prototype <-- ConcretePrototype
     |
  clone()
```

---

# Java Example

## a) Prototype Interface

```java
interface Shape extends Cloneable {
    Shape clone();
    void draw();
}
```

## b) Concrete Prototype

```java
class Circle implements Shape {
    private int radius;
    private String color;

    public Circle(int radius, String color) {
        this.radius = radius;
        this.color = color;
    }

    public void draw() {
        System.out.println("Drawing circle of radius " + radius + " and color " + color);
    }

    public Shape clone() {
        return new Circle(this.radius, this.color); // manual deep copy
    }
}
```

## c) Client Usage

```java
public class Main {
    public static void main(String[] args) {
        Circle original = new Circle(10, "blue");
        Circle copy = (Circle) original.clone();

        original.draw();
        copy.draw();
    }
}
```

---

# Java Notes
- `Cloneable` is a **marker interface** in Java.
- Java’s built-in `.clone()` is **shallow** by default; override if you need **deep copies**.
- Many developers avoid Java's native `clone()` due to its complexity and prefer custom copy constructors or factory methods.

---

# Deep vs. Shallow Cloning

| Type          | Description                                          |
|---------------|------------------------------------------------------|
| **Shallow**   | Copies references to nested objects (shared objects) |
| **Deep**      | Recursively clones all nested objects                |

**Example:**
If `Circle` had a `Position` object as a field:
- Shallow copy: original and copy share the same `Position`
- Deep copy: `Position` is also cloned

---

# Advantages

- Avoids costly object creation
- Can clone objects without knowing their concrete class
- Easy to add new object types at runtime

---

# Disadvantages

- Cloning complex objects correctly (deep copy) can be error-prone
- Java’s native cloning is broken in some cases (e.g., final fields, `CloneNotSupportedException`)
- Can lead to bugs if shared mutable state is unintentionally copied

---

# Comparison with Other Patterns

| Pattern         | Use Case                                           |
|-----------------|----------------------------------------------------|
| Factory Method  | Create new instances using a method                |
| Abstract Factory| Create families of objects                         |
| Builder         | Construct complex objects step-by-step             |
| **Prototype**   | Clone existing instances instead of new creation   |

---

# Alternative: Manual Cloning

Instead of relying on `clone()`, use copy constructors:

```java
class Circle {
    private int radius;
    private String color;

    public Circle(Circle source) {
        this.radius = source.radius;
        this.color = source.color;
    }
}
```

This avoids the problems with Java’s built-in cloning.