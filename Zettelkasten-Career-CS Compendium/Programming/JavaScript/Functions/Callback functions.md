---
tags:
  - FORMAT
  - JavaScript
---
# Callback Functions in JavaScript

A **callback function** is a function that is **passed as an argument** to another function and is **executed later**, usually after some operation is completed.

Callbacks are widely used in **asynchronous programming**, event handling, and functional programming.

---

## Basic Syntax of a Callback Function

A function that takes another function as a parameter is called a **higher-order function**, and the function passed to it is the **callback function**.

```javascript
function higherOrderFunction(callback) {
  console.log("Before calling the callback function.");
  callback(); // Executing the callback function
  console.log("After calling the callback function.");
}

function myCallback() {
  console.log("Hello, I'm the callback function!");
}

higherOrderFunction(myCallback);
```

### Output:

```
Before calling the callback function.
Hello, I'm the callback function!
After calling the callback function.
```

🔹 Here, `myCallback` is passed as an argument to `higherOrderFunction` and executed inside it.

---

## Callback Functions with Parameters

A callback function can also receive parameters.

```javascript
function processNumber(num, callback) {
  console.log("Processing number:", num);
  callback(num);
}

function square(n) {
  console.log("Squared:", n * n);
}

processNumber(4, square);
```

### Output:

```
Processing number: 4
Squared: 16
```

🔹 The `square` function acts as a callback and is called inside `processNumber`.

---

## Asynchronous Callbacks (setTimeout Example)

Callbacks are essential in **asynchronous programming**, where operations (like API calls, timers, or file reads) take time to complete.

```javascript
console.log("Start");

setTimeout(() => {
  console.log("This runs after 2 seconds.");
}, 2000);

console.log("End");
```

### Output:

```
Start
End
This runs after 2 seconds.
```

🔹 The **callback inside `setTimeout`** executes after 2 seconds, while the rest of the code continues running.

---

## Callbacks in Event Listeners

Callbacks are commonly used in event-driven programming.

```javascript
document.querySelector("button").addEventListener("click", () => {
  console.log("Button clicked!");
});
```

🔹 The function inside `addEventListener` is a **callback function** that runs when the button is clicked.

---

## Using Callbacks in Array Methods

Several JavaScript array methods use callbacks, such as `map()`, `filter()`, and `forEach()`.

### Example: Using `map()` with a Callback

```javascript
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(num => num * 2);
console.log(doubled); // Output: [2, 4, 6, 8]
```

🔹 The arrow function `(num => num * 2)` is a **callback function** used inside `map()`.

---

## Handling Errors in Callbacks (Callback Hell)

When multiple callbacks are nested, it leads to **callback hell**, making the code hard to read.

### Example of Callback Hell (Nested Callbacks)

```javascript
function step1(callback) {
  setTimeout(() => {
    console.log("Step 1 complete");
    callback();
  }, 1000);
}

function step2(callback) {
  setTimeout(() => {
    console.log("Step 2 complete");
    callback();
  }, 1000);
}

function step3() {
  setTimeout(() => {
    console.log("Step 3 complete");
  }, 1000);
}

// Nested callbacks (callback hell)
step1(() => {
  step2(() => {
    step3();
  });
});
```

🔹 This structure becomes hard to manage.  
 ✅ **Solution**: Use **Promises** or **Async/Await** to avoid callback hell.

---

## Advantages of Callback Functions

✔ **Allows Asynchronous Execution** – Useful for API calls, event handling, and timers.  
 ✔ **Improves Code Reusability** – Functions can be passed dynamically.  
 ✔ **Functional Programming** – Used in methods like `map()`, `filter()`, and `reduce()`.

---

## 🎯 Summary

* A **callback function** is a function passed as an argument to another function.

* Callbacks help in **asynchronous programming** (e.g., `setTimeout`, API calls).

* Callbacks are used in **event handling** and **array methods** like `map()`, `filter()`, etc.

* Too many nested callbacks lead to **callback hell**, which can be avoided using **Promises** or **Async/Await**.

Callbacks are a **fundamental concept** in JavaScript, making functions more flexible and reusable\! 🚀

