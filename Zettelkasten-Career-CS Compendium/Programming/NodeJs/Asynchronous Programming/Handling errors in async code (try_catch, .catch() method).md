---
tags:
  - FORMAT
  - NodeJs
---
Absolutely. Handling errors properly in asynchronous code is critical to building stable Node.js applications. Let's break it down by the type of async structure you're using: **callbacks**, **promises**, and **async/await**.

---

# Error Handling in Callbacks

In traditional callback-based code, errors are typically passed as the **first argument** to the callback.

## Example:

```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log('File data:', data);
});
```

**Rule of thumb**: Always check `err` before using the result.

---

# Error Handling in Promises

With promises, you use `.catch()` to handle any errors that occur in the promise chain.

## Example:

```javascript
const fs = require('fs').promises;

fs.readFile('file.txt', 'utf8')
  .then(data => {
    console.log('File data:', data);
  })
  .catch(err => {
    console.error('Error reading file:', err);
  });
```

If any `.then()` fails or throws, `.catch()` will handle it.

---

# Error Handling in Async/Await

When using `async/await`, errors can be caught using a `try/catch` block just like synchronous code.

## Example:

```javascript
const fs = require('fs').promises;

async function readFile() {
  try {
    const data = await fs.readFile('file.txt', 'utf8');
    console.log('File data:', data);
  } catch (err) {
    console.error('Error reading file:', err);
  }
}

readFile();
```

---

# When to Use What

| Style | Error Handling Method | Notes |
| ----- | ----- | ----- |
| Callback | Check `err` in callback | Manual and easy to miss if forgotten |
| Promise | `.catch()` | Centralized error catching |
| Async/Await | `try/catch` | Cleaner and more like synchronous code |

---

# Bonus: Catching Unhandled Rejections

Always include a global handler for unhandled promise rejections in production:

```javascript
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason);
  // optionally shut down process
});
```

---

Let me know if you want to see a nested example using both `try/catch` and `.catch()` in more complex workflows.

