---
tags:
  - FORMAT
  - JavaScript
---
# let, const, and var in JavaScript

In JavaScript, **let**, **const**, and **var** are used to declare variables. However, they have different scoping rules, behavior regarding reassignment, and hoisting characteristics. Understanding the differences is key to writing clean and predictable code.

---

## 1️⃣ `var` Declaration

**`var`** was the traditional way to declare variables in JavaScript before ES6 (ECMAScript 2015). It has some unique behaviors that make it different from `let` and `const`.

### Key Characteristics of `var`:

* **Function-scoped**: A variable declared with `var` is scoped to the nearest function, not the nearest block (such as loops or conditionals).

* **Hoisted**: `var` declarations are hoisted to the top of their scope, meaning the variable is available throughout the entire scope, but it’s initialized with `undefined`.

* **Can be re-assigned**: You can reassign a variable declared with `var` multiple times within the same scope.

* **Can be redeclared**: You can declare the same variable more than once within the same scope, which can lead to bugs and unexpected behavior.

### Example:

```javascript
function testVar() {
  if (true) {
    var x = 10;  // var is function-scoped, not block-scoped
  }
  console.log(x);  // Output: 10 (x is accessible outside the if block)
}
testVar();
```

In the above example, `x` is accessible outside the `if` block because `var` is function-scoped, not block-scoped.

### Hoisting with `var`

```javascript
console.log(a); // Output: undefined (hoisted, but not initialized)
var a = 5;
console.log(a); // Output: 5
```

In this case, `a` is hoisted to the top but initialized with `undefined` until the line `var a = 5;` is executed.

---

## 2️⃣ `let` Declaration

**`let`** was introduced in ES6 to solve many of the issues associated with `var`. It is now the preferred way to declare variables in modern JavaScript.

### Key Characteristics of `let`:

* **Block-scoped**: A variable declared with `let` is scoped to the nearest block (such as a loop or conditional), not the entire function.

* **Hoisted**: Like `var`, `let` is hoisted to the top of its block. However, it is not initialized until the line of code where it is declared. This creates a **temporal dead zone** (TDZ) where the variable cannot be accessed before it’s initialized.

* **Can be re-assigned**: You can reassign a variable declared with `let`.

* **Cannot be redeclared** in the same scope.

### Example:

```javascript
function testLet() {
  if (true) {
    let y = 20;  // let is block-scoped
    console.log(y);  // Output: 20
  }
  // console.log(y);  // Error: y is not defined (block-scoped, not accessible outside the block)
}
testLet();
```

In this example, `y` is block-scoped, so it’s not accessible outside the `if` block.

### Hoisting with `let`

```javascript
console.log(b);  // Error: Cannot access 'b' before initialization
let b = 10;
console.log(b);  // Output: 10 (after initialization)
```

Here, accessing `b` before it’s initialized causes a reference error due to the **temporal dead zone**.

---

## 3️⃣ `const` Declaration

**`const`** is also introduced in ES6, and it is used to declare constants—values that cannot be reassigned after their initial assignment.

### Key Characteristics of `const`:

* **Block-scoped**: Like `let`, `const` is block-scoped.

* **Hoisted**: Like `let`, `const` is hoisted but not initialized until the line of code where it is declared. It also has a **temporal dead zone**.

* **Cannot be reassigned**: Once a `const` variable is assigned a value, it cannot be reassigned to a different value.

* **Can be used with objects**: Although `const` makes a variable immutable (constant), the contents of objects and arrays assigned to a `const` variable can still be modified.

### Example:

```javascript
const z = 30;
z = 40;  // Error: Assignment to constant variable.
```

In this case, `z` is a constant and cannot be reassigned after its initial assignment.

### Object Mutation with `const`

```javascript
const person = { name: "Alice", age: 25 };
person.age = 26;  // Allowed: The contents of the object can be changed
console.log(person);  // Output: { name: "Alice", age: 26 }

person = { name: "Bob", age: 30 };  // Error: Assignment to constant variable.
```

Even though `person` is a `const`, you can modify the properties of the object. However, you cannot reassign the object itself.

---

## 4️⃣ Comparison of `let`, `const`, and `var`

| Feature | `var` | `let` | `const` |
| ----- | ----- | ----- | ----- |
| **Scope** | Function-scoped | Block-scoped | Block-scoped |
| **Hoisting** | Hoisted and initialized with `undefined` | Hoisted but uninitialized (TDZ) | Hoisted but uninitialized (TDZ) |
| **Reassignable** | Yes | Yes | No |
| **Redeclarable** | Yes | No | No |
| **Best for** | Older code, global variables | Local variables, loops, conditionals | Constants that shouldn't change |

---

## 5️⃣ Best Practices for Using `let`, `const`, and `var`

* **Prefer `let` and `const`** over `var`. In modern JavaScript, `let` and `const` are more predictable and less prone to bugs due to their block scoping and clearer rules.

* **Use `const` by default**: If a variable does not need to be reassigned, use `const`. This helps prevent accidental reassignment and makes your code more predictable.

* **Use `let` when reassignment is necessary**: If you need to reassign a variable (like in loops or for certain dynamic values), use `let`.

* **Avoid `var`**: Since `var` is function-scoped and prone to bugs, it's considered best practice to avoid it in modern JavaScript.

---

## 6️⃣ Conclusion

* **`var`** is function-scoped, hoisted, and allows redeclaration and reassignment. It's largely considered outdated in favor of `let` and `const`.

* **`let`** is block-scoped, hoisted but not initialized, and allows reassignment. It’s suitable for variables that might change their values.

* **`const`** is also block-scoped, hoisted but not initialized, and doesn’t allow reassignment. It's perfect for values that shouldn’t be changed after assignment.

By understanding these differences, you can write more robust, readable, and bug-free JavaScript code.

