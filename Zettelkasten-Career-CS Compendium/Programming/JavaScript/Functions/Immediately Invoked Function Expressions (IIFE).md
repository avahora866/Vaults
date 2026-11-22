---
tags:
  - FORMAT
  - JavaScript
---
# Immediately Invoked Function Expressions (IIFE) in JavaScript

An **Immediately Invoked Function Expression (IIFE)** is a **function that runs immediately after being defined**. It is a JavaScript design pattern used to create a private scope and avoid polluting the global namespace.

---

## Basic Syntax of an IIFE

An IIFE is a function wrapped in parentheses and **executed immediately**.

```javascript
(function() {
  console.log("IIFE executed!");
})();
```

### Output:

```
IIFE executed!
```

🔹 The `()` at the end **immediately invokes** the function.  
 🔹 The function is **wrapped in parentheses** `(function() { ... })` to turn it into an **expression**.

---

## Arrow Function IIFE

IIFEs can also be written using **arrow functions**:

```javascript
(() => {
  console.log("Arrow function IIFE executed!");
})();
```

### Output:

```
Arrow function IIFE executed!
```

---

## IIFE with Parameters

You can pass arguments to an IIFE:

```javascript
(function(name) {
  console.log("Hello, " + name);
})("Alice");
```

### Output:

```
Hello, Alice
```

🔹 The function receives `"Alice"` as an argument.

---

## IIFE with Return Values

An IIFE can **return a value**:

```javascript
const result = (function() {
  return "IIFE returned this value!";
})();
console.log(result);
```

### Output:

```
IIFE returned this value!
```

🔹 The returned value is stored in `result`.

---

## Why Use IIFE?

### ✅ Avoids Global Scope Pollution

Variables inside an IIFE are **not accessible outside**, preventing unintended modifications.

```javascript
(function() {
  let secret = "This is private!";
  console.log(secret);
})();

// console.log(secret); ❌ Error: secret is not defined
```

🔹 `secret` is **private** and **cannot be accessed** outside.

---

### ✅ Useful in Asynchronous Code

IIFEs are often used in asynchronous functions like `setTimeout`:

```javascript
for (var i = 1; i <= 3; i++) {
  (function(i) {
    setTimeout(() => {
      console.log(i);
    }, 1000);
  })(i);
}
```

### Output (after 1 sec):

```
1
2
3
```

🔹 Each loop iteration creates a **new scope**, preserving the correct value of `i`.

---

## 🎯 Summary

| Feature | Description |
| ----- | ----- |
| **Definition** | A function that runs immediately after being defined. |
| **Syntax** | `(function() { ... })();` |
| **Arrow IIFE** | `(() => { ... })();` |
| **Scopes Variables** | Prevents pollution of the global namespace. |
| **Supports Parameters** | `(function(arg) { ... })(value);` |
| **Use Cases** | Private variables, async functions, avoiding global conflicts. |

IIFEs are a powerful way to create **isolated scopes** and execute code immediately\! 🚀

