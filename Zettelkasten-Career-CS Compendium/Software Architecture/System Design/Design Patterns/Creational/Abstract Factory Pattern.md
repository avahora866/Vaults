---
tags:
  - DesignPattern
---
The Abstract Factory pattern provides an interface for creating **families of related or dependent objects** without specifying their concrete classes.

---

# When to Use It
Use Abstract Factory when:
- You need to create objects that are related or dependent.
- You want to enforce that only compatible objects are used together.
- Your code needs to be decoupled from the concrete classes of the products.

---

# Key Components

| Component           | Role                                                                 |
|---------------------|----------------------------------------------------------------------|
| **AbstractFactory** | Declares creation methods for each product in the family             |
| **ConcreteFactory** | Implements the creation methods to produce specific product variants |
| **AbstractProduct** | Declares interfaces for a set of products                            |
| **ConcreteProduct** | Implements the product interfaces                                    |
| **Client**          | Uses only interfaces declared by AbstractFactory and AbstractProduct |

---

# UML-Like Structure (Text Representation)

```
Client --> AbstractFactory
         |              |
         v              v
      ConcreteFactory1  ConcreteFactory2

Each factory creates:
- ProductA1 or ProductA2
- ProductB1 or ProductB2
```

---

# Java Example

Let’s build a UI toolkit that works on Windows and macOS.

## Interfaces

```java
// Abstract Products
interface Button {
    void paint();
}

interface Checkbox {
    void paint();
}

// Abstract Factory
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}
```

## Concrete Products

```java
class WinButton implements Button {
    public void paint() {
        System.out.println("Rendering Windows button");
    }
}

class MacButton implements Button {
    public void paint() {
        System.out.println("Rendering macOS button");
    }
}

class WinCheckbox implements Checkbox {
    public void paint() {
        System.out.println("Rendering Windows checkbox");
    }
}

class MacCheckbox implements Checkbox {
    public void paint() {
        System.out.println("Rendering macOS checkbox");
    }
}
```

## Concrete Factories

```java
class WinFactory implements GUIFactory {
    public Button createButton() {
        return new WinButton();
    }
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}

class MacFactory implements GUIFactory {
    public Button createButton() {
        return new MacButton();
    }
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```

## Client Code

```java
class Application {
    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void paint() {
        button.paint();
        checkbox.paint();
    }
}
```

## Usage

```java
public class Main {
    public static void main(String[] args) {
        GUIFactory factory;
        String os = System.getProperty("os.name").toLowerCase();

        if (os.contains("mac")) {
            factory = new MacFactory();
        } else {
            factory = new WinFactory();
        }

        Application app = new Application(factory);
        app.paint();
    }
}
```

---

# Advantages
- Enforces product compatibility
- Makes adding new product families easy
- Decouples client from concrete classes

---

# Disadvantages
- Hard to add new products (interfaces must change)
- More complex than simple factory if not needed

---

# Comparison

| Pattern            | Purpose                                  |
|--------------------|-------------------------------------------|
| Factory Method     | Delegate creation to subclasses           |
| Abstract Factory   | Provide families of related objects       |
| Builder            | Step-by-step construction of complex obj  |
| Prototype          | Clone existing objects                    |
