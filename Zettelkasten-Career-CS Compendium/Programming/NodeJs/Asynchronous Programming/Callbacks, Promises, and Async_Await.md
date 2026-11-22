---
tags:
  - FORMAT
  - NodeJs
---
Sure. In Node.js and modern JavaScript, handling asynchronous operations (like reading a file or fetching data) is done using **callbacks**, **promises**, or **async/await**. These are different styles for managing code that runs *later*.

Let’s break down each one.

---

# Callbacks

A **callback** is a function passed as an argument to another function, to be executed after some operation completes.

## Example:

```javascript
function getData(callback) {
  setTimeout(() => {
    callback('Data received');
  }, 1000);
}

getData((result) => {
  console.log(result); // Output after 1 second: Data received
});
```

## Common Problems:

* **Callback hell**: nesting callbacks within callbacks

* **Hard to manage errors**

---

# Promises

A **promise** represents a value that will be available in the future. It can be:

* `resolved` (fulfilled)

* `rejected` (failed)

## Example:

```javascript
function getData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data received');
    }, 1000);
  });
}

getData()
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```

## Advantages over Callbacks:

* Chaining with `.then()`

* Centralized error handling with `.catch()`

* Cleaner and more readable than nested callbacks

---

# Async/Await

**Async/await** is syntax sugar over promises that makes async code look synchronous.

## Example:

```javascript
async function fetchData() {
  try {
    const result = await getData(); // waits for the promise to resolve
    console.log(result);
  } catch (err) {
    console.error(err);
  }
}

fetchData();
```

## Benefits:

* Easier to read and write

* Uses `try/catch` for error handling

* Best suited for modern JavaScript

---

# Comparison Table

| Feature | Callbacks | Promises | Async/Await |
| ----- | ----- | ----- | ----- |
| Syntax | Function nesting | `.then()` / `.catch()` | `await` inside `async` |
| Error Handling | Manual | `.catch()` | `try/catch` |
| Readability | Harder (callback hell) | Better | Best |
| Chaining | Manual nesting | Yes | With multiple `await`s |

---

# Practical Use Case (fs module)

**Callback style:**

```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

**Promise \+ Async/Await:**

```javascript
const fs = require('fs').promises;

async function readFile() {
  try {
    const data = await fs.readFile('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}

readFile();
```

---

Let me know if you want to convert some callback-based code into promises or async/await style.

