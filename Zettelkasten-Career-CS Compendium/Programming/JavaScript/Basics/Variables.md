---
tags:
  - FORMAT
  - JavaScript
---
# JavaScript Variables (let, const, var)

In JavaScript, variables are used to store and manage data. There are three main ways to declare variables: `let`, `const`, and `var`. Each one has specific characteristics and scopes. Let's break down each:

## `var`

`var` is the traditional way of declaring variables in JavaScript. However, it has some quirks that can lead to unintended behavior.

### Characteristics of `var`:

* **Function-scoped**: `var` is scoped to the nearest function, meaning if declared inside a function, it is only accessible within that function. If declared outside a function, it becomes globally scoped.

* **Hoisting**: Variables declared with `var` are hoisted to the top of their scope, but their value is not assigned until the execution reaches that point in the code. This can sometimes lead to unexpected behavior.

* **Can be re-declared and updated**: You can declare the same variable multiple times using `var` without any issues, which can lead to bugs and confusion in your code.

### Example:

```javascript
var x = 10;
console.log(x); // Output: 10

var x = 20;
console.log(x); // Output: 20 (var allows redeclaration)
```

## `let`

`let` was introduced in ES6 (ECMAScript 2015\) to address some of the shortcomings of `var`. It is more predictable and less error-prone.

### Characteristics of `let`:

* **Block-scoped**: `let` is scoped to the nearest enclosing block, whether it's a function, loop, or conditional. This makes it more predictable compared to `var`.

* **Hoisting**: Like `var`, `let` is hoisted, but it does not get initialized. If you try to use a `let` variable before it’s declared, you’ll get a ReferenceError.

* **Can be updated, but not re-declared**: You can change the value of a variable declared with `let`, but you cannot redeclare it within the same scope.

### Example:

```javascript
let x = 10;
console.log(x); // Output: 10

x = 20;
console.log(x); // Output: 20 (let allows reassignment)

let x = 30; // Error: Cannot redeclare block-scoped variable 'x'
```

## `const`

`const` is also introduced in ES6 and is used to declare variables whose values should remain constant (unchanged) after initialization.

### Characteristics of `const`:

* **Block-scoped**: Just like `let`, `const` is scoped to the nearest enclosing block.

* **Hoisting**: Similar to `let`, `const` is hoisted but remains uninitialized until the declaration is encountered in the code.

* **Cannot be updated or re-declared**: Once a variable is assigned with `const`, it cannot be reassigned or redeclared. However, if the value is an object or an array, its contents can be modified.

### Example:

```javascript
const x = 10;
console.log(x); // Output: 10

x = 20; // Error: Assignment to constant variable.

const y = [1, 2, 3];
y.push(4); // This is allowed because the array itself is not reassigned
console.log(y); // Output: [1, 2, 3, 4]
```

## Comparison

| Feature | `var` | `let` | `const` |
| ----- | ----- | ----- | ----- |
| **Scope** | Function-scoped | Block-scoped | Block-scoped |
| **Hoisting** | Yes (initialized with `undefined`) | Yes (temporal dead zone) | Yes (temporal dead zone) |
| **Re-declaration** | Allowed | Not allowed | Not allowed |
| **Re-assignment** | Allowed | Allowed | Not allowed |

## When to Use Each?

* **Use `var`**: Generally, avoid `var` in modern JavaScript, as `let` and `const` offer better scoping and control.

* **Use `let`**: When you need a variable whose value can change over time (like in loops or conditional statements).

* **Use `const`**: When the variable's value should not change after initialization, making your code more predictable and easier to maintain.

In summary:

* **`var`** is largely considered outdated.

* **`let`** is ideal for variables that will change during execution.

* **`const`** is perfect for values that should remain constant.

