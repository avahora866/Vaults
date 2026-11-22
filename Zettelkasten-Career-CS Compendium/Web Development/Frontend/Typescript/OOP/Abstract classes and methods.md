---
tags:
  - FORMAT
  - Typescript
---
# Abstract Classes and Methods in TypeScript

In TypeScript, **abstract classes** and **abstract methods** are used to create base classes that cannot be instantiated directly. They are meant to be extended by other classes, which implement the abstract methods.

An **abstract class** is a class that cannot be instantiated on its own but serves as a base for other classes. It allows you to define common functionality that subclasses can inherit while also enforcing that certain methods must be implemented by the subclasses.

# Key Concepts:

* **Abstract Class**: A class that is declared with the `abstract` keyword. It can contain both fully implemented methods (normal methods) and **abstract methods** (methods without implementation).

* **Abstract Method**: A method that is declared in an abstract class, but does not have any implementation. It must be implemented by any non-abstract class that extends the abstract class.

# Syntax:

```
abstract class AbstractClass {
  abstract methodName(): void;  // Abstract method

  // Concrete method
  concreteMethod() {
    console.log("This is a concrete method.");
  }
}
```

# Example of Abstract Class and Abstract Method:

```
abstract class Animal {
  abstract sound(): void;  // Abstract method

  move(): void {
    console.log("This animal moves.");
  }
}

class Dog extends Animal {
  sound(): void {
    console.log("Woof!");
  }
}

class Cat extends Animal {
  sound(): void {
    console.log("Meow!");
  }
}

const dog = new Dog();
dog.sound();  // Output: Woof!
dog.move();   // Output: This animal moves.

const cat = new Cat();
cat.sound();  // Output: Meow!
cat.move();   // Output: This animal moves.
```

# Explanation:

* The `Animal` class is **abstract**. It contains both an abstract method (`sound()`) and a concrete method (`move()`).

* The abstract method `sound()` does not have an implementation in the `Animal` class. Instead, it forces any class that extends `Animal` to provide its own implementation of the `sound()` method.

* `Dog` and `Cat` are concrete classes that **extend** the `Animal` class. Both classes must provide an implementation for the abstract `sound()` method.

* `move()` is a concrete method, so it can be directly used by the instances of `Dog` and `Cat` without needing to be redefined.

# Abstract Classes Cannot Be Instantiated

An abstract class cannot be instantiated directly. If you try to create an instance of an abstract class, TypeScript will give you an error.

```
const animal = new Animal();  // Error: Cannot create an instance of an abstract class.
```

In the above example, trying to create an instance of `Animal` will result in a compilation error because `Animal` is abstract and cannot be instantiated directly.

# Abstract Methods Must Be Implemented in Subclasses

Any class that extends an abstract class must implement all of its abstract methods. If the subclass does not implement the abstract methods, TypeScript will show an error.

```
abstract class Shape {
  abstract area(): number;  // Abstract method

  display(): void {
    console.log("Displaying shape.");
  }
}

class Circle extends Shape {
  radius: number;

  constructor(radius: number) {
    super();  // Calling the constructor of the abstract class
    this.radius = radius;
  }

  // Implementing the abstract method
  area(): number {
    return Math.PI * this.radius * this.radius;
  }
}

const circle = new Circle(5);
console.log(circle.area());  // Output: 78.53981633974483
circle.display();            // Output: Displaying shape.
```

# Explanation:

* The `Shape` class is abstract and defines an abstract method `area()`.

* The `Circle` class extends `Shape` and implements the `area()` method.

* The `Circle` class also has its own property `radius` and its own constructor.

* If we did not implement the `area()` method in the `Circle` class, TypeScript would give an error: **"Class 'Circle' incorrectly implements abstract class 'Shape'"**.

# Concrete Methods in Abstract Classes

While abstract methods don't have implementations, abstract classes can still contain **concrete methods** (methods with implementations). These methods can be directly inherited by subclasses, so you don't need to implement them again.

```
abstract class Vehicle {
  abstract drive(): void;  // Abstract method

  honk(): void {
    console.log("Honk! Honk!");
  }
}

class Car extends Vehicle {
  drive(): void {
    console.log("Driving the car.");
  }
}

const car = new Car();
car.drive();  // Output: Driving the car.
car.honk();   // Output: Honk! Honk!
```

In this case, the `Vehicle` class defines an abstract `drive()` method that must be implemented in any subclass. The `honk()` method is a concrete method that can be inherited and used by `Car` without needing to be redefined.

# Abstract Classes and Constructors

Abstract classes can also have constructors, but the constructor cannot be used directly. Instead, subclasses must call the constructor of the abstract class using the `super()` keyword.

```
abstract class Employee {
  constructor(public name: string, public role: string) {}

  abstract work(): void;  // Abstract method

  showInfo(): void {
    console.log(`${this.name} is a ${this.role}.`);
  }
}

class Developer extends Employee {
  constructor(name: string, role: string) {
    super(name, role);  // Call the constructor of the abstract class
  }

  work(): void {
    console.log(`${this.name} is writing code.`);
  }
}

const dev = new Developer("John", "Developer");
dev.showInfo();  // Output: John is a Developer.
dev.work();      // Output: John is writing code.
```

In this example:

* The `Employee` class has a constructor to initialize `name` and `role`. It also has an abstract `work()` method.

* The `Developer` class extends `Employee` and provides its own implementation for the `work()` method.

* The `Developer` class calls the constructor of `Employee` using `super()` to initialize the properties `name` and `role`.

# Use Cases for Abstract Classes

Abstract classes are useful in scenarios where:

* You want to define a common base class with some shared functionality (concrete methods) and force subclasses to implement specific functionality (abstract methods).

* You want to establish a contract (abstract methods) that all subclasses must follow while allowing them to implement the specifics.

* You want to prevent direct instantiation of a class but still need shared functionality to be inherited.

# Summary:

1. **Abstract Class**: A class that cannot be instantiated directly and is meant to be extended by other classes. It can contain both abstract methods (without implementation) and concrete methods (with implementation).

2. **Abstract Method**: A method defined in an abstract class that has no implementation and must be implemented by any subclass.

3. **Instantiation**: Abstract classes cannot be instantiated directly; they must be extended by concrete classes.

4. **Constructors**: Abstract classes can have constructors, and these constructors are called by subclasses using `super()`.

Abstract classes and methods allow you to define common functionality while enforcing certain methods to be implemented in subclasses, making your code more structured and maintainable.

