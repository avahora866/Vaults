---
tags:
  - Paradigms
  - "#TRUNK"
---
Functional Programming (FP) is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. The core principles include:

 - Immutability: Data is immutable (i.e., once created, it cannot be changed). This prevents side effects and makes programs more predictable.  
 - First-Class Functions: Functions are first-class citizens, meaning they can be assigned to variables, passed as arguments, and returned from other functions.  
 - Higher-Order Functions: Functions that take one or more functions as parameters, or return a function as a result. This allows functions to be used in a flexible and composable way.  
 - Pure Functions: A function is pure if it produces the same output for the same input and has no side effects (does not modify global state or variables outside the function).  
 - Function Composition: Combining simple functions to create more complex ones. This is often done using higher-order functions.

# Functional Programming Features in Java

Java introduced several features with Java 8 to better support functional programming:

## Lambda Expressions:  
Lambdas enable you to write functions in a concise, inline manner, which can be passed around as arguments.  
Syntax: `(parameters) -> expression`

Example:  
 
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");  
names.forEach(name -> System.out.println(name));
```

   
## Streams:  
The Stream API allows you to process sequences of elements in a functional style. It provides methods like map(), filter(), reduce(), and collect().  
Stream pipeline: A series of operations on data that can be chained together.

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");  
long count = names.stream().filter(name -> name.length() > 3).count();  
System.out.println(count);  // Output: 2 (Alice, Charlie)
```

   
## Method References:  
Method references are a shorthand way of referring to a method of an object or class.  
Syntax: `ClassName::methodName`
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");  
names.forEach(System.out::println);
```

   
## Optional:  
Optional is a container object which may or may not contain a value, helping to avoid NullPointerException and promoting a functional approach to handling null values.
  
```java
Optional<String> name = Optional.ofNullable("Alice");  
name.ifPresent(n -> System.out.println(n));  // Prints "Alice" if present
```

 

## Pure Functions
A pure function is one that satisfies two conditions:

Given the same input, it always produces the same output (i.e., no reliance on external state or variables).  
It does not cause any side effects, such as modifying external variables, altering the state of objects, or performing I/O operations.

## Importance of Pure Functions:

 - Predictability: Since pure functions always return the same result for the same input, they are easy to test and reason about.  
 - Composability: Pure functions can be easily composed together to form more complex operations.  
 - Concurrency: Pure functions can be safely used in parallel execution without worrying about race conditions or shared mutable state.

## Example:

```java
public int add(int a, int b) {  
    return a + b;  // Pure function: output depends only on inputs, no side effects  
}
```

## Immutability
Immutability is the concept of ensuring that once an object is created, its state cannot be changed. This is a key concept in functional programming as it avoids side effects and ensures thread safety.

## Benefits of Immutability:
 - Thread Safety: Immutable objects can be shared safely across multiple threads without synchronization.  
 - Simplified Reasoning: When data is immutable, you don’t have to worry about its state changing unexpectedly, making code easier to understand and debug.  
 - Easier to Test: Since immutable objects don't change, they can be safely shared between functions without the risk of unintended modifications.

## Example:
  
```java
public class Person {  
    private final String name;  // Final field, cannot be changed after initialization  
    private final int age;  
      
    public Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
      
    public String getName() {  
        return name;  
    }  
      
    public int getAge() {  
        return age;  
    }  
}
```

## Higher-Order Functions

A higher-order function is a function that either:

Takes one or more functions as arguments, or  
Returns a function as a result.

## Example:
 
```java
// A higher-order function that takes a function as an argument 
public static List<Integer> applyFunctionToList(List<Integer> list, Function<Integer, Integer> func) {  
    return list.stream().map(func).collect(Collectors.toList());  
}

public static void main(String[] args) {  
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4);  
    List<Integer> squares = applyFunctionToList(numbers, x -> x  x);  // Squares each number  
    squares.forEach(System.out::println);  // Prints 1, 4, 9, 16  
}
```

## Use Cases of Functional Programming in Java

 - Data Transformation: Using streams and lambda expressions, functional programming is excellent for transforming and filtering collections.  
	 - Example: Filtering and mapping a list of objects.  
 - Concurrency: Immutability and pure functions make functional programming an ideal approach for building thread-safe concurrent applications.  
 - Declarative Programming: Functional programming encourages a more declarative style of programming (i.e., focusing on what needs to be done, not how it should be done), which can lead to cleaner, more maintainable code.

## Limitations of Functional Programming in Java

While Java supports functional programming, it still has some limitations:

 - Verbosity: Java’s syntax, even with lambdas and streams, can still be more verbose compared to languages like Scala or Haskell that were designed with functional programming in mind.  
 - Performance: The use of streams and lambdas may introduce some overhead in terms of performance, especially when working with large data sets or complex operations.  
 - Learning Curve: Java developers who are not familiar with functional programming concepts may find it challenging to adopt functional techniques effectively.  
 - Limited Support for Immutable Data Structures: Although Java supports immutability with final variables, it lacks built-in immutable collections, making it more difficult to work with immutable data structures compared to languages that provide this out-of-the-box.

---

## Summary of Key Points:

| Feature | Functional Programming in Java |
| ----- | ----- |
| Core Concepts | Immutability, first-class functions, higher-order functions, pure functions |
| Java Features | Lambda expressions, Streams API, method references, Optional |
| Pure Functions | Functions that have no side effects and always return the same output for the same input |
| Immutability | Objects cannot be modified once created, ensuring thread safety and reducing bugs |
| Higher-Order Functions | Functions that take other functions as arguments or return functions |
| Use Cases | Data transformation, concurrency, declarative programming |
| Limitations | Verbosity, performance overhead, learning curve, limited immutable collections support |


