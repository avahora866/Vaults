---
tags:
  - FORMAT
  - JavaScript
---
# Async/Await in JavaScript

**Async/Await** is a feature introduced in **ES8 (ES2017)** that allows you to write asynchronous code in a more readable and synchronous-like manner. It is built on top of **Promises** and provides a simpler and cleaner way to handle asynchronous operations. With `async` and `await`, you can avoid the complexity and "callback hell" associated with callbacks and promise chains.

---

## 1️⃣ What is Async/Await?

* **`async`**: The `async` keyword is used to define a function as asynchronous. An asynchronous function always returns a promise, and if it doesn't explicitly return a value, it will return a resolved promise with an undefined value.

* **`await`**: The `await` keyword is used inside an `async` function to pause the execution of the function until the promise is resolved (or rejected). It makes asynchronous code look and behave more like synchronous code.

### Basic Syntax:

```javascript
async function myFunction() {
  // Code to execute
}

const result = await somePromise;
```

*   
  `async` is placed before the function declaration.

* `await` is used to pause the function execution until the promise is resolved.

---

## 2️⃣ How Async/Await Works

When using `async` and `await`, the code looks synchronous, but it is still asynchronous under the hood. The key difference is that `await` pauses the function execution until the promise is resolved, avoiding the need for chaining `.then()` or using callbacks.

### Async Function:

* The function declared with `async` always returns a **Promise**.

* Inside the function, you can use `await` to wait for a promise to resolve before continuing the function execution.

### Await:

* The `await` expression is used to pause the execution of the async function.

* It waits for the promise to resolve and then returns the result of that promise.

* If the promise is rejected, it throws an error (which can be caught using `try/catch`).

---

## 3️⃣ Example of Async/Await

Let’s start with a simple example to understand the concept.

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Data fetched!");
    }, 2000);
  });
}

async function fetchDataAsync() {
  const result = await fetchData();  // Pause execution until fetchData is resolved
  console.log(result);  // "Data fetched!" after 2 seconds
}

fetchDataAsync();
```

In this example:

* `fetchData` is a function that returns a promise which resolves after 2 seconds.

* The `fetchDataAsync` function is declared as `async`, and within it, we use `await` to pause the execution until the promise returned by `fetchData` is resolved.

* Once resolved, the result is logged to the console.

---

## 4️⃣ Handling Errors with Async/Await

Since `await` is essentially waiting for a promise to resolve, errors can occur if the promise is rejected. To handle these errors, you can use a **`try...catch`** block, which is similar to synchronous error handling.

### Example with Error Handling:

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    const success = false;
    
    setTimeout(() => {
      if (success) {
        resolve("Data fetched successfully!");
      } else {
        reject("Failed to fetch data");
      }
    }, 2000);
  });
}

async function fetchDataAsync() {
  try {
    const result = await fetchData();
    console.log(result);  // Only if the promise is resolved
  } catch (error) {
    console.error(error);  // Catch any error if the promise is rejected
  }
}

fetchDataAsync();
```

In this example:

* If the promise is rejected, the `catch` block will handle the error and log the message `"Failed to fetch data"`.

* The `try...catch` mechanism makes it easier to handle errors compared to chaining `.catch()` with promises.

---

## 5️⃣ Async/Await and Multiple Promises

When dealing with multiple asynchronous operations, `async/await` can help you manage them in a linear, easy-to-read manner.

### Example with Multiple Promises:

```javascript
function fetchData1() {
  return new Promise(resolve => {
    setTimeout(() => resolve("Data 1 fetched"), 1000);
  });
}

function fetchData2() {
  return new Promise(resolve => {
    setTimeout(() => resolve("Data 2 fetched"), 1500);
  });
}

async function fetchMultipleData() {
  const data1 = await fetchData1();
  console.log(data1);  // Output: "Data 1 fetched"
  
  const data2 = await fetchData2();
  console.log(data2);  // Output: "Data 2 fetched"
}

fetchMultipleData();
```

In this example:

* Each asynchronous operation (`fetchData1` and `fetchData2`) is awaited in sequence.

* The second data fetch will wait until the first one is completed.

---

## 6️⃣ Parallel Execution with Async/Await

If you want to run asynchronous operations **in parallel** (instead of sequentially), you can use `Promise.all()` with `async/await`.

### Example with Parallel Execution:

```javascript
async function fetchMultipleDataInParallel() {
  const [data1, data2] = await Promise.all([fetchData1(), fetchData2()]);
  console.log(data1);  // Output: "Data 1 fetched"
  console.log(data2);  // Output: "Data 2 fetched"
}

fetchMultipleDataInParallel();
```

In this example:

* `Promise.all()` is used to execute both `fetchData1()` and `fetchData2()` in parallel.

* `await Promise.all([...])` waits for all promises to resolve and returns their results as an array.

* The results are then destructured into `data1` and `data2`.

This approach is more efficient than awaiting each promise one after the other.

---

## 7️⃣ Benefits of Async/Await

### Simpler and More Readable Code

* `async/await` makes asynchronous code look and behave more like synchronous code, which is easier to follow.

* It eliminates the need for promise chaining (`.then()` and `.catch()`), reducing the complexity of the code.

### Error Handling with Try/Catch

* With `async/await`, error handling is cleaner and more natural using `try...catch`, similar to handling errors in synchronous code.

### Avoid Callback Hell

* `async/await` avoids the deeply nested callbacks that can make code difficult to maintain and debug.

---

## 8️⃣ Conclusion

**Async/Await** provides a powerful and readable way to work with asynchronous code in JavaScript. It is built on top of Promises but allows you to write asynchronous logic in a more linear, readable, and synchronous-like manner. By using `async` functions and `await` expressions, you can manage asynchronous tasks more efficiently, especially in scenarios that involve multiple promises or require error handling.

### Summary of Key Points:

* **`async`**: Declares a function as asynchronous, and it automatically returns a promise.

* **`await`**: Pauses the execution of the `async` function until the promise is resolved.

* **`try...catch`**: Used for handling errors in asynchronous code.

* **`Promise.all()`**: Useful for executing multiple promises in parallel.

Async/Await simplifies asynchronous code and is a modern way of dealing with tasks like HTTP requests, file reading, or any other asynchronous operation.

