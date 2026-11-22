---
tags:
  - FORMAT
  - Typescript
---
In TypeScript, the `?` (optional) operator is used to mark properties or parameters as optional. When a property or parameter is optional, it means that it may or may not be provided, and TypeScript will allow `undefined` as a valid value for that property or parameter.

# Optional Properties in Interfaces and Types

When defining an interface or a type, you can use the `?` operator after a property name to indicate that the property is optional. This means that an object adhering to this interface or type can either include or omit that property.

## Example with Interface:

```
interface Person {
  name: string;
  age?: number;  // age is optional
}

const person1: Person = { name: "Alice" };  // OK, age is not provided
const person2: Person = { name: "Bob", age: 30 };  // OK, age is provided
```

In the above example, the `age` property is optional. `person1` doesn't include `age`, but that's perfectly fine. The `name` property is still required because it's not marked as optional.

# Optional Function Parameters

You can also use the `?` operator to make function parameters optional. If a parameter is optional, you can either pass it or omit it when calling the function.

## Example with Function:

```
function greet(name: string, age?: number): string {
  if (age !== undefined) {
    return `Hello, ${name}. You are ${age} years old.`;
  } else {
    return `Hello, ${name}.`;
  }
}

console.log(greet("Alice"));  // "Hello, Alice."
console.log(greet("Bob", 30));  // "Hello, Bob. You are 30 years old."
```

Here, `age` is an optional parameter. When calling `greet`, you can either provide `age` or leave it out, and TypeScript handles it gracefully by giving `age` the type `number | undefined`.

# Optional Array Elements

You can also use the `?` operator within array types to specify optional elements.

## Example with Arrays:

```
type Tuple = [string, number?, boolean?]; // Both the number and boolean are optional

const validTuple1: Tuple = ["Hello"];  // OK
const validTuple2: Tuple = ["Hello", 42];  // OK
const validTuple3: Tuple = ["Hello", 42, true];  // OK
```

In this case, the `number` and `boolean` elements in the tuple are optional, so you can create a tuple with just a string, or with a string and one or both of the optional elements.

# Optional Properties with Classes

When you define a class in TypeScript, you can also make class properties optional using the `?` operator.

## Example with Classes:

```
class Car {
  model: string;
  year?: number;  // optional property

  constructor(model: string, year?: number) {
    this.model = model;
    if (year !== undefined) {
      this.year = year;
    }
  }
}

const car1 = new Car("Toyota");  // OK, year is not provided
const car2 = new Car("Honda", 2020);  // OK, year is provided
```

In this example, the `year` property is optional, and the class constructor also takes an optional `year` parameter.

# Optional in Nested Structures

The `?` operator can also be used in nested structures. This is useful when working with deeply nested objects, where some properties might not always be present.

## Example with Nested Objects:

```
interface Address {
  street: string;
  city?: string;  // optional
}

interface User {
  name: string;
  address?: Address;  // optional
}

const user1: User = { name: "Alice" };  // OK, address is not provided
const user2: User = { name: "Bob", address: { street: "123 Main St", city: "Wonderland" } };  // OK, address is provided
```

Here, the `address` property in the `User` interface is optional, and the `city` property inside `Address` is also optional.

# Key Points:

* The `?` operator makes a property or parameter optional, meaning it may or may not be provided.

* It is commonly used in interfaces, function parameters, and class properties.

* TypeScript automatically infers that an optional property or parameter can be `undefined`.

* When using optional properties or parameters, you should handle `undefined` values appropriately in your code.

In short, optional properties make your code more flexible by allowing certain values to be omitted while still maintaining type safety.

