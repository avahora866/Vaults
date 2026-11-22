---
tags:
  - FORMAT
  - WebDevelopment
  - Theory
---
# What is Transpilation?

**Transpilation** is the process of converting code from one programming language (or version) to another while preserving its functionality. In the JavaScript ecosystem, **transpilers** convert **modern JavaScript (ES6+) or TypeScript** into **older JavaScript (ES5)** so it can run in environments that don’t support newer features.

Think of it like **"translation"** for code:

* You write in a modern language (like TypeScript or ES6+).

* A transpiler converts it into an older, compatible version.

* The browser or runtime executes the transpiled code.

---

# When Does Transpilation Happen?

🔹 **At Build Time** – Before the code is deployed.  
 🔹 **During Development** – Some tools (e.g., Webpack) can transpile on the fly.  
 🔹 **Before Running in Older Browsers** – To ensure compatibility.

---

# Examples of Transpilation

## Transpiling ES6+ to ES5 (Using Babel)

**Input (Modern JavaScript \- ES6+):**

```javascript
const greet = (name) => `Hello, ${name}!`;
console.log(greet("Alice"));
```

**Output (Transpiled to ES5 for older browsers):**

```javascript
"use strict";

var greet = function(name) {
  return "Hello, " + name + "!";
};
console.log(greet("Alice"));
```

📌 Here, the **arrow function (`=>`)** is converted into a **regular function** for compatibility.

---

## Transpiling TypeScript to JavaScript

TypeScript adds types, which don’t exist in JavaScript. The transpiler **removes types** and converts the code to JavaScript.

**Input (TypeScript):**

```
const age: number = 25;
function sayHello(name: string): string {
  return `Hello, ${name}`;
}
```

**Output (Transpiled JavaScript):**

```javascript
"use strict";

const age = 25;
function sayHello(name) {
  return "Hello, " + name;
}
```

📌 Here, TypeScript **removes type annotations** (`: number`, `: string`) since JavaScript doesn’t support types.

---

# Common Transpilers

✅ **Babel** – Transpiles ES6+ to ES5 (for browser compatibility).  
 ✅ **TypeScript Compiler (`tsc`)** – Converts TypeScript to JavaScript.  
 ✅ **SWC (Speedy Web Compiler)** – A faster alternative to Babel.

---

# Why is Transpilation Important?

✔ **Ensures Compatibility** – Allows modern code to run in older browsers.  
 ✔ **Enables TypeScript** – Converts TypeScript to JavaScript.  
 ✔ **Supports Future Features** – Write code with the latest syntax before browsers support it.

# Other Use Cases of Transpilation

## JavaScript & TypeScript (Most Common Case)

* **Transpiling ES6+ to ES5** (for browser compatibility).

* **Converting TypeScript to JavaScript** (removing types).

* **Compiling JSX to JS** (React’s JSX to plain JavaScript).

## CoffeeScript → JavaScript

* CoffeeScript is a simplified syntax for JavaScript.

* Example: **CoffeeScript**

```
square = (x) -> x * x
```

*   
  **Transpiled JavaScript**

```javascript
var square = function(x) {
  return x * x;
};
```

## Dart → JavaScript

* Dart is a language by Google that compiles (transpiles) to JavaScript to run in browsers.

## Babel Plugins for Experimental Features

* Babel can **transpile future JavaScript features** (like ESNext proposals) to work in today's browsers.

## WebAssembly Text Format (WAT) → WebAssembly (WASM)

* Some tools transpile human-readable WAT into binary WASM for performance.

## Elm → JavaScript

* Elm, a functional language, transpiles to JavaScript for web applications.

## Python or Ruby → JavaScript

* Tools like **Transcrypt** (Python → JS) and **Opal** (Ruby → JS) allow running non-JS languages in the browser.

---

# How is Transpilation Different from Compilation?

| Feature | Transpilation | Compilation |
| ----- | ----- | ----- |
| **Input & Output** | Converts one high-level language to another (e.g., TS → JS) | Converts high-level code to low-level (e.g., C → Machine Code) |
| **Common Example** | Babel (ES6 → ES5), TypeScript | C/C++ compiled to machine code |
| **Performance Impact** | Minimal | Can include optimizations for CPU execution |

---

