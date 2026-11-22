---
tags:
  - FORMAT
  - JavaScript
---
# Error Handling in Async Functions

Error handling is an essential part of writing robust asynchronous code. When working with async functions and promises, errors can arise in several places, and it’s crucial to manage these errors gracefully to prevent your application from crashing or behaving unpredictably.

JavaScript provides several ways to handle errors in asynchronous code, particularly when using **async/await**. This approach uses **`try...catch`** blocks, which are similar to synchronous error handling.

---

## 1️⃣ Using `try...catch` for Error Handling

The most straightforward way to handle errors in `async` functions is by using the **`try...catch`** block. This allows you to catch both synchronous and asynchronous errors in a single location.

### How it Works:

* **`try` block**: Contains the code that may throw an error, including asynchronous code (using `await`).

* **`catch` block**: Handles any errors thrown by the code in the `try` block, whether the error is synchronous or from a rejected promise.

### Basic Syntax:

```javascript
async function myFunction() {
  try {
    const result = await someAsyncOperation();
    console.log(result);
  } catch (error) {
    console.error("Error occurred:", error);
  }
}
```

*   
  The `try` block executes the asynchronous code.

* If the promise is rejected or an error occurs during the asynchronous operation, the `catch` block will be triggered.

---

## 2️⃣ Example of Async Error Handling

Let’s take a look at an example that demonstrates error handling in an `async` function using `try...catch`.

### Example:

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    const success = false;  // Set to false to simulate an error
    
    setTimeout(() => {
      if (success) {
        resolve("Data fetched successfully!");
      } else {
        reject("Failed to fetch data");
      }
    }, 1000);
  });
}

async function fetchDataAsync() {
  try {
    const data = await fetchData();  // Await the promise
    console.log(data);  // This will not run if fetchData rejects
  } catch (error) {
    console.error("Error:", error);  // This will run if fetchData rejects
  }
}

fetchDataAsync();
// Output: "Error: Failed to fetch data"
```

In this example:

* The `fetchData` function returns a promise that may resolve or reject.

* The `fetchDataAsync` function is marked as `async`, and the `await` keyword is used to pause execution until the promise is resolved or rejected.

* If the promise is rejected, the error is caught by the `catch` block, and the error message is logged.

---

## 3️⃣ Handling Multiple Errors in Async Functions

If you have multiple asynchronous operations within a single `async` function, you can handle errors for each operation using **multiple `try...catch` blocks**.

### Example: Multiple `try...catch` Blocks:

```javascript
async function processData() {
  try {
    const data1 = await fetchData1();
    console.log(data1);
  } catch (error) {
    console.error("Error in fetchData1:", error);
  }

  try {
    const data2 = await fetchData2();
    console.log(data2);
  } catch (error) {
    console.error("Error in fetchData2:", error);
  }
}

async function fetchData1() {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject("Data 1 fetch failed"), 1000);
  });
}

async function fetchData2() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("Data 2 fetched successfully"), 2000);
  });
}

processData();
// Output:
// Error in fetchData1: Data 1 fetch failed
// Data 2 fetched successfully
```

In this example:

* We have two asynchronous functions, `fetchData1()` and `fetchData2()`.

* Each function has its own `try...catch` block, allowing us to handle errors independently for each operation.

This method is useful when different asynchronous operations may fail in different ways, and you want to handle errors for each separately.

---

## 4️⃣ Propagating Errors with `throw`

In some cases, you may want to propagate an error further up the call stack instead of handling it immediately. You can throw the error from inside a `catch` block or from the `try` block.

### Example: Throwing Errors to Propagate

```javascript
async function fetchData() {
  const success = false;
  
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (success) {
        resolve("Data fetched");
      } else {
        reject("Failed to fetch data");
      }
    }, 1000);
  });
}

async function processData() {
  try {
    const result = await fetchData();
    console.log(result);
  } catch (error) {
    console.error("Caught error:", error);
    throw new Error("Error passed up");  // Propagate error to caller
  }
}

async function main() {
  try {
    await processData();  // Propagated error will be caught here
  } catch (error) {
    console.error("Final error handler:", error);
  }
}

main();
// Output:
// Caught error: Failed to fetch data
// Final error handler: Error: Error passed up
```

In this example:

* The error in `fetchData` is caught in the `processData` function’s `catch` block.

* Instead of handling it locally, the error is re-thrown using `throw new Error("Error passed up")`.

* The error is then propagated and caught by the `main` function’s `catch` block.

---

## 5️⃣ Async/Await with `Promise.all` and Error Handling

When dealing with multiple promises using `Promise.all()`, if one of the promises rejects, the whole `Promise.all()` rejects. Using `async/await`, you can handle this scenario as well.

### Example: Handling Errors in `Promise.all`

```javascript
async function fetchData1() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("Data 1 fetched"), 1000);
  });
}

async function fetchData2() {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject("Error fetching Data 2"), 2000);
  });
}

async function fetchDataAsync() {
  try {
    const [data1, data2] = await Promise.all([fetchData1(), fetchData2()]);
    console.log(data1, data2);
  } catch (error) {
    console.error("Error in fetching data:", error);  // Handle error from any promise
  }
}

fetchDataAsync();
// Output: Error in fetching data: Error fetching Data 2
```

In this example:

* `fetchData1()` resolves successfully, while `fetchData2()` is rejected.

* `Promise.all()` rejects as soon as one of its promises rejects, and the error is caught in the `catch` block.

---

## 6️⃣ Conclusion

Error handling in `async` functions is crucial for building reliable and robust applications. The key to handling errors effectively in `async/await` is using **`try...catch`** blocks. You can catch errors for asynchronous code, propagate errors, and handle multiple promises in parallel using tools like `Promise.all()`.

### Key Points:

* **`try...catch`**: Use it to handle both synchronous and asynchronous errors.

* **`throw`**: You can throw errors from inside `catch` or `try` to propagate them further up.

* **Multiple Errors**: You can handle errors for individual asynchronous operations with multiple `try...catch` blocks.

* **Parallel Promises**: When using `Promise.all()`, be mindful of how errors in any of the promises affect the whole process.

By handling errors in `async/await`, you ensure that your asynchronous operations are managed predictably and with the ability to recover gracefully from unexpected situations.

