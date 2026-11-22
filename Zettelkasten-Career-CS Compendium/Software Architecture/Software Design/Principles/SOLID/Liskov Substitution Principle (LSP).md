---
tags:
  - FORMAT
  - SoftwareDesign
  - "#SECONDARY_BRANCH"
---
**Definition**: Subtypes must be substitutable for their base types without altering the correctness of the program.

In other words, **any object of a subclass should be able to stand in for its superclass without breaking the program's behaviour**.

This principle was introduced by **Barbara Liskov** in 1987, and it’s essential for **correct inheritance** and **robust polymorphism** in object-oriented programming.

# What It Really Means

Imagine you have a class `Bird` and a subclass `Penguin`. If `Bird` has a method `fly()`, and `Penguin` inherits from `Bird` but can't fly, substituting a `Penguin` where a `Bird` is expected breaks expectations. That violates LSP.

So LSP is really about **behavioural compatibility**, not just having the same method names or structure. It’s not enough to just override methods—you must **preserve the contract** (the expectations) set by the base class.

# Formal Rule (Liskov’s Original Definition)

If `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` **without altering any of the desirable properties of the program** (correctness, task performance, etc.).

# Key Properties to Maintain in Subclasses

1. **Preconditions must not be strengthened** (you can’t require more than the base class does).
2. **Postconditions must not be weakened** (you must meet the same guarantees as the base class).
3. **Invariants must be preserved** (you shouldn't change rules the base class relies on).
4. **Exception types should be compatible** (throwing new, unexpected exceptions breaks substitution).
# LSP Violation Example in Java

Let’s look at an example involving shapes:

```java
public class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}
```

Now a subclass:

```java
public class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void setHeight(int height) {
        this.height = height;
        this.width = height;
    }
}
```

This seems logical—after all, a square is a special case of a rectangle. But now consider this function:

```java
public void resize(Rectangle r) {
    r.setWidth(5);
    r.setHeight(10);
    assert r.getArea() == 50;
}
```

Pass in a `Square` and the assertion fails because both width and height end up as 10, resulting in an area of 100. That’s a violation of LSP: `Square` cannot be substituted where a `Rectangle` is expected **without changing behavior**.

# How to Adhere to LSP

1. **Favour composition over inheritance** when behaviour differs.
2. **Don’t override methods in a way that changes expected results.**
3. **Ensure subclass honour the "contract"** (same inputs lead to same kinds of outputs).
4. **Use interfaces to separate roles**—sometimes the base class abstraction is too broad.
## Refactor Example

Instead of forcing a square to be a subclass of rectangle, extract a `Shape` interface:

```java
public interface Shape {
    int getArea();
}
```

Then implement `Rectangle` and `Square` separately, each maintaining its own consistent behaviour.

# Why LSP Matters

- **Enables safe polymorphism**: You can rely on base types without knowing the exact subclass.
- **Prevents unexpected bugs**: If LSP is broken, substituting a subclass can introduce subtle failures.
- **Improves maintainability**: Systems are more predictable and easier to reason about.
- **Supports open/closed principle**: LSP lets you extend systems without breaking the parts that rely on the original abstraction.

# Real-World Analogy

If you’re driving a car and expect it to have a steering wheel and brake, but you’re suddenly handed a car that only turns using voice control and stops via a parachute—then that new car breaks the contract you expected. It may technically be a "car", but you can’t substitute it without adjusting how you use it.

---

In essence, **LSP is a principle about trust**: when you write code to work with a parent class or interface, you should be able to **trust that all subclass will behave consistently** with the expectations set by the original abstraction.