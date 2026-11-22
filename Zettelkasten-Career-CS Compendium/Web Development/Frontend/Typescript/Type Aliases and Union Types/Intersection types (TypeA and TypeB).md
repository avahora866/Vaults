---
tags:
  - FORMAT
  - Typescript
---
In TypeScript, **intersection types** allow you to combine multiple types into one, creating a new type that includes all properties and methods from the intersected types. Intersection types are represented using the `&` (ampersand) operator. This means that a variable of an intersection type must satisfy all the conditions (properties, methods, etc.) of the combined types.

# Syntax:

```
type TypeA & TypeB;
```

This means that a value of this new type must have all the properties and methods of both `TypeA` and `TypeB`.

# Basic Example of Intersection Types

Let’s start with a simple example of intersection types. In this case, we combine two types: `Person` and `Employee`.

```
interface Person {
  name: string;
  age: number;
}

interface Employee {
  employeeId: number;
  position: string;
}

type EmployeeDetails = Person & Employee;

const employee: EmployeeDetails = {
  name: "Alice",
  age: 30,
  employeeId: 123,
  position: "Developer"
};

console.log(employee);
```

In this example:

* We have two interfaces: `Person` and `Employee`.

* The `EmployeeDetails` type is an intersection of both interfaces (`Person & Employee`).

* The `employee` object must have all properties from both `Person` and `Employee`: `name`, `age`, `employeeId`, and `position`.

Thus, `EmployeeDetails` is a type that requires all properties from both `Person` and `Employee`, which means any object of this type must include both sets of properties.

# Combining Multiple Types

You can combine more than two types with intersection types. Let’s extend the previous example by adding a `Department` type.

```
interface Department {
  departmentName: string;
}

type EmployeeWithDepartment = Person & Employee & Department;

const employeeWithDept: EmployeeWithDepartment = {
  name: "Bob",
  age: 25,
  employeeId: 456,
  position: "Manager",
  departmentName: "Sales"
};

console.log(employeeWithDept);
```

In this case:

* `EmployeeWithDepartment` is an intersection of `Person`, `Employee`, and `Department`.

* The `employeeWithDept` object must have all properties from `Person`, `Employee`, and `Department`.

# Intersection Types with Classes

Intersection types also work with classes. If a class implements multiple interfaces, it effectively represents the intersection of those interfaces.

```
interface Animal {
  species: string;
}

interface Bird {
  canFly: boolean;
}

class Eagle implements Animal, Bird {
  species: string;
  canFly: boolean;

  constructor(species: string, canFly: boolean) {
    this.species = species;
    this.canFly = canFly;
  }
}

const eagle = new Eagle("Eagle", true);
console.log(eagle); // { species: "Eagle", canFly: true }
```

Here:

* The `Eagle` class implements both the `Animal` and `Bird` interfaces, which means the class must have all properties from both interfaces (`species` and `canFly`).

* The class represents the intersection of `Animal` and `Bird`.

# Intersection Types with Functions

Intersection types can also be used to combine function types. When combining functions with intersection types, the resulting type must satisfy all the function signatures.

```
type FunctionA = (a: number) => string;
type FunctionB = (b: string) => boolean;

type CombinedFunction = FunctionA & FunctionB;

const myFunction: CombinedFunction = (input: any) => {
  if (typeof input === "number") {
    return `Number: ${input}`;
  } else if (typeof input === "string") {
    return input.length > 5;
  }
};

console.log(myFunction(42));         // "Number: 42"
console.log(myFunction("Hello"));    // true
console.log(myFunction("Hi"));       // false
```

Here:

* `FunctionA` takes a `number` and returns a `string`.

* `FunctionB` takes a `string` and returns a `boolean`.

* `CombinedFunction` is the intersection of both, meaning it can handle both a `number` and a `string` as input and returns either a `string` or a `boolean` depending on the input type.

# Intersection Types with Optional Properties

Intersection types can also work with optional properties. If one of the types in the intersection contains optional properties, the resulting type will also have those optional properties.

```
interface Person {
  name: string;
  age?: number;  // Optional property
}

interface Contact {
  phone: string;
  email: string;
}

type PersonContact = Person & Contact;

const personContact: PersonContact = {
  name: "Charlie",
  phone: "123-456-7890",
  email: "charlie@example.com"
};

console.log(personContact);
```

In this example:

* `Person` has an optional property `age`.

* `Contact` has two required properties: `phone` and `email`.

* The `PersonContact` type is the intersection of `Person` and `Contact`, meaning the resulting type must have all properties from both types (`name`, `phone`, and `email`), and `age` is still optional.

# Practical Use Case: Combining Types for Flexible Objects

A practical use of intersection types is when combining different sets of properties into one type for more flexible and reusable objects.

```
type Vehicle = {
  make: string;
  model: string;
};

type ElectricVehicle = {
  batteryCapacity: number;
};

type ElectricCar = Vehicle & ElectricVehicle;

const myElectricCar: ElectricCar = {
  make: "Tesla",
  model: "Model S",
  batteryCapacity: 100
};

console.log(myElectricCar);
```

Here:

* `ElectricCar` is the intersection of `Vehicle` and `ElectricVehicle`, so it includes properties from both (`make`, `model`, and `batteryCapacity`).

* `myElectricCar` must have all the properties from both `Vehicle` and `ElectricVehicle`.

# Key Points:

* **Intersection types** combine multiple types into one, creating a new type that must satisfy all the combined types.

* They are represented using the `&` (ampersand) operator.

* Intersection types can be used with **objects**, **functions**, **classes**, and **other types**.

* They are useful when you want to create more complex types by combining multiple existing types, like when merging properties from multiple interfaces or when combining different function signatures.

* They support both **required** and **optional properties**.

# Conclusion:

Intersection types are a powerful feature in TypeScript that enable you to create types that combine multiple existing types. They are helpful in scenarios where an object or value must satisfy multiple different contracts or behaviors, allowing you to model complex relationships and structures efficiently while maintaining type safety.

