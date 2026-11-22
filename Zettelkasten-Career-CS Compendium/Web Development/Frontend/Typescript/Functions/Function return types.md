---
tags:
  - FORMAT
  - Typescript
---
# Function Return Types in TypeScript

In TypeScript, you can explicitly specify the **return type** of a function. This ensures that the function will return a value of the specified type. If the function tries to return something that doesn’t match the declared return type, TypeScript will throw an error.

---

# Declaring Return Types

You can declare a function’s return type by specifying it after the parameter list, using a colon (`:`) and the type you want.

# Example: Basic Function with Return Type

```
function add(a: number, b: number): number {
    return a + b;
}

let result = add(5, 3); // result: number
console.log(result); // Output: 8
```

*   
  The return type `: number` ensures that the function **must return a number**.

* If `add` tries to return something other than a `number`, TypeScript will show an error.

---

# Return Type with `void` (No Return Value)

If a function does **not return any value**, the return type should be `void`. This indicates that the function is meant for **side effects** (e.g., printing to the console) and doesn't return anything.

# Example: Function with `void` Return Type

```
function logMessage(message: string): void {
    console.log(message);
}

logMessage("Hello, TypeScript!"); // Output: "Hello, TypeScript!"
```

*   
  The return type is `void`, meaning the function will not return a value.

* Trying to return something in this case will cause an error.

```
function logMessage(message: string): void {
    return "Something"; // ❌ Error: Type 'string' is not assignable to type 'void'.
}
```

---

# Functions with Multiple Return Types (`union` Types)

You can return **multiple types** from a function by using a **union type** (`|`). This means the function can return one of several types.

# Example: Function with Union Type as Return Type

```
function getResult(success: boolean): number | string {
    if (success) {
        return 42; // return a number
    } else {
        return "Error"; // return a string
    }
}

let result1 = getResult(true);  // result1: number
let result2 = getResult(false); // result2: string
console.log(result1, result2); // Output: 42 "Error"
```

*   
  The return type is `number | string`, meaning the function can return either a `number` or a `string`.

---

# Returning `never` (For Functions That Never Return)

The `never` type is used for functions that **never return** (e.g., they throw errors or have infinite loops).

# Example: Function with `never` Return Type

```
function throwError(message: string): never {
    throw new Error(message); // This will stop execution
}

throwError("Something went wrong!"); // Execution stops here
```

*   
  The `throwError` function never completes its execution and always throws an error.

* `never` indicates that the function will **never return a value**.

---

# Implicit Return Types

If you don’t explicitly declare the return type, TypeScript will infer the return type based on the returned value.

# Example: Implicit Return Type

```
function multiply(a: number, b: number) {
    return a * b;
}

let result = multiply(4, 5); // TypeScript infers the return type as `number`
console.log(result); // Output: 20
```

*   
  TypeScript inferred that `multiply` returns a `number` based on the return value.

* Although you didn’t explicitly specify the return type, TypeScript still understands it from the logic.

---

# Functions Returning Objects or Arrays

When a function returns an object or array, you can specify the structure of the return value by using **object types** or **array types**.

# Example: Function Returning an Object

```
function createUser(name: string, age: number): { name: string; age: number } {
    return { name, age };
}

let user = createUser("Alice", 25);
console.log(user); // Output: { name: "Alice", age: 25 }
```

*   
  The return type `: { name: string; age: number }` ensures that the returned object has properties `name` (string) and `age` (number).

---

# Return Types in Arrow Functions

In arrow functions, the return type can be specified in the same way as regular functions.

# Example: Arrow Function with Return Type

```
const subtract = (a: number, b: number): number => {
    return a - b;
};

let result = subtract(10, 3);
console.log(result); // Output: 7
```

*   
  `subtract` has a return type of `number`, and TypeScript ensures the function returns a `number`.

---

# Returning `null` or `undefined`

You can also specify that a function **returns `null` or `undefined`** by using the `null` or `undefined` types.

# Example: Function Returning `null`

```
function findUser(id: number): null {
    return null; // Returns `null`
}

let user = findUser(101); // user: null
```

# Example: Function Returning `undefined`

```
function logMessage(message: string): undefined {
    console.log(message);
    return undefined; // Returns `undefined`
}

logMessage("Logging a message"); // Output: "Logging a message"
```

---

# Summary of Function Return Types

| Return Type | Description | Example |
| ----- | ----- | ----- |
| `number` | Function returns a number | `function add(a: number, b: number): number { return a + b; }` |
| `void` | Function does not return anything | `function log(message: string): void { console.log(message); }` |
| `never` | Function never returns (e.g., throws an error) | `function throwError(message: string): never { throw new Error(message); }` |
| \`number | string\` | Function can return either a number or a string |
| `object` | Function returns an object with a specific structure | `function createPerson(name: string, age: number): { name: string; age: number } { return { name, age }; }` |
| `null` or `undefined` | Function returns `null` or `undefined` | `function findUser(id: number): null { return null; }` |

---

# When to Use Explicit Return Types

* **Clarity and Documentation**: It’s often a good practice to specify the return type for **clarity** and to serve as **documentation** for future developers.

* **Function Composition**: When combining functions or working with complex logic, having explicit return types helps **avoid bugs** and makes your code easier to understand.

* **Avoiding Errors**: Explicit return types make sure the function returns what you expect and TypeScript can help catch errors early.

---

Would you like more examples or specific use cases for function return types? 😊

