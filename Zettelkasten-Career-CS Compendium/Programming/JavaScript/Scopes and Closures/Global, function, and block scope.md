---
tags:
  - FORMAT
  - JavaScript
---
# Global, Function, and Block Scope in JavaScript

**Scope** in JavaScript refers to the visibility and accessibility of variables in different parts of a program. There are three main types of scope:

1. **Global Scope**

2. **Function Scope**

3. **Block Scope**

---

## Global Scope

A **global variable** is declared **outside** of any function or block and can be accessed from **anywhere** in the program.

### Example: Global Scope

```javascript
let globalVar = "I am global";

function showGlobal() {
  console.log(globalVar); // Accessible inside function
}

showGlobal(); 
console.log(globalVar); // Accessible outside function
```

### Output:

```
I am global
I am global
```

🔹 **Global variables** remain in memory throughout the execution of the script.  
 🔹 Too many global variables **can cause conflicts** and **make debugging difficult**.

---

## Function Scope

A variable declared **inside a function** using `var`, `let`, or `const` **is accessible only within that function**.

### Example: Function Scope

```javascript
function myFunction() {
  let functionVar = "I exist only in this function";
  console.log(functionVar); // ✅ Accessible here
}

myFunction();
console.log(functionVar); // ❌ Error: functionVar is not defined
```

### Output:

```
I exist only in this function
Uncaught ReferenceError: functionVar is not defined
```

🔹 **Function scope** ensures that variables **do not interfere** with other parts of the code.  
 🔹 Variables declared with **`var` are function-scoped**, meaning they **do not obey block scope**.

---

## Block Scope (`let` & `const`)

A block is any code wrapped in **curly braces `{}`**, such as loops, conditionals, and functions. Variables declared with `let` and `const` inside a block **are only accessible within that block**.

### Example: Block Scope

```javascript
if (true) {
  let blockVar = "I am inside a block";
  console.log(blockVar); // ✅ Accessible inside
}

console.log(blockVar); // ❌ Error: blockVar is not defined
```

### Output:

```
I am inside a block
Uncaught ReferenceError: blockVar is not defined
```

🔹 `let` and `const` **do not exist outside the block** where they are declared.  
 🔹 `var`, on the other hand, **ignores block scope**.

### Difference Between `var`, `let`, and `const` in Scope

```javascript
if (true) {
  var x = 10;  // Function-scoped (ignores block scope)
  let y = 20;  // Block-scoped
  const z = 30; // Block-scoped
}

console.log(x); // ✅ Accessible (var is function-scoped)
console.log(y); // ❌ Error: y is not defined
console.log(z); // ❌ Error: z is not defined
```

---

## Nested Scopes & Lexical Scope

JavaScript has **lexical scope**, meaning **inner functions can access variables from outer functions**.

### Example: Lexical Scope

```javascript
function outerFunction() {
  let outerVar = "I'm from the outer function";

  function innerFunction() {
    console.log(outerVar); // ✅ Accessible (lexical scope)
  }

  innerFunction();
}

outerFunction();
```

### Output:

```
I'm from the outer function
```

🔹 **Inner functions inherit variables from outer functions**, but the reverse is **not** true.

---

## Summary Table

| Scope Type | Where Defined | Accessibility |
| ----- | ----- | ----- |
| **Global Scope** | Outside functions/blocks | Accessible **anywhere** in the script |
| **Function Scope** | Inside a function | Accessible **only inside** that function |
| **Block Scope** | Inside `{}` (if, loops, etc.) | Accessible **only inside** that block (`let`, `const`) |

✅ **Best Practices:**

* Use **`let` and `const`** to prevent accidental global variables.

* Minimize global variables to avoid **naming conflicts** and **bugs**.

* Understand **lexical scope** to work with closures and nested functions.

Mastering scope helps in **writing clean, bug-free JavaScript code\!** 🚀

