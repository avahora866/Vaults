---
tags:
  - FORMAT
  - JavaScript
---
# Lexical Scope in JavaScript

### What is Lexical Scope?

**Lexical scope** (also called **static scope**) means that a function **can access variables from its outer scope**, even after the outer function has finished executing.

🔹 In JavaScript, scope is **determined at the time of writing the code**, not at runtime.  
 🔹 **Inner functions inherit variables** from their parent functions (but not the other way around).

---

## How Lexical Scope Works

When a function is defined inside another function, it has access to: ✅ **Its own variables**  
 ✅ **Variables from its parent function**  
 ✅ **Global variables**

### Example: Basic Lexical Scope

```javascript
function outerFunction() {
  let outerVar = "I'm from the outer function";

  function innerFunction() {
    console.log(outerVar); // ✅ Can access outer function's variable
  }

  innerFunction();
}

outerFunction();
```

### Output:

```
I'm from the outer function
```

🔹 `innerFunction()` has access to `outerVar` **because of lexical scope**.  
 🔹 If `outerVar` was declared in the global scope, `innerFunction` could still access it.

---

## Lexical Scope with Multiple Nested Functions

Functions can **access variables from all outer levels**.

```javascript
function grandparent() {
  let grandparentVar = "Grandparent's variable";

  function parent() {
    let parentVar = "Parent's variable";

    function child() {
      console.log(grandparentVar); // ✅ Can access grandparent's variable
      console.log(parentVar); // ✅ Can access parent's variable
    }

    child();
  }

  parent();
}

grandparent();
```

### Output:

```
Grandparent's variable
Parent's variable
```

🔹 The `child()` function can access both `parentVar` and `grandparentVar` due to **lexical scope**.

---

## Lexical Scope Does Not Work in Reverse

An **outer function** **cannot** access variables declared in an **inner function**.

```javascript
function outer() {
  function inner() {
    let innerVar = "I'm inside inner";
  }

  console.log(innerVar); // ❌ Error: innerVar is not defined
}

outer();
```

### Error:

```
Uncaught ReferenceError: innerVar is not defined
```

🔹 `innerVar` is **not accessible** from `outer()` because **scope only flows downward**.

---

## Lexical Scope in Closures

Lexical scope is the foundation of **closures**, where an inner function **remembers** variables from its parent scope even after the parent function has finished executing.

### Example: Closure with Lexical Scope

```javascript
function outerFunction() {
  let count = 0;

  return function innerFunction() {
    count++;
    console.log("Count:", count);
  };
}

const counter = outerFunction();
counter(); // Output: Count: 1
counter(); // Output: Count: 2
counter(); // Output: Count: 3
```

🔹 Even though `outerFunction` has finished executing, `innerFunction` **still remembers `count`** due to **lexical scope**.

---

## Summary

| Concept | Explanation |
| ----- | ----- |
| **Lexical Scope** | Functions **inherit variables** from their parent scope. |
| **Scope Flow** | **Inner functions** can access outer variables, but **outer functions cannot** access inner variables. |
| **Closures** | A function retains access to its **lexical scope** even after the parent function has returned. |

✅ **Best Practices:**

* Use lexical scope to **organize code efficiently**.

* Combine **lexical scope with closures** to **preserve data** across function calls.

Lexical scope is a **core concept** in JavaScript that helps in writing **cleaner and more powerful** code\! 🚀

