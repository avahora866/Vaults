---
tags:
  - FORMAT
  - Typescript
---
# Generic Interfaces and Classes in TypeScript

In TypeScript, **generics** allow you to write flexible, reusable code that can work with different types while still maintaining type safety. A **generic interface** or **generic class** can work with any type (or a set of types) without losing the type information, making your code more reusable and type-safe.

# Generic Interfaces

A **generic interface** allows you to define a structure that can operate on multiple types without specifying a concrete type upfront. Instead, you use a type parameter, which will be replaced with a specific type when the interface is implemented.

# Syntax for Generic Interfaces:

```
interface InterfaceName<T> {
  property: T;
  method(arg: T): T;
}
```

In this syntax:

* `T` is a **type parameter**, which acts as a placeholder for any type that will be used when the interface is implemented or used.

# Example of a Generic Interface:

Let's create a generic interface `Box` that represents a container that can hold any type of item.

```
interface Box<T> {
  value: T;
  getValue(): T;
  setValue(value: T): void;
}

class StringBox implements Box<string> {
  value: string;

  constructor(value: string) {
    this.value = value;
  }

  getValue(): string {
    return this.value;
  }

  setValue(value: string): void {
    this.value = value;
  }
}

const stringBox = new StringBox("Hello, TypeScript!");
console.log(stringBox.getValue());  // Output: Hello, TypeScript!

stringBox.setValue("New value");
console.log(stringBox.getValue());  // Output: New value
```

# Explanation:

* `Box<T>` is a generic interface where `T` is the type parameter that will be replaced with a concrete type when the interface is used.

* The `StringBox` class implements the `Box<string>` interface, meaning that `T` is replaced with `string`.

* The `getValue()` and `setValue()` methods are defined for `string` types.

# Generic Classes

Like interfaces, classes can also be made generic. This allows you to define classes that can work with different types while maintaining type safety.

# Syntax for Generic Classes:

```
class ClassName<T> {
  property: T;

  constructor(property: T) {
    this.property = property;
  }

  method(arg: T): T {
    return arg;
  }
}
```

In this syntax:

* `T` is a type parameter that will be replaced with a specific type when the class is instantiated or used.

# Example of a Generic Class:

Let's create a generic class `Pair` that represents a pair of items, where both items can be of any type.

```
class Pair<T, U> {
  first: T;
  second: U;

  constructor(first: T, second: U) {
    this.first = first;
    this.second = second;
  }

  getFirst(): T {
    return this.first;
  }

  getSecond(): U {
    return this.second;
  }
}

const pair1 = new Pair<string, number>("apple", 10);
console.log(pair1.getFirst());  // Output: apple
console.log(pair1.getSecond()); // Output: 10

const pair2 = new Pair<boolean, string>(true, "hello");
console.log(pair2.getFirst());  // Output: true
console.log(pair2.getSecond()); // Output: hello
```

# Explanation:

* The `Pair<T, U>` class is a generic class with two type parameters, `T` and `U`, which represent the types of the two items in the pair.

* The class constructor takes two parameters (`first` and `second`), and we use `T` and `U` to enforce type safety for these properties.

* In the example, `pair1` is a `Pair<string, number>`, while `pair2` is a `Pair<boolean, string>`. The types `T` and `U` are replaced with specific types when the class is instantiated.

# Using Constraints with Generics

Sometimes you may want to restrict the types that can be used with a generic type. You can do this using **constraints**, which define conditions that the generic type must meet. This is useful when you want the generic type to have certain properties or methods.

# Syntax for Constraints:

```
interface InterfaceName<T extends SomeType> {
  property: T;
}
```

In this syntax:

* `T extends SomeType` means that `T` must be a subtype of `SomeType` (or `SomeType` itself).

# Example of Generic with Constraints:

Let's say we want to create a generic class `Wrapper` that only works with objects that have a `name` property.

```
interface Named {
  name: string;
}

class Wrapper<T extends Named> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }

  getName(): string {
    return this.value.name;
  }
}

const namedObject = { name: "Alice", age: 25 };
const wrapper = new Wrapper(namedObject);
console.log(wrapper.getName());  // Output: Alice
```

# Explanation:

* The `Wrapper<T extends Named>` class uses a constraint (`T extends Named`) to ensure that the `T` type has a `name` property.

* The `Named` interface defines that any object used as `T` must have a `name` property of type `string`.

* In the example, `namedObject` is an object with a `name` property, so it can be passed to the `Wrapper` class.

* If you try to use a type that doesn't have a `name` property, TypeScript will show an error.

# Generic Interfaces with Multiple Types

You can also create interfaces with multiple type parameters to allow more flexibility in your designs.

```
interface Pair<T, U> {
  first: T;
  second: U;
}

function createPair<T, U>(first: T, second: U): Pair<T, U> {
  return { first, second };
}

const pair = createPair("Hello", 42);
console.log(pair.first);  // Output: Hello
console.log(pair.second); // Output: 42
```

# Explanation:

* The `Pair<T, U>` interface has two generic type parameters: `T` and `U`.

* The `createPair` function is a generic function that returns a `Pair<T, U>`, where the types of `first` and `second` can vary.

* When we call `createPair("Hello", 42)`, `T` is inferred as `string`, and `U` is inferred as `number`.

# Using Generics with Functions

Generics can also be used in functions, allowing you to create reusable, type-safe functions.

```
function identity<T>(value: T): T {
  return value;
}

const num = identity(10);       // T is inferred as number
const str = identity("hello");  // T is inferred as string

console.log(num);  // Output: 10
console.log(str);  // Output: hello
```

# Explanation:

* The `identity` function takes a parameter `value` of type `T` and returns it. The type `T` is inferred when the function is called.

* When `identity(10)` is called, `T` is inferred as `number`, and when `identity("hello")` is called, `T` is inferred as `string`.

# Summary:

1. **Generic Interfaces**:

   * Define a flexible, reusable structure that can work with different types.

   * Use a type parameter (`<T>`) to specify the type to be used when the interface is implemented.

2. **Generic Classes**:

   * Allow classes to work with any type by using type parameters (`<T>`).

   * Support multiple type parameters (`<T, U>`) for more complex scenarios.

3. **Constraints**:

   * Limit the types that can be used with a generic by specifying that the type must extend a certain class or interface.

4. **Reusable Functions**:

   * Generics can also be used in functions to provide flexibility and type safety.

5. **Type Inference**:

   * TypeScript can often infer the type of the generic based on the provided values, making it easier to use generics without explicitly specifying types.

Generics are powerful tools for creating reusable, type-safe code in TypeScript, and they allow you to write flexible code that works with different types while preserving strong typing.

