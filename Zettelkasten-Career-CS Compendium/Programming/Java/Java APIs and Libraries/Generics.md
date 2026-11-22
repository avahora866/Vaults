---
tags:
  - Java
---
> [!NOTE] Note:
> Java generics allow you to define classes, interfaces, and methods that can operate on various types, while providing compile-time type safety.

Generics were introduced in Java 5 to enhance the type system and provide more flexibility and reusability in code.

Here are some key concepts and syntax related to Java generics:

- Type Parameters: Generics use type parameters, which are placeholders for specific types. Type parameters are represented by angle brackets ("<" and ">") and can be named anything you want. By convention, single uppercase letters are commonly used, such as T, E, or K.
- Generic Classes: You can create generic classes by specifying type parameters when defining the class. For example:

```java
public class Box<T> {    private T item;
    public void setItem(T item) {        this.item = item;    }
    public T getItem() {        return item;    }}
```

In the above example, T is a type parameter for the Box class. It can represent any type, and it will be determined when an instance of the Box class is created.

- Using Generic Types: When creating an instance of a generic class, you specify the actual type you want to use in place of the type parameter. For example:

```java
Box<String> stringBox = new Box<>();stringBox.setItem("Hello");String item = stringBox.getItem();
```

In this case, Box`<String>` creates a Box instance that works with String objects.

- Generic Methods: You can also have generic methods within non-generic classes. These methods have their own type parameters separate from the class's type parameters. For example:

```java
public class Utility {    public static <T> void printArray(T[] array) {        for (T element : array) {            System.out.println(element);        }    }}
```

The method printArray accepts an array of type T and prints its elements.

- Bounded Type Parameters: You can constrain type parameters to be a specific type or its subtypes by using bounded type parameters. For example, class MyClass<T extends Number> specifies that the type parameter T must be a subclass of Number.
- Wildcards: Wildcards (?) allow you to work with unknown types in generic classes or methods. There are two types of wildcards: ? extends Type (upper bounded wildcard) and ? super Type (lower bounded wildcard). They provide flexibility when dealing with different types.

These are just some of the basic concepts of Java generics. They enable you to create reusable code that can work with different types, ensuring type safety at compile-time and reducing the need for explicit type casting.