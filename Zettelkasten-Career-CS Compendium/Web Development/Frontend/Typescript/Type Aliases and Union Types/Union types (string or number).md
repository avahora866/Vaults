---
tags:
  - FORMAT
  - Typescript
---
In TypeScript, **union types** allow you to define a type that can be one of multiple possible types. Instead of restricting a value to a single type, you can use a union to specify that the value can be one of several types. Union types are represented using the pipe (`|`) symbol.

# Syntax:

```
type MyType = Type1 | Type2 | Type3;
```

This means that a variable of type `MyType` can be of type `Type1`, `Type2`, or `Type3`.

# Basic Union Type Example

Let’s start with a simple union type:

```
let value: string | number;

value = "Hello";  // OK
value = 42;       // OK
value = true;     // Error: Type 'boolean' is not assignable to type 'string | number'
```

Here:

* `value` is a variable of type `string | number`, meaning it can hold either a `string` or a `number`.

* You can assign a string (e.g., `"Hello"`) or a number (e.g., `42`) to `value`, but not a boolean (e.g., `true`), because boolean is not part of the union type.

# Union Types in Functions

Union types are useful in functions where a parameter can accept multiple types. For example, a function that accepts either a string or a number can be defined as follows:

```
function printValue(value: string | number): void {
  console.log(value);
}

printValue("Hello");  // Output: Hello
printValue(42);       // Output: 42
```

In this case, the `printValue` function takes a `value` parameter that can be either a `string` or a `number`.

# Using Union Types with Arrays

Union types can also be used in arrays, allowing the array to hold elements of different types.

```
let items: (string | number)[] = ["apple", 42, "banana", 100];

console.log(items);  // Output: ["apple", 42, "banana", 100]
```

Here, the `items` array can contain both strings and numbers, thanks to the union type `(string | number)`.

# Using Union Types with Objects

You can use union types in object properties, making them flexible in terms of the types they accept.

```
interface User {
  name: string;
  id: string | number;  // id can be either a string or a number
}

const user1: User = { name: "Alice", id: 123 };
const user2: User = { name: "Bob", id: "abc123" };
```

In this example, the `id` property can either be a string or a number. This allows flexibility when dealing with different types of identifiers.

# Type Narrowing with Union Types

TypeScript provides a feature called **type narrowing**. When working with union types, you can narrow down the type by using control structures (such as `if` or `switch`) and checking the type of a variable at runtime.

```
function printId(id: string | number): void {
  if (typeof id === "string") {
    console.log("String ID: " + id);
  } else {
    console.log("Number ID: " + id);
  }
}

printId("abc123");  // Output: String ID: abc123
printId(456);       // Output: Number ID: 456
```

In this example:

* We check the type of `id` using `typeof`, which narrows the type from `string | number` to just `string` or `number` within the respective block.

* TypeScript automatically understands that `id` is either a `string` or a `number` based on the conditional check, providing type safety.

# Using Union Types with Literal Types

Union types are often used in conjunction with **literal types**, where a variable can only be one of a specific set of values.

```
type Direction = "up" | "down" | "left" | "right";

function move(direction: Direction): void {
  console.log(`Moving ${direction}`);
}

move("up");    // OK
move("down");  // OK
move("forward"); // Error: Argument of type '"forward"' is not assignable to parameter of type 'Direction'
```

In this case, `Direction` is a union of string literals (`"up"`, `"down"`, `"left"`, `"right"`). The `move` function can only accept one of these values, and trying to pass a value like `"forward"` will result in a compile-time error.

# Union Types with Type Aliases

You can also define a union type using a **type alias**, which can simplify complex types:

```
type StringOrNumber = string | number;

function add(a: StringOrNumber, b: StringOrNumber): StringOrNumber {
  return a + b;
}

console.log(add(10, 5));      // Output: 15
console.log(add("Hello, ", "World!")); // Output: Hello, World!
```

In this example:

* We define a `StringOrNumber` type alias that represents the union of `string` and `number`.

* The `add` function accepts two parameters of type `StringOrNumber` and returns a result of the same type.

# Key Points:

* **Union types** allow a value to be one of multiple types, making it more flexible.

* Use the `|` (pipe) symbol to combine multiple types.

* Type narrowing is useful for checking the type of a value at runtime, making your code more type-safe.

* Union types work well with functions, arrays, objects, and type aliases.

* They can be used to define more flexible and reusable data structures.

# Conclusion:

Union types in TypeScript help you write more flexible and expressive code by allowing a variable, parameter, or property to accept multiple types. They are a powerful tool for handling situations where a value could be one of many possibilities, while still maintaining the type safety that TypeScript provides.

