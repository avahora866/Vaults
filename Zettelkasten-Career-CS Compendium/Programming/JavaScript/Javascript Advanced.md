---
tags:
  - "#SECONDARY_BRANCH"
---
#NOTES 
### 1. The Prototype Chain (The "Real" Inheritance)

You currently use `class` and `extends` because they feel comfortable (like Java). However, JavaScript does **not** have classes.

- **The Concept:** JavaScript uses **Prototypal Inheritance**. Objects inherit directly from other objects, not from "blueprints" (classes).
    
- **The Reality:** The `class` keyword is just "syntactic sugar." Under the hood, JS links objects together via a hidden property (`__proto__`). Understanding this is required to debug weird inheritance issues or to patch standard library functions (Polyfills).
    

### 2. Advanced Asynchronous Patterns

We covered Promises and Async/Await, but that is the surface level.

- **Generators (`function*` and `yield`):** Functions that can be paused in the middle of execution and resumed later. This allows you to write custom iterators or state machines.
    
- **The Microtask vs. Macrotask Queue:** Not all async tasks are equal. `Promise.then()` goes to the **Microtask Queue** (higher priority), while `setTimeout` goes to the **Macrotask Queue**. Knowing the difference explains why some code executes "out of order" even when async.
    

### 3. Metaprogramming (Magic)

Java has Reflection; JavaScript has **Proxies** and **Reflect**.

- **Proxies:** You can wrap an object in a "Proxy" to intercept and redefine fundamental operations. You can make an object that logs every time a property is read, or validates data before it's written, or calculates values on the fly. This is how modern frontend frameworks (like Vue.js) fundamentally work.
    

### 4. `this` Binding (The Explicit Control)

In Java, `this` is always the instance. In JavaScript, `this` changes based on _how_ a function is called, not where it is defined.

- **Call / Apply / Bind:** You can manually force a function to use a specific object as `this`.
    
- **Arrow Functions vs. Regular Functions:** Arrow functions (`() => {}`) do not have their own `this`; they steal it from their parent scope. Regular functions define their own `this`.
    

### 5. Multithreading (Web Workers)

I told you JS is single-threaded. That is 99% true.

- **Web Workers:** Allow you to spawn real, separate threads for heavy computation (like image processing) so the UI doesn't freeze. However, these threads **cannot** touch the DOM; they can only communicate via messages.
    

### 6. Functional Programming Patterns

JavaScript is heavily influenced by functional languages (like Scheme).

- **Currying:** Transforming a function `f(a, b, c)` into `f(a)(b)(c)`.
    
- **High-Order Functions:** Functions that create other functions (we touched on this with Closures).
    
- **Immutability:** Managing state without modifying objects (critical for Redux/React).
    

### 7. Memory Management Nuances

- **WeakMap / WeakSet:** Special data structures that hold "weak" references to objects. If the object is deleted elsewhere, it is automatically garbage collected from the Map. This is vital for preventing memory leaks in long-running applications.