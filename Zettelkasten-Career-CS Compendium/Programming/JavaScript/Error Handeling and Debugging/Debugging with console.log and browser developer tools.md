---
tags:
  - FORMAT
  - JavaScript
---
# Debugging with `console.log` and Browser Developer Tools

Debugging is an essential part of development, allowing you to identify and fix issues in your code. JavaScript provides several tools for debugging, with **`console.log()`** and **browser developer tools** being two of the most common methods.

---

## Debugging with `console.log()`

The `console.log()` method is one of the simplest and most widely used debugging tools. It allows you to print values to the console, providing insight into the current state of your code.

### Basic Example of `console.log()`

```javascript
let x = 10;
let y = 5;
let result = x + y;

console.log(result);  // Output: 15
```

*   
  **Use Case:** You can use `console.log()` to print variable values, check the flow of execution, or inspect the result of an operation.

### Example: Logging Complex Objects

```javascript
const user = {
  name: "John",
  age: 30,
  occupation: "Developer"
};

console.log(user);
```

*   
  **Use Case:** Log entire objects to inspect their properties and values.

### Example: Logging Function Calls

```javascript
function add(a, b) {
  console.log(`Adding ${a} and ${b}`);
  return a + b;
}

let sum = add(3, 7);
```

*   
  **Use Case:** Track the execution of functions and check their input/output.

### Console Methods for Enhanced Debugging

* **`console.error(message)`**: Logs an error message.

```javascript
console.error("This is an error message");
```

*   
  **`console.warn(message)`**: Logs a warning message.

```javascript
console.warn("This is a warning");
```

*   
  **`console.table(data)`**: Displays tabular data in a neat table format.

```javascript
const people = [{ name: "John", age: 30 }, { name: "Jane", age: 25 }];
console.table(people);
```

While `console.log()` is great for simple debugging, it can become messy in larger applications. In such cases, you may need to use **browser developer tools** for a more powerful debugging experience.

---

## Using Browser Developer Tools

Modern browsers like Chrome, Firefox, and Edge offer built-in developer tools that provide advanced debugging features. The most commonly used tools are the **Console**, **Sources** (or Debugger), and **Network** panels.

### Accessing Developer Tools

* **Google Chrome:** Press `F12` or `Ctrl + Shift + I` (Windows/Linux) or `Cmd + Option + I` (Mac).

* **Firefox:** Press `F12` or `Ctrl + Shift + I` (Windows/Linux) or `Cmd + Option + I` (Mac).

* **Microsoft Edge:** Press `F12` or `Ctrl + Shift + I` (Windows) or `Cmd + Option + I` (Mac).

### Using the Console Panel

The **Console** panel allows you to:

* View `console.log()`, `console.error()`, `console.warn()`, and other log messages.

* Execute JavaScript directly in the browser.

### Example: Using Console to Test Code

* In the Console, you can write JavaScript code and instantly see results.

```javascript
let sum = 5 + 10;
sum;  // Will display 15 in the console.
```

### Using the Sources (Debugger) Panel

The **Sources** panel provides a powerful way to debug your JavaScript code by setting breakpoints, stepping through code, and inspecting variables.

#### Steps for Using the Debugger

1. **Set Breakpoints**: Click on the line number in your JavaScript file (loaded in the Sources tab) to set a breakpoint.

2. **Step Through Code**: Once the breakpoint is hit, you can:

   * **Step Over** (`F10`): Move to the next line of code.

   * **Step Into** (`F11`): Enter into the function calls.

   * **Step Out** (`Shift + F11`): Exit from the current function.

3. **Watch Variables**: You can monitor the values of variables in the **Watch** section or **Scope**.

4. **Inspect Call Stack**: See the sequence of function calls leading up to the breakpoint in the **Call Stack**.

### Example: Debugging a Function

```javascript
function multiply(a, b) {
  let result = a * b;
  return result;
}

console.log(multiply(2, 3));
```

1.   
   Open the Sources panel in Developer Tools.

2. Set a breakpoint on the `let result = a * b;` line.

3. Reload the page or trigger the function.

4. The code will pause at the breakpoint, and you can inspect the values of `a`, `b`, and `result`.

### Using the Network Panel

The **Network** panel is useful for debugging network requests like API calls and loading resources:

* View all the network requests made by your page (e.g., AJAX calls, image loading, etc.).

* Inspect request and response data.

### Example: Debugging an API Call

1. Open the **Network** tab in Developer Tools.

2. Make an API request (e.g., via `fetch()` or `XMLHttpRequest`).

3. The request will appear in the Network tab. You can inspect the status, headers, and response.

---

## Summary of Debugging Tools

| Tool/Method | Description |
| ----- | ----- |
| `console.log()` | Prints values to the console. |
| `console.error()` | Logs an error message. |
| `console.warn()` | Logs a warning message. |
| `console.table()` | Logs data in a table format. |
| **Sources (Debugger)** | Set breakpoints and step through code. Inspect variables and call stacks. |
| **Network Panel** | Monitor network requests and API calls. |

---

## Key Takeaways

* **`console.log()`** is great for quick debugging, but it may clutter the code in larger projects.

* Use **browser developer tools** for more powerful debugging, including inspecting variables, setting breakpoints, and monitoring network requests.

* **The Sources (Debugger)** and **Network** panels are especially useful for tracking down complex issues, such as logic errors and API call problems.

By using these tools effectively, you can significantly improve your debugging process, making it easier to track down and fix issues in your JavaScript code.

