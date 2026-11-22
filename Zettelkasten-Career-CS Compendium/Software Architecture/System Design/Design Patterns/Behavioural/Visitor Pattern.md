---
tags:
  - DesignPattern
---
The **Visitor** pattern is a **behavioral** design pattern that lets you separate algorithms from the objects on which they operate. It allows you to define new operations on object structures without changing the classes of the elements on which it operates.

# Intent

- Separate algorithms from the objects they operate on.
- Add new behavior to a class hierarchy **without modifying** the classes themselves.
- Follow the **Open/Closed Principle**: open for extension, closed for modification.

# Motivation

Imagine you have a complex object structure (e.g., an AST, a composite document, or a file system) and you need to perform operations like printing, exporting, or analyzing. If you add these behaviors directly to the objects, it violates **Single Responsibility Principle** and makes maintenance difficult.

The Visitor pattern solves this by **externalizing operations** into a separate visitor class and letting the elements “accept” a visitor that performs the operation.

# Structure

1. **Visitor**: Declares a `visit()` method for each concrete element.
2. **ConcreteVisitor**: Implements the operations for each type of element.
3. **Element**: Declares an `accept()` method that takes a visitor.
4. **ConcreteElement**: Implements `accept()` to call `visitor.visit(this)`.

```plaintext
+----------------+       +------------------+
|    Visitor     |<------+  ConcreteVisitor |
+----------------+       +------------------+
       |                          |
+----------------+      +---------------------+
|    Element     |<-----|   ConcreteElement   |
+----------------+      +---------------------+
       |
  +----------+
  | accept() |----> double-dispatch
```

# Double Dispatch

Visitor uses **double dispatch** to determine the operation based on both:

- The type of the element
- The type of the visitor

This allows operations to vary based on both types, which single dispatch (like regular method calls) can’t do alone.

# Java Example

Let’s model a shopping cart with different items (e.g., books and fruits). The visitor will calculate total cost.

## Step 1: Define the Visitor interface

```java
interface ShoppingCartVisitor {
    int visit(Book book);
    int visit(Fruit fruit);
}
```

## Step 2: Define the Element interface

```java
interface ItemElement {
    int accept(ShoppingCartVisitor visitor);
}
```

## Step 3: Create Concrete Elements

```java
class Book implements ItemElement {
    private int price;
    private String isbn;

    public Book(int price, String isbn) {
        this.price = price;
        this.isbn = isbn;
    }

    public int getPrice() {
        return price;
    }

    public String getIsbn() {
        return isbn;
    }

    public int accept(ShoppingCartVisitor visitor) {
        return visitor.visit(this);
    }
}
```

```java
class Fruit implements ItemElement {
    private int pricePerKg;
    private int weight;
    private String name;

    public Fruit(int pricePerKg, int weight, String name) {
        this.pricePerKg = pricePerKg;
        this.weight = weight;
        this.name = name;
    }

    public int getPricePerKg() {
        return pricePerKg;
    }

    public int getWeight() {
        return weight;
    }

    public String getName() {
        return name;
    }

    public int accept(ShoppingCartVisitor visitor) {
        return visitor.visit(this);
    }
}
```

## Step 4: Create the Concrete Visitor

```java
class ShoppingCartVisitorImpl implements ShoppingCartVisitor {
    public int visit(Book book) {
        int cost = book.getPrice();
        System.out.println("Book ISBN: " + book.getIsbn() + " cost = " + cost);
        return cost;
    }

    public int visit(Fruit fruit) {
        int cost = fruit.getPricePerKg() * fruit.getWeight();
        System.out.println(fruit.getName() + " cost = " + cost);
        return cost;
    }
}
```

## Step 5: Client Code

```java
public class VisitorPatternDemo {
    public static void main(String[] args) {
        ItemElement[] items = new ItemElement[] {
            new Book(50, "1234"),
            new Book(100, "5678"),
            new Fruit(10, 2, "Banana"),
            new Fruit(5, 5, "Apple")
        };

        int total = 0;
        ShoppingCartVisitor visitor = new ShoppingCartVisitorImpl();
        for (ItemElement item : items) {
            total += item.accept(visitor);
        }

        System.out.println("Total Cost = " + total);
    }
}
```

## Output:

```
Book ISBN: 1234 cost = 50
Book ISBN: 5678 cost = 100
Banana cost = 20
Apple cost = 25
Total Cost = 195
```

# Benefits

- **Open/Closed Principle**: Add new operations without modifying existing element classes.
- **Separation of concerns**: Business logic is separated from object structure.
- **Easier to define related operations**: Related behaviors for different classes are centralized in one place.

# Drawbacks

- **Adding new elements is hard**: Every visitor must implement visit methods for each new element, so it's better suited when the element structure is stable.
- **Tight coupling**: Elements must know about the visitor interface.
- **Verbose**: Can be boilerplate-heavy in statically typed languages like Java.

# Common Use Cases

- Compilers and interpreters: Traversing abstract syntax trees.
- File systems: Applying operations like compression or virus scanning.
- UI frameworks: Applying themes or rendering logic.
- Object structure analytics: Metrics, reporting, or logging.

# Related Patterns

- **Composite**: Often used together. Visitor traverses a composite structure.
- **Interpreter**: Visitor can evaluate or compile expressions.
- **Double Dispatch**: Visitor leverages double dispatch to achieve flexible behavior selection.