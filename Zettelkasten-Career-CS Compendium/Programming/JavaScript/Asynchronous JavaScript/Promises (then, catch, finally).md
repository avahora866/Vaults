---
tags:
  - FORMAT
  - JavaScript
---
# Promises in JavaScript (then, catch, finally)

In JavaScript, **Promises** are used to handle asynchronous operations more effectively. A **promise** represents a value that may be available now, or in the future, or never (in case of an error). Promises allow you to manage asynchronous code in a cleaner, more readable way than callbacks.

---

## 1️⃣ What is a Promise?

A **Promise** is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. It can be in one of three states:

* **Pending**: The initial state; the promise is still being processed.

* **Fulfilled**: The operation completed successfully, and the promise has a result value.

* **Rejected**: The operation failed, and the promise has an error.

### Basic Promise Syntax:

```javascript
const promise = new Promise((resolve, reject) => {
  // Asynchronous operation
  if (/* success condition */) {
    resolve("Success!");  // Fulfills the promise
  } else {
    reject("Error!");  // Rejects the promise
  }
});
```

In this example, `resolve()` is called if the asynchronous operation succeeds, and `reject()` is called if it fails. The promise is created using the `new Promise()` constructor, which takes a function with two arguments: `resolve` and `reject`.

---

## 2️⃣ Using Promises (then, catch, finally)

After a promise is created, it can be handled with the following methods: **`then`**, **`catch`**, and **`finally`**.

### `then()` Method

The **`then()`** method is used to handle the successful resolution of a promise. It takes one or two callback functions:

* The first callback is executed when the promise is **fulfilled** (resolved).

* The second callback (optional) is executed when the promise is **rejected**.

#### Syntax:

```javascript
promise.then(onFulfilled, onRejected);
```

*   
  **`onFulfilled`**: A function that is called if the promise is resolved.

* **`onRejected`**: A function that is called if the promise is rejected (optional in some cases).

#### Example:

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;
  
  if (success) {
    resolve("Operation Successful!");
  } else {
    reject("Something went wrong.");
  }
});

promise.then(
  (result) => { console.log(result); },  // onFulfilled
  (error) => { console.error(error); }   // onRejected
);
// Output: "Operation Successful!"
```

Here, the `then()` method is used to handle both success and failure cases.

### `catch()` Method

The **`catch()`** method is used to handle **rejected promises**. It is essentially a shorthand for handling errors that would otherwise be passed to the second parameter of `then()`.

#### Syntax:

```javascript
promise.catch(onRejected);
```

*   
  **`onRejected`**: A function that is called if the promise is rejected.

#### Example:

```javascript
const promise = new Promise((resolve, reject) => {
  const success = false;

  if (success) {
    resolve("Operation Successful!");
  } else {
    reject("Something went wrong.");
  }
});

promise
  .then(result => { console.log(result); })
  .catch(error => { console.error(error); });
// Output: "Something went wrong."
```

In this case, the promise is rejected, and the error is caught by `catch()`. The output is "Something went wrong."

### `finally()` Method

The **`finally()`** method is used to specify code that will be executed **after the promise has been settled**, regardless of whether it was fulfilled or rejected. It's useful for cleanup tasks, such as hiding a loading spinner or resetting variables, regardless of the outcome.

#### Syntax:

```javascript
promise.finally(() => {
  // Code to be executed after the promise is settled (resolved or rejected)
});
```

#### Example:

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;
  
  if (success) {
    resolve("Operation Successful!");
  } else {
    reject("Something went wrong.");
  }
});

promise
  .then(result => { console.log(result); })
  .catch(error => { console.error(error); })
  .finally(() => { console.log("This is executed regardless of success or failure."); });
// Output:
// Operation Successful!
// This is executed regardless of success or failure.
```

In this example, `finally()` is used to log a message after the promise has either been resolved or rejected, without affecting the result.

---

## 3️⃣ Chaining Promises

One of the most powerful features of promises is the ability to chain multiple asynchronous operations together. Using `then()`, you can chain promises in a readable way, allowing you to handle multiple steps of an asynchronous workflow.

### Example of Chaining:

```javascript
const fetchData = new Promise((resolve, reject) => {
  const data = { name: "Alice", age: 25 };
  resolve(data);
});

fetchData
  .then(data => {
    console.log("Data received:", data);
    return data.age;  // Pass data to the next step in the chain
  })
  .then(age => {
    console.log("Age:", age);
    return age + 5;  // Pass data to the next step in the chain
  })
  .then(updatedAge => {
    console.log("Updated age:", updatedAge);
  })
  .catch(error => {
    console.error("Error:", error);
  })
  .finally(() => {
    console.log("All operations are completed.");
  });
// Output:
// Data received: { name: "Alice", age: 25 }
// Age: 25
// Updated age: 30
// All operations are completed.
```

In this case, the data flows through each `.then()` callback, and `catch()` handles any potential errors. The `finally()` block is executed once all the promises have been settled.

---

## 4️⃣ Error Handling with Promises

Error handling with promises is simplified compared to nested callbacks. Instead of dealing with multiple levels of error handling, you can handle errors in a single `.catch()` method.

### Example:

```javascript
const fetchData = new Promise((resolve, reject) => {
  const success = false;
  
  if (success) {
    resolve("Data loaded successfully");
  } else {
    reject("Failed to load data");
  }
});

fetchData
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error); // Output: "Failed to load data"
  })
  .finally(() => {
    console.log("Operation finished.");
  });
// Output:
// Failed to load data
// Operation finished.
```

If the promise is rejected, the error is caught in the `catch()` block.

---

## 5️⃣ Promise.all() and Promise.race()

In addition to `then()`, `catch()`, and `finally()`, there are two other useful methods for handling multiple promises:

### `Promise.all()`

The **`Promise.all()`** method takes an array of promises and returns a single promise that resolves when all the promises in the array are resolved. If any of the promises are rejected, `Promise.all()` immediately rejects.

#### Example:

```javascript
const promise1 = new Promise(resolve => resolve("First promise"));
const promise2 = new Promise(resolve => resolve("Second promise"));
const promise3 = new Promise(resolve => resolve("Third promise"));

Promise.all([promise1, promise2, promise3])
  .then(results => {
    console.log(results); // Output: ["First promise", "Second promise", "Third promise"]
  })
  .catch(error => {
    console.error(error);
  });
```

### `Promise.race()`

The **`Promise.race()`** method returns a promise that resolves or rejects as soon as one of the promises in the array resolves or rejects.

#### Example:

```javascript
const promise1 = new Promise(resolve => setTimeout(resolve, 1000, "First promise"));
const promise2 = new Promise(resolve => setTimeout(resolve, 2000, "Second promise"));

Promise.race([promise1, promise2])
  .then(result => {
    console.log(result); // Output: "First promise"
  })
  .catch(error => {
    console.error(error);
  });
```

In this case, `Promise.race()` resolves with the result of the first promise that completes.

---

## 6️⃣ Conclusion

Promises are a powerful tool in JavaScript for handling asynchronous operations. The main methods associated with promises are:

* **`then()`**: Handles the successful resolution of a promise.

* **`catch()`**: Handles the rejection of a promise (errors).

* **`finally()`**: Executes code after the promise is settled (regardless of success or failure).

Promises allow for cleaner, more readable asynchronous code and make error handling easier. Additionally, methods like **`Promise.all()`** and **`Promise.race()`** help in managing multiple promises concurrently.

