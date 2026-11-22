---
tags:
  - OOP
  - Java
---
In Java, polymorphism allows methods to perform different tasks based on the object they are acting upon. There are two types of polymorphism:

# Compile-Time Polymorphism

Also known as static polymorphism or method overloading, this occurs when multiple methods in the same class have the same name but different parameters (number, type, or order). The method to be called is determined at compile time.

- How it works: The compiler determines which method to call by checking the method signature.
- Example:

```java
class MathUtil {
    // Method with one parameter
    int add(int a) {
        return a + 10;
    }

    // Method with two parameters (overloaded)
    int add(int a, int b) {
        return a + b;
    }

    // Method with different parameter types (overloaded)
    double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        MathUtil util = new MathUtil();
        System.out.println(util.add(5));        // Calls add(int a)
        System.out.println(util.add(5, 10));    // Calls add(int a, int b)
        System.out.println(util.add(2.5, 3.5)); // Calls add(double a, double b)
    }
}
```

- Key points:
    - Resolved during compilation.
    - Achieved using method overloading.
    - No inheritance or runtime overhead is involved.

# Runtime Polymorphism

Also known as dynamic polymorphism or method overriding, this occurs when a subclass overrides a method of its superclass. The method to be called is determined at runtime based on the object type.

- How it works: The JVM determines which method implementation to execute based on the object instance, not the reference type.
- Example:

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal;

        myAnimal = new Dog();
        myAnimal.sound(); // Calls Dog's sound() -> Output: Dog barks

        myAnimal = new Cat();
        myAnimal.sound(); // Calls Cat's sound() -> Output: Cat meows
    }
}
```

- Key points:
    - Resolved during runtime.
    - Achieved using method overriding.
    - Requires inheritance (subclass and superclass relationship).
    - Uses dynamic method dispatch.
## Interface -Based
Interface-based polymorphism is a type of polymorphism where different classes implement a common interface, allowing objects of those classes to be treated uniformly. This means you can write code that works with an interface type, without needing to know the specific class implementing it. The actual method that gets executed is determined at runtime, depending on the object’s class.

This approach promotes loose coupling, as it relies on the contract defined by the interface rather than the concrete implementation. It allows for flexibility and scalability because new classes can be introduced without altering existing code, as long as they implement the interface. This is commonly used in dependency injection and [[Design Patterns|design patterns]] like [[Strategy Pattern|Strategy]] or [[Observer Pattern|Observer]], making code more modular and easier to maintain.

# Summary of Differences

|Aspect|Compile-Time Polymorphism|Runtime Polymorphism|
|---|---|---|
|Also Known As|Static Polymorphism / Method Overloading|Dynamic Polymorphism / Method Overriding|
|Method Resolution|At compile time|At runtime|
|Inheritance Needed|No|Yes (method overriding in subclasses)|
|Flexibility|Less flexible, decided at compile time|More flexible, decided at runtime|
|Performance|Faster, as it's resolved at compile time|Slightly slower due to runtime binding|
|Use Case|Same method name with different parameters|Same method name and signature, different implementation in subclasses|

# When to Use Each

- Compile-Time Polymorphism: When the behavior is determined by the number or type of arguments (e.g., utility classes with multiple variants of methods).
- Runtime Polymorphism: When you need to override a method in a subclass to provide a specific implementation (e.g., implementing different behaviors for different object types in an inheritance hierarchy).

Runtime polymorphism is heavily used in frameworks and libraries that rely on method overriding (e.g., Spring and Hibernate), allowing for flexible and dynamic behavior at runtime.