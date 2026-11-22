---
tags:
  - FORMAT
  - JavaScript
---
# Classes and Constructors (class, constructor)

In JavaScript, **classes** and **constructors** provide a way to define and create objects in a more structured and organized manner. They are syntactic sugar over JavaScript's prototype-based inheritance system and offer a cleaner and more readable approach to object-oriented programming (OOP).

---

## What is a Class?

A **class** is a blueprint for creating objects with specific properties and methods. Classes provide a more intuitive syntax for creating and managing objects and their behavior compared to traditional constructor functions.

### Basic Syntax of a Class:

```javascript
class ClassName {
  constructor() {
    // Constructor code
  }
  
  // Method definitions
  methodName() {
    // Method code
  }
}
```

*   
  **`class`**: Defines the class.

* **`constructor()`**: A special method for initializing new objects created from the class.

* **Methods**: Functions that are defined within the class, representing the behavior of objects created from that class.

---

## The `constructor` Method

The **`constructor()`** method is a special method used to create and initialize objects created from the class. It’s automatically invoked when you create a new instance of a class using the `new` keyword.

### Constructor Syntax:

```javascript
class Person {
  constructor(name, age) {
    this.name = name; // Assign the name parameter to the object property
    this.age = age;   // Assign the age parameter to the object property
  }
}
```

In this example:

* The `constructor` accepts parameters (`name` and `age`).

* The `this` keyword is used to set the values of the object's properties, which are available to all methods of the class.

### Example: Using the Constructor

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  introduce() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

// Creating an instance of the Person class
const person1 = new Person('John', 30);
person1.introduce();  // Outputs: Hello, my name is John and I am 30 years old.
```

In this example:

* **`new Person('John', 30)`**: The constructor is called, creating a new `Person` object with the name "John" and age 30\.

* The method `introduce()` is called on the object, which outputs a message with the object’s properties.

---

## Creating Instances of a Class

To create an instance (or object) of a class, you use the `new` keyword. This calls the class’s constructor method to initialize the object with the specified properties.

### Example: Creating Instances

```javascript
class Car {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
  }

  startEngine() {
    console.log(`The engine of the ${this.year} ${this.make} ${this.model} is now running.`);
  }
}

const myCar = new Car('Toyota', 'Corolla', 2021);
const yourCar = new Car('Honda', 'Civic', 2020);

myCar.startEngine(); // Outputs: The engine of the 2021 Toyota Corolla is now running.
yourCar.startEngine(); // Outputs: The engine of the 2020 Honda Civic is now running.
```

*   
  The `new Car('Toyota', 'Corolla', 2021)` creates a new object `myCar` with the properties `make`, `model`, and `year`.

* The `startEngine()` method is called on both `myCar` and `yourCar` instances.

---

## Class Inheritance

One of the main advantages of using classes is the ability to create subclasses using inheritance. A class can inherit properties and methods from another class using the **`extends`** keyword.

### Syntax for Inheritance:

```javascript
class ChildClass extends ParentClass {
  constructor() {
    super(); // Call the parent class's constructor
    // Additional initialization code
  }
}
```

*   
  **`extends`**: This keyword is used to create a subclass that inherits from a parent class.

* **`super()`**: This keyword is used to call the parent class's constructor method.

### Example: Inheritance in Classes

```javascript
class Animal {
  constructor(name, species) {
    this.name = name;
    this.species = species;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name, 'Dog');  // Call the parent class constructor
    this.breed = breed;  // Additional property specific to Dog class
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog('Buddy', 'Golden Retriever');
dog.speak();  // Outputs: Buddy barks.
```

In this example:

* The `Dog` class extends the `Animal` class and inherits its properties and methods.

* The `super(name, 'Dog')` calls the constructor of the `Animal` class to set the `name` and `species` properties.

* The `speak()` method is overridden in the `Dog` class to provide a different implementation.

---

## Static Methods

Static methods are methods that are called on the class itself, not on instances of the class. They are defined using the `static` keyword and are not inherited by instances.

### Example: Static Method

```javascript
class MathOperations {
  static add(a, b) {
    return a + b;
  }

  static subtract(a, b) {
    return a - b;
  }
}

console.log(MathOperations.add(5, 3));  // Outputs: 8
console.log(MathOperations.subtract(5, 3)); // Outputs: 2
```

In this example:

* **`MathOperations.add(5, 3)`**: The `add` method is a static method and is called on the class itself, not an instance of it.

---

## Getter and Setter Methods

In JavaScript, you can define getter and setter methods within a class. These methods are used to get and set the value of an object’s properties. Getters and setters are useful for controlling access to an object’s properties.

### Example: Getter and Setter Methods

```javascript
class Person {
  constructor(name, age) {
    this._name = name;
    this._age = age;
  }

  // Getter method for name
  get name() {
    return this._name;
  }

  // Setter method for name
  set name(newName) {
    this._name = newName;
  }

  // Getter method for age
  get age() {
    return this._age;
  }

  // Setter method for age
  set age(newAge) {
    if (newAge > 0) {
      this._age = newAge;
    } else {
      console.log('Age must be positive');
    }
  }
}

const person = new Person('John', 30);
console.log(person.name); // Outputs: John

person.name = 'Jane'; // Calls the setter
console.log(person.name); // Outputs: Jane

person.age = -5; // Outputs: Age must be positive
console.log(person.age); // Outputs: 30
```

*   
  **Getter**: Used to get the value of a property (`person.name`).

* **Setter**: Used to modify the value of a property (`person.name = 'Jane'`).

---

## Key Takeaways

* **Classes** in JavaScript provide a more readable and structured way to create objects and manage object-oriented code.

* The **constructor** method is called when a new instance of a class is created and is used to initialize the object’s properties.

* **Inheritance** allows classes to extend other classes, inheriting their methods and properties.

* **Static methods** belong to the class itself, not instances of the class, and can be called directly on the class.

* **Getters and setters** allow controlled access to an object's properties, adding encapsulation to your code.

