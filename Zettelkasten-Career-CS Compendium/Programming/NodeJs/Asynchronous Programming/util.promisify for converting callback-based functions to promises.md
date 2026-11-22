---
tags:
  - FORMAT
  - NodeJs
---
Yes\! `util.promisify` is a built-in function in Node.js that converts **callback-based functions** (typically using the Node.js error-first style) into **promise-based** ones.

This is super useful when working with older Node.js APIs (like `fs`, `crypto`, etc.) that still use callbacks — you can modernize them for use with `.then()` or `async/await`.

---

# Basic Syntax

```javascript
const { promisify } = require('util');

const promisedFunction = promisify(originalCallbackFunction);
```

*   
  `originalCallbackFunction` must follow the Node.js convention:  
   `(err, result) => {}`

---

# Example with `fs.readFile`

## Callback version:

```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

## Using `util.promisify`:

```javascript
const fs = require('fs');
const { promisify } = require('util');

const readFileAsync = promisify(fs.readFile);

readFileAsync('file.txt', 'utf8')
  .then(data => console.log(data))
  .catch(err => console.error('Error:', err));
```

Or with `async/await`:

```javascript
async function readFile() {
  try {
    const data = await readFileAsync('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error('Error:', err);
  }
}

readFile();
```

---

# When Should You Use `promisify`?

* You want to use `async/await` with legacy callback APIs

* You're refactoring old code

* You're integrating libraries that don't offer a promise version

---

# Notes

* Works only for **error-first** callbacks (e.g., `(err, result)`)

* Doesn't handle callbacks with **multiple results** unless used with custom wrappers

---

# Bonus: Multiple results with `util.promisify.custom`

If the callback returns multiple results (not just a single value), you'd need to manually wrap the function to support that. But for most standard Node.js modules, `promisify` works out of the box.

---

Let me know if you want to see a custom wrapper for multi-value callbacks or help refactoring a callback-based function to use `promisify`.

