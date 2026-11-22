---
tags:
  - FORMAT
  - Typescript
---
In TypeScript, a **type alias** is a way to create a new name for a type. It allows you to define custom types that can be used throughout your code. Type aliases can be particularly useful for simplifying complex types, improving readability, and reusing types.

The syntax for defining a type alias is the `type` keyword followed by the alias name and the type definition.

# Syntax:

```
type AliasName = TypeDefinition;
```

For example:

```
type User = {
  name: string;
  age: number;
};
```

In this case:

* `User` is a type alias.

* The type `User` is defined as an object with two properties: `name` (a string) and `age` (a number).

# Basic Type Alias Example

Here’s a simple example where we define a `User` type alias and use it to create a variable:

```
type User = {
  name: string;
  age: number;
};

const user1: User = {
  name: "Alice",
  age: 30
};

const user2: User = {
  name: "Bob",
  age: 25
};
```

In this example:

* We define a type alias `User` for an object type that has two properties: `name` (of type `string`) and `age` (of type `number`).

* We then create `user1` and `user2` variables of type `User` to store user information.

# Using Type Aliases with Complex Types

Type aliases are also useful for defining complex or composite types, such as arrays, unions, and intersections.

## Example with an Array:

```
type StringArray = string[];

const fruits: StringArray = ["apple", "banana", "cherry"];
```

Here, `StringArray` is a type alias for an array of strings. This allows you to use `StringArray` wherever you would normally use `string[]`.

## Example with Unions:

```
type Status = "active" | "inactive" | "pending";

const userStatus: Status = "active";  // OK
const invalidStatus: Status = "disabled";  // Error: Type '"disabled"' is not assignable to type 'Status'
```

In this case, the `Status` type alias is a **union** type, meaning it can be one of the specific values `"active"`, `"inactive"`, or `"pending"`.

## Example with Intersections:

```
type Address = {
  street: string;
  city: string;
};

type Contact = {
  email: string;
  phone: string;
};

type UserWithContact = User & Address & Contact;

const userWithContact: UserWithContact = {
  name: "Charlie",
  age: 40,
  street: "123 Main St",
  city: "Springfield",
  email: "charlie@example.com",
  phone: "555-1234"
};
```

Here, `UserWithContact` is an intersection type that combines `User`, `Address`, and `Contact` types. This means that the `UserWithContact` type will require all the properties from `User`, `Address`, and `Contact`.

# Type Alias for Functions

Type aliases can also be used to define the types of functions.

```
type GreetFunction = (name: string) => string;

const greet: GreetFunction = (name) => `Hello, ${name}!`;

console.log(greet("Alice"));  // Output: Hello, Alice!
```

In this example:

* `GreetFunction` is a type alias for a function that takes a `string` as an argument and returns a `string`.

* The `greet` function is defined to match the `GreetFunction` type.

# Type Aliases for Objects with Optional Properties

Type aliases can also be used with optional properties. You can define properties as optional by appending a `?` to the property name.

```
type UserProfile = {
  name: string;
  age: number;
  email?: string;  // Optional property
};

const profile1: UserProfile = { name: "Alice", age: 30 };
const profile2: UserProfile = { name: "Bob", age: 25, email: "bob@example.com" };
```

In this example, the `email` property is optional. The `UserProfile` type alias allows objects with or without an `email` property.

# Type Alias vs Interface

While both **type aliases** and **interfaces** can define shapes of objects, there are some differences. Here's a comparison:

* **Type Aliases** can represent more complex types, such as unions, intersections, tuples, and function types. They also allow you to alias primitive types or other types.

* **Interfaces** are generally used to define object shapes (though they can extend other interfaces). They support declaration merging, which means you can define the same interface multiple times and TypeScript will merge the definitions.

## Example Comparison:

```
// Type alias for a function
type Greeter = (message: string) => void;

// Interface for a function
interface GreeterInterface {
  (message: string): void;
}
```

In most cases, **type aliases** and **interfaces** can be used interchangeably for defining object types. However, **interfaces** are better suited when you need to extend or implement other interfaces.

# Key Points:

* **Type aliases** are used to create custom names for types, making the code more readable and reusable.

* You can use type aliases to define simple object types, arrays, function types, unions, intersections, and more.

* Type aliases are particularly useful for more complex types that can’t be easily expressed using interfaces.

* You can use type aliases to define optional properties or other specialized types.

In summary, **type aliases** provide a powerful way to define and manage types in TypeScript, making your code more flexible, readable, and easier to maintain.

