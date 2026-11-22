---
tags:
  - FORMAT
  - Typescript
---
In TypeScript, `readonly` is a modifier used to define properties or variables that cannot be reassigned after they have been initialized. It is commonly used with object properties or array elements to ensure immutability.

Here’s a breakdown of how it works:

# Readonly Properties in Interfaces and Types

You can define `readonly` properties in interfaces or types, ensuring that once the property is set, it can't be modified.

```
interface Person {
  readonly name: string;
  age: number;
}

const person: Person = { name: "Alice", age: 25 };

person.name = "Bob"; // Error: Cannot assign to 'name' because it is a read-only property
person.age = 26; // OK
```

In this example, the `name` property is marked as `readonly`, so attempting to change its value results in a compile-time error. However, the `age` property is mutable.

# Readonly Arrays

TypeScript also provides a way to make arrays immutable. When you use `ReadonlyArray<T>`, you get a type that prevents modifications like `push`, `pop`, and direct item reassignment.

```
const numbers: ReadonlyArray<number> = [1, 2, 3];

numbers[0] = 4; // Error: Index signature in type 'readonly number[]' only permits reading
numbers.push(4); // Error: Property 'push' does not exist on type 'readonly number[]'
```

# Readonly with `as const`

You can also use `as const` to create literal types, which makes both the properties and the array elements `readonly`.

```
const settings = {
  theme: 'dark',
  layout: 'grid'
} as const;

settings.theme = 'light'; // Error: Cannot assign to 'theme' because it is a read-only property
```

This effectively locks the values of `theme` and `layout` to the specific values defined, and they cannot be changed.

# Readonly with Classes

In classes, `readonly` can be used to ensure that properties cannot be changed once initialized, but can be assigned in the constructor.

```
class Car {
  readonly make: string;
  readonly model: string;

  constructor(make: string, model: string) {
    this.make = make;
    this.model = model;
  }
}

const car = new Car('Toyota', 'Corolla');
car.make = 'Honda'; // Error: Cannot assign to 'make' because it is a read-only property
```

# Key Points:

* **Read-only properties** cannot be reassigned after they have been initialized.

* This is useful for immutability, preventing accidental changes to important data.

* You can apply `readonly` to object properties, array elements, and class properties.

In summary, `readonly` is a useful feature to enhance the safety and immutability of your code, ensuring that values can't be unintentionally modified.

