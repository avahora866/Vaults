---
tags:
  - Java
---
In Java, an **anonymous class** is a type of inner class that does not have a name and is instantiated in a single expression. It is useful when you need to override methods of a class or implement an interface on the fly without explicitly defining a new class.

# Syntax

An anonymous class is created using the `new` keyword, followed by the name of the class or interface being extended/implemented. It is typically used in two scenarios:

1. **Extending a class**
2. **Implementing an interface**

# Example: Extending a Class

```java
abstract class Animal {
    abstract void makeSound();
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Animal() {
            @Override
            void makeSound() {
                System.out.println("Roar!");
            }
        };
        myAnimal.makeSound(); // Output: Roar!
    }
}
```

Here, an anonymous class extends `Animal` and overrides the `makeSound` method.

# Example: Implementing an Interface

```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting greeting = new Greeting() {
            @Override
            public void sayHello() {
                System.out.println("Hello, World!");
            }
        };
        greeting.sayHello(); // Output: Hello, World!
    }
}
```

Here, an anonymous class implements the `Greeting` interface and provides an implementation for the `sayHello` method.

# Key Features of Anonymous Classes

- They **cannot have a constructor** since they don’t have a name.
- They are **created and used in a single expression**.
- They are useful for **short-lived** implementations, especially for event handling.
- They **can access local variables** (but only if those variables are effectively `final`).