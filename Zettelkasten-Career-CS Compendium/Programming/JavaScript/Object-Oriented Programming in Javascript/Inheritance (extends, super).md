---
tags:
  - FORMAT
  - JavaScript
---
# Inheritance (extends, super)

Inheritance is one of the fundamental concepts in object-oriented programming (OOP). In JavaScript, inheritance allows one class (called a subclass or child class) to **inherit properties and methods** from another class (called a superclass or parent class). This concept is implemented using the **`extends`** keyword and the **`super()`** method.

---

## The `extends` Keyword

The **`extends`** keyword is used to create a **subclass** that inherits from a **superclass**. When a class extends another class, it automatically has access to the properties and methods of the parent class, unless they are overridden in the subclass.

### Syntax:

```javascript
class SubClass extends SuperClass {
  // Subclass specific code
}
```

*   
  **`SubClass`**: The subclass that inherits from the superclass.

* **`SuperClass`**: The parent class from which the subclass inherits.

### Example: Basic Inheritance

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call the parent class constructor
    this.breed = breed; // Additional property specific to Dog
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog('Buddy', 'Golden Retriever');
myDog.speak();  // Outputs: Buddy barks.
```

*   
  **`Dog`** inherits from **`Animal`** using `extends`.

* The **`Dog`** class inherits the `speak()` method from the `Animal` class, but it also **overrides** it with its own implementation.

* **`super(name)`** is used to call the parent class's constructor and initialize the `name` property.

---

## The `super()` Method

The **`super()`** method is used in two primary ways:

1. **Calling the constructor of the parent class**: This is necessary in the subclass constructor to initialize properties inherited from the parent class.

2. **Calling methods from the parent class**: You can use `super` to call methods defined in the parent class from within the subclass.

### Calling Parent Class Constructor with `super()`

When a subclass inherits from a parent class, it must call `super()` in its constructor if it wants to access the parent class’s constructor. The `super()` method initializes the inherited properties of the parent class.

### Example: Calling the Parent Constructor

```javascript
class Vehicle {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }

  displayInfo() {
    console.log(`This is a ${this.make} ${this.model}.`);
  }
}

class Car extends Vehicle {
  constructor(make, model, type) {
    super(make, model); // Call the parent class's constructor
    this.type = type;    // Additional property for Car
  }

  displayInfo() {
    super.displayInfo(); // Call the parent class's displayInfo method
    console.log(`It is a ${this.type} car.`);
  }
}

const myCar = new Car('Toyota', 'Camry', 'sedan');
myCar.displayInfo();
// Outputs:
// This is a Toyota Camry.
// It is a sedan car.
```

In this example:

* **`super(make, model)`** calls the constructor of the `Vehicle` class to initialize the `make` and `model` properties.

* The **`Car`** class calls the `displayInfo()` method from the `Vehicle` class using `super.displayInfo()`.

---

## Overriding Methods

When a subclass inherits from a parent class, it can **override** the parent class’s methods to provide its own specific implementation. The subclass can still call the parent class’s method by using **`super.methodName()`**.

### Example: Overriding Methods

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call the parent class constructor
    this.breed = breed;
  }

  speak() {
    super.speak(); // Call the parent class's speak method
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog('Buddy', 'Golden Retriever');
myDog.speak();
// Outputs:
// Buddy makes a sound.
// Buddy barks.
```

*   
  The **`Dog`** class overrides the `speak()` method, but it still calls the parent class's `speak()` method using **`super.speak()`** to avoid duplication and maintain its behavior.

* After that, it adds its own functionality, logging that the dog barks.

---

## Inheritance Chain

In JavaScript, the inheritance chain can go beyond a single level. A class can inherit from another class, and that class can inherit from another class, forming a **chain of inheritance**.

### Example: Multi-level Inheritance

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Mammal extends Animal {
  constructor(name, isWarmBlooded) {
    super(name); // Call Animal constructor
    this.isWarmBlooded = isWarmBlooded;
  }

  speak() {
    super.speak(); // Call Animal's speak method
    console.log(`${this.name} is a mammal.`);
  }
}

class Dog extends Mammal {
  constructor(name, breed) {
    super(name, true); // Call Mammal constructor
    this.breed = breed;
  }

  speak() {
    super.speak(); // Call Mammal's speak method
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog('Buddy', 'Golden Retriever');
myDog.speak();
// Outputs:
// Buddy makes a sound.
// Buddy is a mammal.
// Buddy barks.
```

*   
  **`Dog`** inherits from **`Mammal`**, which in turn inherits from **`Animal`**.

* Each class calls `super()` to initialize the inherited properties and can override methods as needed.

---

## `instanceof` and Inheritance

The **`instanceof`** operator can be used to check if an object is an instance of a specific class or any of its parent classes in the inheritance chain.

### Example: Using `instanceof`

```javascript
class Animal {}
class Dog extends Animal {}

const dog = new Dog();

console.log(dog instanceof Dog);  // true
console.log(dog instanceof Animal);  // true
console.log(dog instanceof Object);  // true
```

*   
  **`dog instanceof Dog`** returns `true` because `dog` is an instance of the `Dog` class.

* **`dog instanceof Animal`** returns `true` because `Dog` inherits from `Animal`.

* **`dog instanceof Object`** returns `true` because all classes ultimately inherit from `Object`.

---

## Key Takeaways

* **Inheritance** in JavaScript allows a class to inherit properties and methods from another class, promoting code reuse and structure.

* **`extends`** is used to create a subclass that inherits from a superclass.

* **`super()`** is used to call the constructor or methods of the parent class from the subclass.

* **Method Overriding** allows a subclass to provide its own implementation of a method that is already defined in the parent class.

* JavaScript supports **multi-level inheritance** and the **`instanceof`** operator allows you to check if an object is an instance of a class in the inheritance chain.

