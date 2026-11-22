---
tags:
  - FORMAT
  - Typescript
---
# Rest Parameters in TypeScript

In TypeScript, **rest parameters** allow a function to accept an **arbitrary number of arguments** as an array. This feature makes it easier to work with functions that need to handle variable-length argument lists.

---

 Syntax of Rest Parameters

To define rest parameters in a function, use the `...` syntax followed by the name of the parameter. The rest parameter will **collect all remaining arguments into an array**.

# Basic Syntax:

```
function functionName(...args: type[]): returnType {
    // function body
}
```

*   
  `...args` is the rest parameter, and `args` will be an **array** containing all the arguments passed to the function.

* `type[]` indicates that the rest parameter is an array of the specified type.

---

 Example of Rest Parameters

Here’s a simple example where we use the rest parameter to sum any number of arguments.

# Example: Summing Multiple Numbers

```
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));          // Output: 6
console.log(sum(5, 10, 15, 20));    // Output: 50
console.log(sum(7));                // Output: 7
```

*   
  The `sum` function uses `...numbers` to collect all arguments passed to it into an array.

* It then sums all the numbers using `reduce`.

# Explanation:

* `...numbers: number[]` means that `numbers` is an array of `number` values.

* `reduce` is used to accumulate the sum of all the numbers.

---

 Rest Parameters vs. Arguments

Rest parameters allow you to **capture multiple arguments** into a single array, unlike traditional parameters that can only hold one value per argument.

# Example: Traditional Parameters

```
function add(a: number, b: number): number {
    return a + b;
}

console.log(add(1, 2));  // Output: 3
```

# Example: Rest Parameters

```
function addMultiple(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(addMultiple(1, 2, 3, 4));  // Output: 10
```

*   
  The `addMultiple` function can accept any number of arguments, while the traditional `add` function only accepts two.

---

 Rest Parameters with Other Parameters

You can use rest parameters **after** regular (non-rest) parameters in a function. However, **rest parameters must always be the last parameter** in the function signature.

# Example: Mixing Regular and Rest Parameters

```
function logMessage(level: string, ...messages: string[]): void {
    console.log(`[${level}]`, ...messages);
}

logMessage("INFO", "System is running smoothly");  // Output: "[INFO] System is running smoothly"
logMessage("ERROR", "Something went wrong", "Try again later");  // Output: "[ERROR] Something went wrong Try again later"
```

*   
  Here, `level` is a regular parameter, while `...messages` collects all additional arguments as an array of strings.

---

 Rest Parameters with Objects

You can also use rest parameters to collect **object properties** into an array.

# Example: Rest Parameters with Objects

```
function printDetails(name: string, ...details: string[]): void {
    console.log(`Name: ${name}`);
    details.forEach(detail => console.log(detail));
}

printDetails("Alice", "Age: 25", "Location: NYC", "Job: Developer");
// Output:
// Name: Alice
// Age: 25
// Location: NYC
// Job: Developer
```

*   
  In this case, the `...details` rest parameter collects all additional arguments into an array of strings.

---

 Rest Parameters with Other Data Types

While we often see rest parameters used with primitive types (like `number` or `string`), you can use them with **any data type** (including objects, arrays, etc.).

# Example: Rest Parameters with Objects

```
function createUser(name: string, ...tags: string[]): void {
    console.log(`Name: ${name}`);
    console.log(`Tags: ${tags.join(", ")}`);
}

createUser("Alice", "admin", "active", "developer");
// Output:
// Name: Alice
// Tags: admin, active, developer
```

*   
  Here, the `tags` parameter collects multiple string values into an array.

---

 Type Inference with Rest Parameters

TypeScript can **infer the type** of rest parameters automatically based on how they are used.

# Example: TypeScript Inferred Types

```
function mergeArrays(...arrays: number[][]): number[] {
    return arrays.flat();
}

const merged = mergeArrays([1, 2], [3, 4], [5, 6]);
console.log(merged);  // Output: [1, 2, 3, 4, 5, 6]
```

*   
  In this case, TypeScript automatically infers that `arrays` is of type `number[][]` because we pass arrays of `number[]` to the function.

---

 Rest Parameters in Arrow Functions

Rest parameters can also be used in **arrow functions**.

# Example: Rest Parameters in Arrow Function

```
const multiply = (...numbers: number[]): number => numbers.reduce((acc, num) => acc * num, 1);

console.log(multiply(2, 3, 4)); // Output: 24
console.log(multiply(5, 10));    // Output: 50
```

*   
  This arrow function uses rest parameters to multiply any number of values passed into it.

---

 Rest Parameters with Default Values

You can combine **default parameters** with **rest parameters**. Rest parameters will collect any extra arguments, while default parameters provide fallback values for specific arguments.

# Example: Default and Rest Parameters Combined

```
function buildMessage(greeting: string = "Hello", ...names: string[]): void {
    console.log(`${greeting}, ${names.join(", ")}!`);
}

buildMessage("Hi", "Alice", "Bob", "Charlie");  // Output: "Hi, Alice, Bob, Charlie!"
buildMessage(undefined, "Alice");               // Output: "Hello, Alice!"
```

*   
  In this example, the `greeting` parameter has a default value of `"Hello"`, and `...names` collects all remaining arguments as an array.

---

 Important Notes about Rest Parameters

* Rest parameters **must be the last parameter** in the function signature.

* They can accept **any number of arguments** and will always be an array.

* Rest parameters can be used with **different types** (e.g., `string[]`, `number[]`, or even complex objects like `{ id: number, name: string }[]`).

* **Rest parameters are always optional**: if no arguments are provided, the resulting array will be empty.

---

# Final Thoughts

* **Rest parameters** are a powerful tool in TypeScript that allow you to work with functions that can accept an arbitrary number of arguments.

* They are useful for building flexible functions, handling variable-length argument lists, and improving function signatures for complex logic.

Would you like more detailed examples or explanations on any specific part? 😊

