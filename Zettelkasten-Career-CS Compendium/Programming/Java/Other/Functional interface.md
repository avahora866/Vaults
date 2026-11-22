---
tags:
  - Java
---
A **functional interface** in Java is an interface that contains exactly **one [[Java/Object-Oriented-Programming/Abstraction|abstract]] method**. It can have multiple default or static methods, but only a single abstract method is allowed. Functional interfaces are primarily used in **lambda expressions** and **method references**, enabling a more concise and readable way to write code.

The main purpose is to easilly implement an abstract method that is inside an interface in one line of code. Instead of creating a concrete class to implement it.


# Examples of Functional Interfaces:

## Built-in Functional Interfaces:

Java provides several built-in functional interfaces in the `java.util.function` package, such as:

- `Runnable` → `void run()`
- `Callable<T>` → `T call()`
- `Consumer<T>` → `void accept(T t)`
- `Supplier<T>` → `T get()`
- `Function<T, R>` → `R apply(T t)`
- `Predicate<T>` → `boolean test(T t)`

## Custom Functional Interface:

You can create your own functional interface using the `@FunctionalInterface` annotation (optional but recommended):

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void execute(); // Single abstract method
}

public class Main {
    public static void main(String[] args) {
        // Using Lambda Expression
        MyFunctionalInterface func = () -> System.out.println("Hello, Functional Interface!");
        func.execute();
    }
}
```

# Key Features:

- Only **one** abstract method is allowed.
- Can have multiple **default** and **static** methods.
- The `@FunctionalInterface` annotation helps enforce the rule (optional but recommended).
- Can be used with **lambda expressions** and **method references**.