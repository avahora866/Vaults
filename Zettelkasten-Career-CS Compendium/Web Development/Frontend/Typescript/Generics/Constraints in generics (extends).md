---
tags:
  - FORMAT
  - Typescript
---
# Constraints in Generics (`extends`)

In TypeScript, **constraints** in generics allow you to restrict the types that can be used with a generic type. This ensures that the type being passed as a generic parameter conforms to a specific structure or has certain properties or methods.

The `extends` keyword is used to specify a **constraint** for a generic type. It allows you to define that the generic type must extend a certain type (or implement a certain interface). This helps you ensure that the type passed to a generic is compatible with certain characteristics.

# Syntax for Constraints in Generics:

```
function functionName<T extends SomeType>(param: T): T {
  return param;
}
```

In this syntax:

* `T` is the generic type.

* `extends SomeType` means that `T` must be a subtype of `SomeType`. In other words, `T` must either be the `SomeType` itself or any class or interface that extends/implements it.

# Why Use Constraints?

1. **Enforcing Structure**: Ensure that the types used in your generic function, class, or interface have specific properties or methods.

2. **Type Safety**: Allow you to write more flexible and reusable code while still maintaining strong type safety.

# Basic Example of a Constraint

Suppose you have a function that accepts a parameter of a generic type `T`, but you want to ensure that `T` is an object with a `name` property (a string). You can use `extends` to enforce that the type passed to the generic type must have a `name` property.

```
interface HasName {
  name: string;
}

function greet<T extends HasName>(person: T): string {
  return `Hello, ${person.name}`;
}

const person = { name: "Alice", age: 25 };
console.log(greet(person));  // Output: Hello, Alice

// Error: Type '{ age: number }' is missing the 'name' property
// const noName = { age: 30 };
// console.log(greet(noName));  // Error
```

# Explanation:

* The `HasName` interface defines that an object must have a `name` property of type `string`.

* The `greet` function accepts a generic type `T` that **extends** `HasName`. This means that only objects with a `name` property (or objects that implement `HasName`) can be passed to the `greet` function.

* If an object does not have a `name` property, TypeScript will raise an error, ensuring that the function only accepts objects that meet the constraint.

# Using Constraints with Classes

You can also use constraints with generic classes to ensure that a class operates only on certain types. Let's say we want to create a class that works with types that extend a base class `Shape`:

```
class Shape {
  color: string;

  constructor(color: string) {
    this.color = color;
  }
}

class Circle extends Shape {
  radius: number;

  constructor(color: string, radius: number) {
    super(color);
    this.radius = radius;
  }
}

class Square extends Shape {
  sideLength: number;

  constructor(color: string, sideLength: number) {
    super(color);
    this.sideLength = sideLength;
  }
}

class Box<T extends Shape> {
  item: T;

  constructor(item: T) {
    this.item = item;
  }

  getItemColor(): string {
    return this.item.color;
  }
}

const circle = new Circle("red", 10);
const square = new Square("blue", 5);

const circleBox = new Box(circle);
const squareBox = new Box(square);

console.log(circleBox.getItemColor()); // Output: red
console.log(squareBox.getItemColor()); // Output: blue
```

# Explanation:

* `Shape` is a base class with a `color` property.

* `Circle` and `Square` are subclasses that extend `Shape`.

* The `Box<T extends Shape>` class can only accept objects that are instances of `Shape` or its subclasses (i.e., `Circle` or `Square`).

* When creating `circleBox` and `squareBox`, both classes are valid because `Circle` and `Square` extend `Shape`.

# Constraining Multiple Properties with Interfaces

You can use constraints in combination with interfaces to ensure that the generic type has multiple properties. For example, let's create a function that accepts a type that extends both an interface `HasName` and `HasAge`.

```
interface HasName {
  name: string;
}

interface HasAge {
  age: number;
}

function printInfo<T extends HasName & HasAge>(person: T): string {
  return `${person.name} is ${person.age} years old.`;
}

const person = { name: "Bob", age: 30 };
console.log(printInfo(person));  // Output: Bob is 30 years old.

const noAge = { name: "Alice" };
// Error: Type '{ name: string; }' is missing the 'age' property
// console.log(printInfo(noAge));  // Error
```

# Explanation:

* `HasName` and `HasAge` are two interfaces.

* The `printInfo` function accepts a generic type `T` that **extends** both `HasName` and `HasAge`. This means `T` must have both a `name` property (from `HasName`) and an `age` property (from `HasAge`).

* The function can only be called with objects that satisfy both constraints.

# Constraints on Function Arguments

You can use constraints to enforce that a function works only with certain kinds of arguments. For example, if you want a function that accepts a type `T` that has a `length` property (like a string, array, or any object with a `length` property), you can define the constraint as follows:

```
function printLength<T extends { length: number }>(item: T): number {
  return item.length;
}

console.log(printLength("Hello")); // Output: 5
console.log(printLength([1, 2, 3, 4])); // Output: 4

// Error: Property 'length' does not exist on type '{ name: string; }'
// console.log(printLength({ name: "Alice" }));  // Error
```

# Explanation:

* The `printLength` function accepts a generic parameter `T` that **extends** `{ length: number }`, which means `T` must have a `length` property.

* We can pass a string or array to the function because both of these types have a `length` property.

* If we try to pass an object that doesn't have a `length` property (like `{ name: "Alice" }`), TypeScript will raise an error.

# Using `extends` with Multiple Types

You can also use `extends` to apply constraints to multiple types (interfaces or classes). For example, if you want a function to only accept types that implement both `HasName` and `HasAge`, you can combine multiple constraints using `&` (intersection).

```
interface HasName {
  name: string;
}

interface HasAge {
  age: number;
}

function printFullInfo<T extends HasName & HasAge>(person: T): string {
  return `${person.name}, ${person.age} years old`;
}

const person = { name: "Charlie", age: 28 };
console.log(printFullInfo(person));  // Output: Charlie, 28 years old
```

# Explanation:

* `T extends HasName & HasAge` means that `T` must satisfy both the `HasName` interface and the `HasAge` interface.

* This allows for a more specific set of constraints, ensuring that the type has both a `name` and an `age`.

# Summary:

* **Constraints in Generics**: Using the `extends` keyword, you can restrict the types that a generic can accept.

* **Why Use Constraints**: They allow you to ensure that a generic type has specific properties, methods, or relationships with other types.

* **`extends`**: This keyword enforces that the type passed to the generic parameter must inherit from or implement the specified type (class or interface).

* **Combining Constraints**: You can combine multiple constraints using `&` to ensure that the type meets multiple requirements.

Constraints in generics allow you to write flexible yet type-safe code, providing both reusability and type safety while maintaining clear and predictable behavior.

