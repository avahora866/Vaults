---
tags:
  - FORMAT
  - Typescript
---
In TypeScript, **extending interfaces** allows you to create new interfaces that inherit the properties and methods of existing interfaces. This is useful for creating more specific or refined versions of an interface, building on top of an existing contract without repeating code.

You can extend interfaces using the `extends` keyword. When an interface extends another, it gets all the properties and methods from the parent interface, and you can also add new properties or methods specific to the child interface.

# Basic Example of Extending an Interface

When you extend an interface, the child interface inherits the properties from the parent interface.

## Example:

```
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  breed: string;
}

const dog: Dog = {
  name: "Buddy",
  age: 3,
  breed: "Golden Retriever"
};
```

In this example:

* The `Dog` interface extends the `Animal` interface.

* The `Dog` interface inherits the `name` and `age` properties from `Animal`.

* It also adds a new property `breed`.

Thus, the `dog` object must include all the properties from both `Animal` and `Dog` (`name`, `age`, and `breed`).

# Extending Multiple Interfaces

TypeScript allows you to extend more than one interface at a time. When you extend multiple interfaces, the child interface will inherit properties from all of them.

## Example:

```
interface Animal {
  name: string;
}

interface Mammal {
  hasFur: boolean;
}

interface Dog extends Animal, Mammal {
  breed: string;
}

const dog: Dog = {
  name: "Charlie",
  hasFur: true,
  breed: "Beagle"
};
```

Here, the `Dog` interface extends both `Animal` and `Mammal`. As a result, the `dog` object needs to satisfy all properties of the `Animal`, `Mammal`, and `Dog` interfaces.

# Method Inheritance in Extended Interfaces

In addition to properties, interfaces can also inherit methods when extended. You can define methods in a parent interface and reuse them in a child interface.

## Example:

```
interface Animal {
  name: string;
  speak(): void; // Method
}

interface Dog extends Animal {
  breed: string;
}

const dog: Dog = {
  name: "Rex",
  breed: "German Shepherd",
  speak: () => { console.log("Woof!"); }
};

dog.speak(); // Output: Woof!
```

In this example:

* The `Dog` interface extends the `Animal` interface, inheriting the `name` property and the `speak` method.

* The `dog` object implements both the properties and the method from the `Animal` interface, as well as the `breed` property from the `Dog` interface.

# Overriding Properties in Extended Interfaces

You can also override properties from the parent interface when extending it. This is useful if you want to change the type or make adjustments to an inherited property.

## Example:

```
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  age: string;  // Override the type of age to be a string
  breed: string;
}

const dog: Dog = {
  name: "Max",
  age: "5 years",  // Now age is a string instead of a number
  breed: "Bulldog"
};
```

In this example, the `Dog` interface overrides the `age` property of the `Animal` interface, changing its type from `number` to `string`.

# Extending Interfaces in Classes

When you use interfaces with classes, a class can implement one or more interfaces. If the class implements an interface that extends another, it must provide the properties and methods from all the extended interfaces.

## Example:

```
interface Animal {
  name: string;
  speak(): void;
}

interface Mammal extends Animal {
  hasFur: boolean;
}

class Dog implements Mammal {
  name: string;
  hasFur: boolean;

  constructor(name: string, hasFur: boolean) {
    this.name = name;
    this.hasFur = hasFur;
  }

  speak() {
    console.log("Woof!");
  }
}

const dog = new Dog("Bella", true);
dog.speak(); // Output: Woof!
```

Here:

* The `Mammal` interface extends the `Animal` interface, so it includes all properties of `Animal` (`name` and `speak`).

* The `Dog` class implements the `Mammal` interface, so it must provide implementations for all properties and methods defined by `Mammal` and `Animal`.

# Extending Interfaces with Optional Properties

When you extend an interface with optional properties, the child interface will also inherit those optional properties.

## Example:

```
interface Person {
  name: string;
  age?: number;  // Optional property
}

interface Employee extends Person {
  jobTitle: string;
}

const employee: Employee = {
  name: "Alice",
  jobTitle: "Engineer"
};

console.log(employee); // { name: "Alice", jobTitle: "Engineer" }
```

Here, the `Employee` interface extends the `Person` interface, inheriting the optional `age` property. The `employee` object can omit `age` and still be valid.

# Key Points:

* **`extends` keyword**: Used to inherit properties from another interface or interfaces.

* **Inheriting multiple interfaces**: A child interface can extend multiple parent interfaces, inheriting all their properties.

* **Overriding properties**: A child interface can override properties of the parent interface to change their type or behavior.

* **Classes and interfaces**: Classes can implement extended interfaces, ensuring they provide all the required properties and methods.

In summary, extending interfaces allows you to build upon existing structures while maintaining the benefits of type safety and code reuse. It helps you model complex relationships between different types of data in a clear and manageable way.

