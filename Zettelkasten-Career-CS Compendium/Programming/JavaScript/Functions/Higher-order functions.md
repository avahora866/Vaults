---
tags:
  - FORMAT
  - JavaScript
---
# Higher-Order Functions in JavaScript

A **higher-order function (HOF)** is a function that either:

1. **Takes another function as an argument** (callback function).

2. **Returns a function as its result**.

Higher-order functions enable functional programming techniques, making code more modular and reusable.

---

## Functions as Arguments (Callbacks)

Since JavaScript treats functions as **first-class citizens** (meaning functions can be assigned to variables, passed as arguments, and returned), we can pass functions as arguments to other functions.

### Example: Using a Callback Function

```javascript
function greet(name, callback) {
  console.log("Hello, " + name);
  callback();
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet("Alice", sayGoodbye);
// Output:
// Hello, Alice
// Goodbye!
```

🔹 Here, `greet` is a **higher-order function** because it takes another function (`sayGoodbye`) as a parameter.

---

## Functions That Return Functions

A higher-order function can also **return** another function.

### Example: Returning a Function

```javascript
function multiplyBy(factor) {
  return function (number) {
    return number * factor;
  };
}

const double = multiplyBy(2);
console.log(double(5)); // Output: 10

const triple = multiplyBy(3);
console.log(triple(5)); // Output: 15
```

🔹 Here:

* `multiplyBy` is a **higher-order function** because it **returns** another function.

* `double` and `triple` are customized functions created dynamically.

---

## Common Higher-Order Functions in JavaScript

JavaScript provides several built-in higher-order functions, especially for working with arrays.

### (a) `map()` – Transforming Arrays

`map()` applies a function to each element in an array and returns a **new array**.

```javascript
const numbers = [1, 2, 3, 4];

const squared = numbers.map(num => num * num);
console.log(squared); // Output: [1, 4, 9, 16]
```

🔹 `map()` is a **higher-order function** because it takes a callback function (`num => num * num`).

---

### (b) `filter()` – Filtering Arrays

`filter()` returns a **new array** with elements that satisfy a condition.

```javascript
const ages = [15, 22, 30, 10, 40];

const adults = ages.filter(age => age >= 18);
console.log(adults); // Output: [22, 30, 40]
```

🔹 `filter()` is a **higher-order function** because it takes a callback function (`age => age >= 18`).

---

### (c) `reduce()` – Reducing Arrays

`reduce()` accumulates array values into a single result.

```javascript
const numbers = [1, 2, 3, 4];

const sum = numbers.reduce((accumulator, num) => accumulator + num, 0);
console.log(sum); // Output: 10
```

🔹 `reduce()` is a **higher-order function** because it takes a callback that processes each element.

---

## Why Use Higher-Order Functions?

✅ **Code Reusability** – Avoid repeating code by passing functions dynamically.  
 ✅ **Modularity** – Breaks complex logic into smaller, reusable parts.  
 ✅ **Improved Readability** – Makes JavaScript code more declarative and functional.

Higher-order functions are **powerful tools** for writing cleaner, more maintainable JavaScript code. 🚀

