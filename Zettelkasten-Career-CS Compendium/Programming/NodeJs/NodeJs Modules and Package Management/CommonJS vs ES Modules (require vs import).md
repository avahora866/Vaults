---
tags:
  - FORMAT
  - NodeJs
---
Sure\! Let's dive into the differences between **CommonJS** and **ES Modules** (ESM) in JavaScript, focusing on the key differences between `require` (used in CommonJS) and `import` (used in ES Modules).

# What are CommonJS and ES Modules?

* **CommonJS** (CJS) is the **module system** used by default in **Node.js** for server-side JavaScript. It was designed for synchronous loading of modules and has been widely used in Node.js for many years.

* **ES Modules** (ESM) is the **standardized module system** in **JavaScript**, defined by the ECMAScript specification. It’s part of the JavaScript language itself and is used in modern JavaScript, both in the browser and in Node.js (starting from Node 12+).

---

# Key Differences Between CommonJS and ES Modules

## Module Syntax: `require` vs `import`

* **CommonJS (CJS)** uses the `require` function to import modules and `module.exports` to export them.

  * **Importing**:

```javascript
const moduleName = require('moduleName');
```

  *   
    **Exporting**:

```javascript
module.exports = someFunction;
```

*   
  **ES Modules (ESM)** uses `import` to load modules and `export` to define them.

  * **Importing**:

```javascript
import { something } from 'moduleName';
```

  *   
    **Exporting**:

```javascript
export const someFunction = () => {};
```

## Synchronous vs Asynchronous Loading

* **CommonJS** modules are **synchronously loaded**. This means the `require` statement blocks the execution of the code until the module is loaded. This is fine for server-side environments like Node.js, where all the files are local and it's okay to block the process for a short time.

   Example (CommonJS):

```javascript
const fs = require('fs'); // Synchronous
const data = fs.readFileSync('file.txt', 'utf8');
```

*   
  **ES Modules** are **asynchronous by default**, meaning they can be used for **dynamic loading** and better optimizations in the browser (like lazy-loading modules). This is especially useful for front-end development, where you want modules to be loaded only when necessary.

   Example (ESM):

```javascript
import { readFile } from 'fs/promises'; // Asynchronous
const data = await readFile('file.txt', 'utf8');
```

## Module Export/Import Patterns

* **CommonJS** exports a **single object** (or value) and imports modules via `require`.

   Example (CommonJS Export):

```javascript
// file.js
module.exports = {
  sayHello: function() {
    console.log('Hello, world!');
  },
};
```

*   
  Example (CommonJS Import):

```javascript
// app.js
const file = require('./file');
file.sayHello();
```

*   
  **ES Modules** allow **named exports** (specific values) and **default exports** (the main exported value of the module).

   Example (ESM Named Export):

```javascript
// file.js
export const sayHello = () => {
  console.log('Hello, world!');
};
```

*   
  Example (ESM Default Export):

```javascript
// file.js
export default function() {
  console.log('Hello, world!');
};
```

*   
  Example (ESM Import):

```javascript
// app.js
import { sayHello } from './file'; // Named import
sayHello();

import sayHelloFunction from './file'; // Default import
sayHelloFunction();
```

## Support and Compatibility

* **CommonJS** is supported **natively** by Node.js, as it’s the default module system. Older versions of Node.js (before v12) only supported CommonJS.

* **ES Modules** (ESM) were initially not supported by Node.js (pre-Node 12), but they are now **fully supported** starting from Node 12 and above. However, Node.js handles ESM in a slightly different way compared to CommonJS (more on that later).

## File Extensions

* **CommonJS** typically uses `.js` for all files, including modules.

* **ES Modules** use the `.mjs` extension (when using Node.js). However, starting from Node.js v14, you can use `.js` with the `"type": "module"` setting in your `package.json` to indicate that the files are ES Modules.

## Circular Dependencies

* **CommonJS** modules handle **circular dependencies** more gracefully. If module A depends on module B, and module B depends on module A, Node.js can handle this and will still resolve the dependencies correctly.

* **ES Modules** handle **circular dependencies** more strictly. If there’s a circular dependency, Node.js will load the module, but the dependencies may be in a partially constructed state. This can lead to unexpected behavior in some cases.

## Dynamic Import (ESM Only)

* **ES Modules** support **dynamic imports**, which allow you to load modules dynamically at runtime using the `import()` function. This is useful for lazy-loading modules only when needed.

   Example (Dynamic Import):

```javascript
const moduleName = await import('moduleName');
```

*   
  **CommonJS** doesn’t natively support dynamic imports; you need to use `require()` in a synchronous way.

---

# Node.js and ES Modules

Node.js introduced **experimental support** for ES Modules starting with **version 8** and improved it in later versions (v12 and v14). However, using **ESM** in Node.js requires a few configuration steps:

1. **Package.json Configuration**: To use ES Modules with `.js` extensions in Node.js, you need to specify `"type": "module"` in your `package.json`:

```
{
  "type": "module"
}
```

2.   
   This allows you to use `.js` files with `import` and `export` syntax.

3. **Using .mjs for ESM Files**: If you don't want to modify your `package.json`, you can still use ES Modules by renaming your files with the `.mjs` extension.

---

# Summary of CommonJS vs ES Modules

| Feature | CommonJS (require) | ES Modules (import/export) |
| ----- | ----- | ----- |
| **Syntax** | `require()` / `module.exports` | `import` / `export` |
| **Loading** | Synchronous | Asynchronous |
| **File Extension** | `.js` | `.mjs` or `.js` (with `"type": "module"` in `package.json`) |
| **Exports** | Single export object | Named and default exports |
| **Dynamic Import** | Not natively supported | `import()` for dynamic imports |
| **Circular Dependencies** | Gracefully handled | Can cause issues with incomplete modules |
| **Compatibility** | Native to Node.js | Supported in modern Node.js (v12+), browsers |

# When to Use Each?

* **CommonJS** is ideal when working with **Node.js** environments that depend on synchronous loading and older codebases. It’s still widely used for back-end development and legacy systems.

* **ES Modules** is the **modern** standard for both the browser and server-side JavaScript (in environments like Node.js). It's better for **modular code** and **asynchronous loading**, especially for front-end development where you might want to lazily load code.

---

Let me know if you need more details or examples on any specific part\!

