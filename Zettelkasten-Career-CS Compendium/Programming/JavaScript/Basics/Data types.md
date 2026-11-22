---
tags:
  - FORMAT
  - JavaScript
---
# JavaScript Data Types

In JavaScript, data types define the type of value a variable can hold. There are **primitive types** and **non-primitive types** (also known as reference types). Understanding these data types is key to writing effective JavaScript code.

## Primitive Data Types

Primitive data types are immutable and are passed by value. The eight primitive data types in JavaScript are:

### String

A string is a sequence of characters used for storing and manipulating text.

* Strings can be created using single quotes (`'`), double quotes (`"`), or backticks (`` ` ``) for template literals.

* Strings are immutable, meaning once created, they cannot be changed.

#### Example:

```javascript
let name = "John Doe";   // using double quotes
let message = 'Hello!';  // using single quotes
let greet = `Hi, ${name}`; // using template literals (backticks)
```

### Number

The number data type represents both integer and floating-point numbers. JavaScript doesn't distinguish between different types of numbers (e.g., integer vs float) as some other languages do.

* JavaScript has **NaN** (Not-a-Number), which is a special value when an operation fails to produce a valid number.

* **Infinity** and **\-Infinity** represent values that exceed the largest or smallest numbers that JavaScript can handle.

#### Example:

```javascript
let age = 25;        // integer
let price = 19.99;   // floating-point number
let notNumber = NaN; // Not a Number
```

### Boolean

A boolean represents a logical value that can only be either `true` or `false`.

* It is typically used for conditional testing and flow control (e.g., `if` statements).

#### Example:

```javascript
let isActive = true;   // true value
let isCompleted = false; // false value
```

### Null

`null` is a special value that represents the intentional absence of any object value.

* It is explicitly assigned to a variable to indicate that it does not hold any value.

#### Example:

```javascript
let user = null; // The variable is intentionally empty or has no value.
```

### Undefined

`undefined` is a type as well as a value. It means a variable has been declared but has not yet been assigned a value.

* It is also the default value for uninitialized variables or function arguments that are not provided.

#### Example:

```javascript
let result;  // variable is declared but not assigned
console.log(result); // Output: undefined
```

### Symbol

A `symbol` is a unique and immutable value primarily used to create anonymous and unique property keys in objects.

* Symbols are often used in situations where you want to guarantee that a property key is unique, even across different codebases or applications.

#### Example:

```javascript
let sym1 = Symbol('description');
let sym2 = Symbol('description');
console.log(sym1 === sym2); // Output: false, because symbols are unique
```

### BigInt

`BigInt` is a relatively new data type that allows for the representation of integers with arbitrary precision. It is useful for dealing with very large numbers beyond the `Number` data type's limit.

* A `BigInt` is created by appending `n` to the number or using the `BigInt()` constructor.

#### Example:

```javascript
let bigNumber = 1234567890123456789012345678901234567890n;
let anotherBig = BigInt(12345678901234567890);
console.log(bigNumber + anotherBig); // Output: a large BigInt number
```

## Non-Primitive Data Types (Reference Types)

Non-primitive data types are mutable and are passed by reference, meaning that when you assign one object to another, both variables refer to the same memory location.

### Object

An object is a collection of key-value pairs, where the keys are strings (or symbols), and the values can be any data type. Objects can represent more complex structures and are the foundation for most data structures in JavaScript.

* Objects are used to store data and functions.

#### Example:

```javascript
let person = {
  name: "Alice",
  age: 30,
  isStudent: false
};
console.log(person.name);  // Output: Alice
```

### Array

An array is an ordered collection of values, which can be of any data type. Arrays are zero-indexed, meaning the first element is at index `0`. Arrays can be used to store lists of items.

#### Example:

```javascript
let numbers = [1, 2, 3, 4, 5];
let fruits = ["apple", "banana", "cherry"];
console.log(numbers[0]);  // Output: 1
console.log(fruits[1]);   // Output: banana
```

## Summary of JavaScript Data Types

| Data Type | Description | Example |
| ----- | ----- | ----- |
| **String** | Sequence of characters (text) | `"Hello"`, `'World'`, `\`Hi\`\` |
| **Number** | Integer or floating-point number | `42`, `3.14`, `NaN` |
| **Boolean** | Logical value (`true` or `false`) | `true`, `false` |
| **Null** | Represents a deliberate non-value or absence of any object | `null` |
| **Undefined** | A variable that has been declared but not assigned a value | `undefined` |
| **Symbol** | Unique and immutable value used for unique property keys | `Symbol('description')` |
| **BigInt** | Large integers beyond `Number` type limit | `123456789012345678901234567890n` |
| **Object** | Collection of key-value pairs (can store complex data) | `{ name: "Alice", age: 30 }` |
| **Array** | Ordered collection of values (can store multiple types) | `[1, 2, 3, "Hello", false]` |

### Key Points:

* **Primitive types** (string, number, boolean, null, undefined, symbol, BigInt) are immutable and passed by value.

* **Non-primitive types** (object, array) are mutable and passed by reference.

Each data type serves a specific purpose in JavaScript, and understanding their differences will help you write more efficient and predictable code.