---
tags:
  - FORMAT
  - JavaScript
---
# Closures in JavaScript

### What is a Closure?

A **closure** is a function that **remembers variables** from its **lexical scope**, even after the outer function has finished executing.

🔹 **Closures allow inner functions to access outer function variables, even after the outer function has returned.**  
 🔹 This is possible because JavaScript **does not destroy** the outer function's variables if an inner function is still using them.

---

## Basic Closure Example

```javascript
function outerFunction() {
  let message = "Hello from outer function";

  function innerFunction() {
    console.log(message); // ✅ Can access 'message' due to closure
  }

  return innerFunction;
}

const myClosure = outerFunction();
myClosure(); // Output: Hello from outer function
```

### How It Works?

1. `outerFunction()` is called → `message` is created in its scope.

2. `innerFunction` is returned but **not executed yet**.

3. `myClosure` stores `innerFunction`, which **still remembers `message`**.

4. When `myClosure()` runs, it **still has access** to `message` from `outerFunction`.

---

## Practical Use Cases of Closures

### 1️⃣ Data Privacy & Encapsulation

Closures help create **private variables** that **cannot be modified from outside**.

#### Example: Creating a Counter

```javascript
function createCounter() {
  let count = 0; // Private variable

  return {
    increment: function() {
      count++;
      console.log("Count:", count);
    },
    decrement: function() {
      count--;
      console.log("Count:", count);
    },
    getCount: function() {
      return count;
    }
  };
}

const counter = createCounter();
counter.increment(); // Output: Count: 1
counter.increment(); // Output: Count: 2
counter.decrement(); // Output: Count: 1
console.log(counter.getCount()); // Output: 1
```

🔹 The `count` variable **is private** and can only be modified using `increment`, `decrement`, or `getCount`.  
 🔹 This is useful for creating **secure, encapsulated logic**.

---

### 2️⃣ Function Factories

Closures can **generate multiple functions with different behaviors**.

#### Example: Creating Custom Greetings

```javascript
function createGreeting(greeting) {
  return function(name) {
    console.log(`${greeting}, ${name}!`);
  };
}

const sayHello = createGreeting("Hello");
const sayGoodbye = createGreeting("Goodbye");

sayHello("Alice"); // Output: Hello, Alice!
sayGoodbye("Bob"); // Output: Goodbye, Bob!
```

🔹 `sayHello` and `sayGoodbye` **remember** the `greeting` variable even after `createGreeting` has finished.

---

### 3️⃣ Maintaining State in Event Listeners

Closures can help **retain state** in event listeners.

#### Example: Click Counter

```javascript
function setupButton() {
  let clickCount = 0;

  document.getElementById("myButton").addEventListener("click", function() {
    clickCount++;
    console.log("Button clicked", clickCount, "times");
  });
}

setupButton();
```

🔹 Even though `setupButton` **executes once**, the event listener **retains access** to `clickCount` due to closures.

---

### 4️⃣ Delaying Execution (setTimeout)

Closures are useful when **preserving values inside setTimeout()**.

#### Example: Using `setTimeout` in a Loop

```javascript
for (let i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log("Number:", i);
  }, i * 1000);
}
```

### Output (after delays):

```
Number: 1
Number: 2
Number: 3
```

🔹 The function inside `setTimeout` **remembers `i`** because of **closures**.

---

### 5️⃣ Implementing Memoization (Performance Optimization)

Closures can store **previously computed results** to avoid redundant calculations.

#### Example: Memoized Fibonacci Function

```javascript
function memoizedFibonacci() {
  let cache = {};

  return function fib(n) {
    if (n in cache) return cache[n];
    if (n <= 1) return n;

    cache[n] = fib(n - 1) + fib(n - 2);
    return cache[n];
  };
}

const fibonacci = memoizedFibonacci();
console.log(fibonacci(10)); // Output: 55
console.log(fibonacci(10)); // Instant result from cache
```

🔹 The closure **stores computed Fibonacci numbers** in `cache` for **faster subsequent calculations**.

---

## Summary

| Use Case | Description |
| ----- | ----- |
| **Data Privacy** | Prevents direct access to variables, useful for secure code. |
| **Function Factories** | Creates dynamic, reusable functions. |
| **Event Listeners** | Retains state between event handler executions. |
| **Delayed Execution** | Preserves values in `setTimeout` or asynchronous operations. |
| **Performance Optimization** | Memoization and caching techniques for efficient execution. |

### ✅ Key Takeaways

* A **closure retains access** to its **outer function’s variables** even after the function has executed.

* Used for **data privacy, function factories, event listeners, memoization, and managing state**.

* **Closures help write cleaner, more efficient JavaScript code**.

Closures are one of JavaScript’s **most powerful features**\! 🚀

