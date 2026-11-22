---
tags:
  - FORMAT
  - Typescript
---
# Function Overloading in TypeScript

In TypeScript, **function overloading** allows you to define a function with multiple signatures, enabling the same function to handle different types or numbers of arguments. This is useful when you want the function to behave differently based on the types or number of arguments provided.

TypeScript doesn't support traditional overloading like other languages (e.g., Java), but you can achieve similar behavior by providing multiple function signatures.

---

# What is Function Overloading?

Function overloading allows a function to have **different behavior** based on its input parameters. By defining **multiple signatures**, you can make the function more flexible while ensuring type safety.

# How Overloading Works:

* You define multiple **function signatures** with different parameter types.

* You then define a **single function implementation** that matches one of the signatures. TypeScript will figure out which signature to use based on the arguments passed.

---

 Syntax for Function Overloading in TypeScript

To define function overloading, you follow this pattern:

1. **Declare multiple function signatures** (with different parameter types).

2. **Provide a single implementation** for the function that handles the different argument combinations.

# Example of Function Overloading

```
// Overload signatures
function greet(name: string): string;
function greet(age: number): string;
function greet(isDay: boolean): string;

// Function implementation
function greet(arg: string | number | boolean): string {
    if (typeof arg === 'string') {
        return `Hello, ${arg}!`;
    } else if (typeof arg === 'number') {
        return `You are ${arg} years old.`;
    } else {
        return isDay ? "Good morning!" : "Good evening!";
    }
}

console.log(greet("Alice"));     // Output: "Hello, Alice!"
console.log(greet(25));          // Output: "You are 25 years old."
console.log(greet(true));        // Output: "Good morning!"
```

# Explanation:

* We define **three overload signatures**:

  * One that accepts a `string`.

  * One that accepts a `number`.

  * One that accepts a `boolean`.

* The **function implementation** accepts a union type (`string | number | boolean`) and performs different logic based on the type of the argument.

---

 Detailed Example with Multiple Parameters

# Example: Overloading with Multiple Parameters

Let's take the example of a function that can either add two numbers or concatenate two strings based on the input type:

```
// Overload signatures
function combine(a: number, b: number): number;
function combine(a: string, b: string): string;

// Function implementation
function combine(a: number | string, b: number | string): number | string {
    if (typeof a === "number" && typeof b === "number") {
        return a + b; // Add numbers
    } else if (typeof a === "string" && typeof b === "string") {
        return a + b; // Concatenate strings
    }
    throw new Error("Invalid argument types");
}

console.log(combine(5, 10));         // Output: 15
console.log(combine("Hello", " World"));  // Output: "Hello World"
```

# Explanation:

* The function `combine` has two overload signatures: one for numbers and one for strings.

* The **implementation** checks the types of `a` and `b` to determine whether to add the numbers or concatenate the strings.

---

 How TypeScript Resolves Overloading

TypeScript resolves function overloading by trying to match the argument types passed in the function call with one of the declared overload signatures. The implementation must handle all cases and return the correct type.

# Example: TypeScript Resolves Based on Signatures

```
function calculate(a: number, b: number): number; // Overload 1
function calculate(a: string, b: string): string; // Overload 2

function calculate(a: number | string, b: number | string): number | string {
    if (typeof a === 'number' && typeof b === 'number') {
        return a + b;
    } else if (typeof a === 'string' && typeof b === 'string') {
        return a + b;
    }
    throw new Error("Invalid input");
}

console.log(calculate(5, 10));          // Output: 15
console.log(calculate("Hello", " there"));  // Output: "Hello there"
```

*   
  TypeScript will pick the correct overload based on the types of the arguments passed in the function call.

---

 Overloading and Return Types

The return type of the implementation function should match the return types of the overload signatures. If the overload signatures return different types, the function implementation should be able to return any of those types.

# Example: Overloading with Different Return Types

```
// Overload signatures
function format(input: string): string;
function format(input: number): string;

// Function implementation
function format(input: string | number): string {
    if (typeof input === "string") {
        return `String: ${input}`;
    } else {
        return `Number: ${input.toFixed(2)}`;
    }
}

console.log(format("Hello"));  // Output: "String: Hello"
console.log(format(123.456));  // Output: "Number: 123.46"
```

# Explanation:

* The function `format` has overloads for both a `string` and a `number`.

* The implementation checks the type and formats the input accordingly, returning a string.

---

 Error Handling in Function Overloading

When implementing overloaded functions, it’s important to handle **invalid argument types** (i.e., when arguments don't match any of the overload signatures).

# Example: Handling Invalid Arguments

```
function getItem(id: number): string;
function getItem(id: string): string;

function getItem(id: number | string): string {
    if (typeof id === "number") {
        return `Item with ID number: ${id}`;
    } else if (typeof id === "string") {
        return `Item with name: ${id}`;
    }
    throw new Error("Invalid argument: id must be a number or string");
}

console.log(getItem(1));      // Output: "Item with ID number: 1"
console.log(getItem("Book")); // Output: "Item with name: Book"
console.log(getItem(true));   // ❌ Error: Invalid argument: id must be a number or string
```

*   
  Here, we handle cases where an invalid argument is passed by throwing an error, since the `id` should either be a `string` or a `number`.

---

 Why Use Function Overloading?

* **Code Clarity**: Overloading allows you to define multiple behaviors for the same function name, based on the arguments, making the code more readable.

* **Type Safety**: TypeScript ensures that the correct function signature is used and prevents passing incompatible types to the function.

* **Flexibility**: Function overloading provides the flexibility to handle different argument types or numbers in a single function.

---

# Summary of Function Overloading in TypeScript

| Concept | Description |
| ----- | ----- |
| **Function Overloading** | Allows a function to accept different parameter types or numbers of parameters. |
| **Multiple Signatures** | Define multiple signatures for the same function with different parameter types. |
| **Single Implementation** | Provide a single implementation that can handle all cases, based on the argument types passed. |
| **Type Safety** | TypeScript ensures that the correct overload is used based on the arguments, and prevents invalid argument types. |
| **Handling Invalid Types** | You can throw errors or return a default value when arguments don’t match the expected types or signatures. |

---

# Final Thoughts

* Function overloading provides flexibility in how a function can be used, while still enforcing **type safety**.

* While TypeScript doesn't support **traditional overloading**, the combination of **multiple signatures** and a **single implementation** achieves a similar result.

Would you like to see more advanced use cases of function overloading or have any further questions? 😊

