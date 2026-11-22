---
tags:
  - FORMAT
  - JavaScript
---
# Modules in JavaScript (import/export)

In JavaScript, modules allow you to break up your code into smaller, reusable pieces. This modular approach helps in organizing large codebases and improves maintainability. The **`import`** and **`export`** keywords, introduced in **ES6**, provide a way to work with modules in JavaScript.

---

## 1️⃣ What are JavaScript Modules?

Modules are individual units of code that can be imported and exported between different JavaScript files. They allow you to divide your code into separate files, making it more manageable and maintainable.

Modules can export functions, objects, or values from one file and import them into another.

---

## 2️⃣ Exporting Modules

The **`export`** keyword is used to expose variables, functions, or objects from a module, making them available for use in other files.

### Named Exports

With **named exports**, you can export multiple values from a module. These values can be functions, objects, or primitive values.

#### Example:

```javascript
// math.js (module file)

export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b;
```

Here, three functions (`add`, `subtract`, and `multiply`) are exported using named exports.

#### Named Exports with Aliases

You can also export a variable or function under a different name using **aliases**.

```javascript
export { add as addition, subtract as subtraction };
```

### Default Exports

A **default export** allows you to export a single value, such as a function or a class, from a module. You don’t need to use curly braces when importing a default export.

#### Example:

```javascript
// math.js (module file)

const multiply = (a, b) => a * b;
export default multiply;
```

Here, the `multiply` function is exported as the **default** export of the module.

You can only have one **default export** per module.

---

## 3️⃣ Importing Modules

You can use the **`import`** keyword to bring in exported values from other files (modules). Depending on how the values are exported, there are different ways to import them.

### Named Imports

If you are importing **named exports**, you must use the exact name of the export, enclosed in curly braces.

#### Example:

```javascript
// app.js (another file)

import { add, subtract } from './math.js';

console.log(add(2, 3)); // Output: 5
console.log(subtract(5, 2)); // Output: 3
```

Here, both `add` and `subtract` are named exports from the `math.js` module, and they are imported into the `app.js` file.

### Importing with Aliases

You can import named exports under a different name using **aliases**.

```javascript
import { add as addition, subtract as subtraction } from './math.js';

console.log(addition(3, 4)); // Output: 7
console.log(subtraction(5, 2)); // Output: 3
```

### Default Imports

If a module has a **default export**, you can import it without curly braces and give it any name you want.

#### Example:

```javascript
// app.js (another file)

import multiply from './math.js';

console.log(multiply(3, 4)); // Output: 12
```

In this case, `multiply` is the default export from `math.js`, and it is imported into `app.js` as the `multiply` function.

---

## 4️⃣ Combining Named and Default Imports

You can import both named exports and the default export from a module in the same import statement.

#### Example:

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export default (a, b) => a * b;
```

```javascript
// app.js
import multiply, { add, subtract } from './math.js';

console.log(multiply(2, 3)); // Output: 6
console.log(add(2, 3)); // Output: 5
console.log(subtract(5, 2)); // Output: 3
```

Here, `multiply` is the default export, and `add` and `subtract` are named exports.

---

## 5️⃣ Exporting Multiple Items in One Statement

You can also export multiple items in a single statement.

#### Example:

```javascript
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;

export { add, subtract, multiply };
```

This approach allows you to export multiple values from the same statement.

---

## 6️⃣ Dynamic Imports

In addition to the static `import` statements, JavaScript also allows **dynamic imports**, where modules are loaded at runtime. This is useful when you need to load a module only when it’s required, improving performance and reducing the initial load time.

#### Example:

```javascript
const loadMathModule = async () => {
  const { add, subtract } = await import('./math.js');
  console.log(add(2, 3)); // Output: 5
  console.log(subtract(5, 2)); // Output: 3
};

loadMathModule();
```

Here, the `import()` function is used to dynamically import the `math.js` module. This returns a promise, which resolves to the module's exports.

---

## 7️⃣ Browser and Node.js Support

* **Browser Support**: Most modern browsers now support JavaScript modules using `<script type="module">` tags. However, if you need to support older browsers, you may need a build tool like **Webpack** or **Babel** to transpile the code into a compatible format.

* **Node.js Support**: In Node.js, modules can be used by either using `.mjs` extensions or setting `"type": "module"` in the `package.json`. Otherwise, Node.js uses the CommonJS module system with `require` and `module.exports`.

---

## 8️⃣ Best Practices for Using Modules

* **Use `const` and `let` for exports**: Always use `const` or `let` for exporting functions or values to ensure they are not accidentally re-assigned.

* **Prefer default exports for a single value**: If a module exports only one thing (like a single function or class), use a **default export**. This provides cleaner and more intuitive imports.

* **Group related functions or constants together**: Group related variables, functions, or objects in one module to keep the codebase organized.

* **Use named exports for multiple items**: When a module exports multiple functions or objects, use **named exports**. This provides more clarity and flexibility when importing specific values.

---

## 9️⃣ Conclusion

JavaScript modules with **`import`** and **`export`** provide a powerful way to break down code into reusable, maintainable components. The key points to remember are:

* **Named Exports**: Export multiple items from a module, which need to be imported by their exact names.

* **Default Exports**: Export a single item from a module, which can be imported with any name.

* **Dynamic Imports**: Import modules dynamically at runtime to improve performance.

By using modules, you can organize your code better, create reusable components, and maintain a clean codebase.

