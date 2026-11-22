---
tags:
  - FORMAT
  - Typescript
---
# Creating Classes and Constructors in TypeScript

In TypeScript, a **class** is a blueprint for creating objects, providing methods and properties. A **constructor** is a special function inside a class that is automatically called when a new object is created.

# Syntax for Classes:

```
class ClassName {
  constructor() {
    // Initialization code
  }
}
```

# Example of Creating a Class and Constructor:

```
class Person {
  name: string;
  age: number;

  // Constructor to initialize the object
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  // Method
  greet(): void {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const person1 = new Person("Alice", 30);
person1.greet();  // Output: Hello, my name is Alice and I am 30 years old.
```

In this example:

* `Person` is a class with properties `name` and `age`.

* The constructor takes parameters `name` and `age` to initialize these properties when a new instance of `Person` is created.

* The `greet` method is used to print a greeting message.

# Public, Private, and Protected Access Modifiers

Access modifiers control the visibility of class members (properties and methods). There are three main access modifiers in TypeScript:

* **`public`**: This is the default modifier. It means the property or method can be accessed from anywhere.

* **`private`**: This means the property or method can only be accessed within the class where it is defined.

* **`protected`**: This means the property or method can be accessed within the class and by subclasses (derived classes), but not outside of them.

## Example of Access Modifiers:

```
class Person {
  public name: string;
  private age: number;
  protected city: string;

  constructor(name: string, age: number, city: string) {
    this.name = name;
    this.age = age;
    this.city = city;
  }

  public greet(): void {
    console.log(`Hello, my name is ${this.name}, I am ${this.age} years old and live in ${this.city}.`);
  }

  private secretMethod(): void {
    console.log("This is a private method");
  }

  protected showCity(): void {
    console.log(`I live in ${this.city}`);
  }
}

const person = new Person("Alice", 30, "New York");
person.greet(); // OK, because greet is public

// person.age = 25;  // Error: 'age' is private and cannot be accessed outside the class
// person.secretMethod(); // Error: 'secretMethod' is private and cannot be accessed outside the class
// person.city = "Chicago"; // Error: 'city' is protected and cannot be accessed outside the class
```

In this example:

* `name` is public, so it can be accessed anywhere.

* `age` is private, so it can only be accessed within the `Person` class.

* `city` is protected, so it can be accessed within the class and subclasses (but not from outside).

# Readonly Properties

A **readonly** property in TypeScript can only be assigned a value during initialization (in the constructor or at the point of declaration). After that, its value cannot be modified.

## Example of Readonly Properties:

```
class Person {
  readonly name: string;
  readonly age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): void {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const person = new Person("Alice", 30);
person.greet();

// person.name = "Bob"; // Error: Cannot assign to 'name' because it is a read-only property.
```

In this example:

* The `name` and `age` properties are `readonly`, meaning once they are set in the constructor, they cannot be modified again.

# Getters and Setters

In TypeScript, you can use **getters** and **setters** to define methods that allow you to interact with an object’s properties in a controlled way.

* A **getter** is a method that allows you to access a property value.

* A **setter** is a method that allows you to modify a property value.

They are defined using the `get` and `set` keywords.

## Example of Getters and Setters:

```
class Person {
  private _age: number;

  constructor(private _name: string, age: number) {
    this._age = age;
  }

  // Getter for _name
  get name(): string {
    return this._name;
  }

  // Setter for _name
  set name(value: string) {
    if (value.length < 3) {
      console.log("Name is too short!");
    } else {
      this._name = value;
    }
  }

  // Getter for _age
  get age(): number {
    return this._age;
  }

  // Setter for _age
  set age(value: number) {
    if (value < 0) {
      console.log("Age cannot be negative!");
    } else {
      this._age = value;
    }
  }

  greet(): void {
    console.log(`Hello, my name is ${this._name} and I am ${this._age} years old.`);
  }
}

const person = new Person("Alice", 30);
person.greet();  // Output: Hello, my name is Alice and I am 30 years old.

person.name = "Bob";  // OK
person.age = 25;      // OK

person.name = "Al";  // Output: Name is too short!
person.age = -5;     // Output: Age cannot be negative!

person.greet();  // Output: Hello, my name is Bob and I am 25 years old.
```

In this example:

* The `name` and `age` properties are accessed and modified through the **getter** and **setter** methods.

* The **getter** `name()` simply returns the `_name` value.

* The **setter** `name()` checks if the value is at least 3 characters long before assigning it.

* The **getter** `age()` returns the `_age` value.

* The **setter** `age()` checks if the value is non-negative before assigning it.

## Using Getters and Setters

* **Getters** provide a way to retrieve the value of a property.

* **Setters** provide a way to change the value of a property, often with additional validation.

# Summary:

1. **Classes and Constructors**: Used to define the structure of objects and initialize them with specific values through constructors.

2. **Access Modifiers**:

   * `public`: Can be accessed from anywhere.

   * `private`: Can only be accessed within the class.

   * `protected`: Can be accessed within the class and by subclasses.

3. **Readonly Properties**: Ensure a property can only be set once, either during initialization or within the constructor.

4. **Getters and Setters**: Provide controlled access to an object's properties, often with validation or transformation logic.

These features allow TypeScript to provide robust and flexible object-oriented programming tools, making it easier to manage and enforce rules on how objects are created and modified.

