---
tags:
  - FORMAT
  - Typescript
---
# Implementing Interfaces in Classes in TypeScript

In TypeScript, **interfaces** define the structure or contract that a class must follow. Interfaces can specify the properties and methods that a class must implement, but they cannot contain any implementation details. When a class **implements** an interface, it must provide an implementation for all the properties and methods declared in that interface.

# Key Concepts:

1. **Interface**: A blueprint that defines the structure of an object. It specifies properties and methods but does not provide implementation.

2. **Class Implementation**: When a class implements an interface, it must provide implementations for all of the properties and methods declared in the interface.

# Syntax for Implementing an Interface in a Class:

```
interface InterfaceName {
  property: type;
  method(): returnType;
}

class ClassName implements InterfaceName {
  // Implementation of the properties and methods defined in the interface
}
```

# Example: Implementing an Interface in a Class

Let's say we want to define a simple interface `Person` that requires `name` and `age` properties and a `greet()` method. A class `Employee` will implement this interface.

```
interface Person {
  name: string;
  age: number;
  greet(): void;  // Method without implementation
}

class Employee implements Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): void {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const employee1 = new Employee("Alice", 30);
employee1.greet();  // Output: Hello, my name is Alice and I am 30 years old.
```

# Explanation:

* The `Person` interface defines the structure, with `name` (a `string`), `age` (a `number`), and a method `greet()` that returns `void`.

* The `Employee` class **implements** the `Person` interface. This means `Employee` must have the `name`, `age` properties and the `greet()` method.

* The `greet()` method is implemented in the `Employee` class to match the interface's signature.

* If we tried to omit the `greet()` method or define it incorrectly, TypeScript would give an error.

# Multiple Interfaces in a Class

A class can implement multiple interfaces, meaning it must provide the properties and methods required by each interface. This is useful when you want to combine functionality from different sources.

```
interface Contact {
  email: string;
  phoneNumber: string;
}

interface Address {
  street: string;
  city: string;
  country: string;
}

class Customer implements Contact, Address {
  email: string;
  phoneNumber: string;
  street: string;
  city: string;
  country: string;

  constructor(email: string, phoneNumber: string, street: string, city: string, country: string) {
    this.email = email;
    this.phoneNumber = phoneNumber;
    this.street = street;
    this.city = city;
    this.country = country;
  }

  printContactInfo(): void {
    console.log(`Contact Info: ${this.email}, ${this.phoneNumber}`);
  }

  printAddressInfo(): void {
    console.log(`Address: ${this.street}, ${this.city}, ${this.country}`);
  }
}

const customer1 = new Customer("alice@example.com", "123-456-7890", "123 Main St", "New York", "USA");
customer1.printContactInfo();  // Output: Contact Info: alice@example.com, 123-456-7890
customer1.printAddressInfo();  // Output: Address: 123 Main St, New York, USA
```

# Explanation:

* The `Customer` class implements two interfaces: `Contact` and `Address`.

* The class must have all properties from both interfaces (`email`, `phoneNumber`, `street`, `city`, and `country`).

* The class also provides methods to print the contact and address information.

# Optional Properties in Interfaces

Interfaces can also have optional properties, meaning a class implementing the interface can choose whether or not to implement those properties. Optional properties are defined using the `?` syntax.

```
interface Employee {
  name: string;
  age?: number;  // Optional property
  position: string;
}

class Developer implements Employee {
  name: string;
  position: string;
  
  constructor(name: string, position: string) {
    this.name = name;
    this.position = position;
  }
}

const dev = new Developer("Alice", "Developer");
console.log(dev.name);  // Output: Alice
console.log(dev.position);  // Output: Developer
```

# Explanation:

* The `Employee` interface has an optional `age` property (`age?`).

* The `Developer` class implements the `Employee` interface, but it is not required to have an `age` property, because it is optional.

* As a result, it works even if the `age` is omitted in the class implementation.

# Interface with Methods

Interfaces can define methods, and any class implementing that interface must provide the method's implementation. Here's an example where the interface requires a method that returns a value.

```
interface Shape {
  getArea(): number;  // Method to calculate the area of the shape
}

class Circle implements Shape {
  radius: number;

  constructor(radius: number) {
    this.radius = radius;
  }

  getArea(): number {
    return Math.PI * this.radius * this.radius;
  }
}

const circle = new Circle(5);
console.log(circle.getArea());  // Output: 78.53981633974483
```

# Explanation:

* The `Shape` interface requires a method `getArea()` to calculate the area of the shape.

* The `Circle` class implements the `Shape` interface and provides an implementation for `getArea()`.

* The `circle.getArea()` method calculates and returns the area of the circle.

# Extending Interfaces

Just like classes, interfaces can also extend other interfaces. When an interface extends another interface, it inherits the properties and methods of the parent interface. This is useful for creating more specific interfaces based on a common base.

```
interface Animal {
  name: string;
  speak(): void;
}

interface Dog extends Animal {
  breed: string;
}

class GoldenRetriever implements Dog {
  name: string;
  breed: string;

  constructor(name: string, breed: string) {
    this.name = name;
    this.breed = breed;
  }

  speak(): void {
    console.log(`${this.name} says woof!`);
  }
}

const dog = new GoldenRetriever("Buddy", "Golden Retriever");
dog.speak();  // Output: Buddy says woof!
```

# Explanation:

* The `Dog` interface extends the `Animal` interface, so it inherits `name` and `speak()`.

* The `GoldenRetriever` class implements the `Dog` interface and provides its own implementation of the `speak()` method.

# Summary:

* **Implementing Interfaces**: A class that implements an interface must provide an implementation for all properties and methods declared by the interface.

* **Multiple Interfaces**: A class can implement multiple interfaces and must provide the implementation for all properties and methods from all interfaces.

* **Optional Properties**: Interfaces can have optional properties (`property?`), which the implementing class can choose to provide or omit.

* **Method Implementations**: When an interface defines a method, any class that implements that interface must provide an implementation for the method.

* **Extending Interfaces**: Interfaces can extend other interfaces, allowing you to build more specific interfaces from a common base.

Implementing interfaces helps in creating reusable and flexible code by enforcing structure while allowing classes to define their own behavior.

