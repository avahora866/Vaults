---
tags:
  - FORMAT
  - JavaScript
---
# Function Declarations vs. Function Expressions in JavaScript

JavaScript provides two primary ways to define functions: **function declarations** and **function expressions**. While they may look similar, they have key differences in how they are hoisted and used.

---

## Function Declaration

A **function declaration** defines a function with the `function` keyword, followed by the function name. These functions are hoisted, meaning they can be called **before they are defined** in the code.

### Syntax:

```javascript
function functionName(parameters) {
  // Function body
}
```

### Example:

```javascript
sayHello(); // ✅ Works due to hoisting

function sayHello() {
  console.log("Hello, world!");
}

sayHello(); // Output: Hello, world!
```

### Key Characteristics:

* **Named function**: Always has a function name (`sayHello` in this case).

* **Hoisted**: Can be called before its declaration in the code.

---

## Function Expression

A **function expression** defines a function and assigns it to a variable. These functions are **not hoisted**, meaning they cannot be called before their definition.

### Syntax:

```javascript
const functionName = function(parameters) {
  // Function body
};
```

### Example:

```javascript
// sayHello(); ❌ Error: Cannot access 'sayHello' before initialization

const sayHello = function() {
  console.log("Hello, world!");
};

sayHello(); // Output: Hello, world!
```

### Key Characteristics:

* **Anonymous or named function**: The function inside can be anonymous (no name) or named.

* **Not hoisted**: Cannot be used before it is defined.

---

## Named vs. Anonymous Function Expressions

Function expressions can be either **anonymous** (no function name) or **named** (has a name inside).

### Anonymous Function Expression

```javascript
const greet = function() {
  console.log("Hello!");
};
```

### Named Function Expression

```javascript
const greet = function sayHello() {
  console.log("Hello!");
};
```

The difference is:

* **Anonymous functions** don’t have a name, making them harder to reference inside themselves (e.g., recursion).

* **Named functions** help with debugging and self-referencing.

---

## Arrow Function Expressions (ES6)

An arrow function is a shorter syntax for function expressions and does not have its own `this` value.

### Syntax:

```javascript
const functionName = (parameters) => {
  // Function body
};
```

### Example:

```javascript
const sayHello = () => {
  console.log("Hello, world!");
};

sayHello(); // Output: Hello, world!
```

### Arrow Function Shortcuts:

* **One parameter (without parentheses):**

```javascript
const square = x => x * x;
console.log(square(4)); // Output: 16
```

*   
  **Multiple parameters (with parentheses):**

```javascript
const add = (a, b) => a + b;
console.log(add(2, 3)); // Output: 5
```

*   
  **Single expression (implicit return):**

```javascript
const greet = () => "Hello!";
console.log(greet()); // Output: Hello!
```

---

## Key Differences: Function Declaration vs. Function Expression

| Feature | Function Declaration | Function Expression |
| ----- | ----- | ----- |
| **Hoisting** | Yes (can be called before definition) | No (cannot be called before definition) |
| **Syntax** | `function name() {}` | `const name = function() {}` or `const name = () => {}` |
| **Self-reference** | Named function available inside itself | Anonymous function cannot reference itself |
| **Use case** | Best for defining reusable functions | Useful for inline functions, callbacks |

---

## When to Use Each?

* **Use function declarations** when defining reusable functions that are called multiple times throughout the code.

* **Use function expressions** when defining functions inside another function, storing them in variables, or passing them as arguments (e.g., callbacks).

* **Use arrow functions** for concise function expressions, especially for callbacks and simple operations.

By understanding these differences, you can write cleaner, more predictable JavaScript code\! 🚀

