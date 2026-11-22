---
tags:
  - FORMAT
  - NodeJs
---
# Using `process.on("uncaughtException")` and `process.on("unhandledRejection")` in Node.js

In Node.js, errors that are not properly handled can cause your application to crash. However, you can use **global error handling** for certain types of errors that may occur, such as uncaught exceptions and unhandled promise rejections. These global handlers allow you to manage errors that have not been caught by `try...catch` blocks or `.catch()` methods.

Let’s dive into both of these events:

---

# `process.on("uncaughtException")`

The `uncaughtException` event is emitted when an exception is thrown and not caught anywhere in the application. It typically results from errors that are not handled by `try...catch` or other error-handling mechanisms.

## How `uncaughtException` Works:

* This event is triggered when your program encounters an error that isn't caught in the normal way.

* You can use `process.on("uncaughtException")` to handle this type of error globally.

* **Warning**: It’s generally **not recommended** to use this for regular error handling. Handling uncaught exceptions may allow the application to continue running, but it can leave the program in an inconsistent or corrupt state. It’s typically better to handle errors locally where they occur and gracefully shut down or restart the process when necessary.

## Example of Using `process.on("uncaughtException")`:

```javascript
// Add a global uncaughtException handler
process.on('uncaughtException', (error) => {
  console.error('An uncaught exception occurred:', error.message);
  // Gracefully shutdown the app or log the error
  process.exit(1); // Optional: exit the process with error code
});

// Example of an uncaught exception
setTimeout(() => {
  throw new Error('Something went wrong!');
}, 1000);
```

In this example:

* When the error is thrown inside the `setTimeout`, it is not caught anywhere, and the `uncaughtException` handler is triggered.

* The application logs the error message and exits with a status code `1` (indicating failure).

## Important Considerations:

* It is better to catch errors as close to their source as possible rather than relying on `uncaughtException`.

* If you do use this handler, ensure that you log the error and clean up any resources before shutting down the application or restarting it.

---

# `process.on("unhandledRejection")`

The `unhandledRejection` event is emitted when a **promise rejection** occurs and there is no `.catch()` handler attached to the promise.

* In earlier versions of Node.js (before v15), unhandled promise rejections would silently fail, which could lead to unexpected behavior.

* In **Node.js v15 and beyond**, unhandled promise rejections now cause the process to exit by default unless you explicitly handle them with `process.on("unhandledRejection")`.

## How `unhandledRejection` Works:

* This event is useful for tracking and handling unhandled promise rejections.

* **Best practice**: Always handle promise rejections using `.catch()` or `async/await` with `try...catch`.

* If you don't handle a promise rejection, you can catch it globally using `process.on("unhandledRejection")`.

## Example of Using `process.on("unhandledRejection")`:

```javascript
// Add a global unhandledRejection handler
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled rejection at:', promise, 'reason:', reason);
  // Gracefully shutdown or log the error
  process.exit(1); // Optional: exit the process with error code
});

// Example of an unhandled promise rejection
const myPromise = new Promise((resolve, reject) => {
  reject(new Error('This promise was rejected!'));
});
```

In this example:

* The promise `myPromise` is rejected without a `.catch()` handler, and the `unhandledRejection` handler is triggered.

* The application logs the rejection reason and promise, and then exits with a status code `1`.

## Important Considerations:

* From Node.js v15 onwards, unhandled promise rejections will terminate the process by default. You can change this behavior to log the rejection instead of exiting by setting `process.on('unhandledRejection', 'warn')` (this is Node.js behavior as of v15).

* It's always better to handle promise rejections where they occur, using `.catch()` or `try...catch` for async/await functions.

---

# Debugging in Node.js

When debugging a Node.js application, there are several ways to diagnose and understand errors or inspect how your program is executing. The most common methods for debugging are `console.log`, the built-in `debugger` module, and using an IDE like **VS Code**.

---

# Debugging with `console.log`

`console.log` is one of the simplest and most common ways to output debugging information. It can be used to print variable values, track the flow of execution, or display error messages.

## Example:

```javascript
function add(a, b) {
  console.log('Adding:', a, b); // Debugging output
  return a + b;
}

const result = add(5, 10);
console.log('Result:', result); // Output the result
```

In this example:

* `console.log` is used to print the values of `a` and `b` as they are passed into the `add` function.

* After calculating the result, `console.log` outputs the result to the console.

While `console.log` is helpful, it can clutter the output, and overuse can make it harder to manage your code. For more structured debugging, consider using the built-in `debugger` or tools like **VS Code Debugger**.

---

# Debugging with `debugger`

Node.js has a built-in **debugger** module that allows you to set breakpoints and inspect your code during execution.

## Steps to Use the Debugger:

1. Add `debugger` to your code where you want to break:

```javascript
function add(a, b) {
  debugger; // Execution will stop here when you run the app in debug mode
  return a + b;
}

add(5, 10);
```

2.   
   Start the Node.js application in debug mode using the `inspect` flag:

```
node --inspect-brk app.js
```

*   
  The `--inspect-brk` flag tells Node.js to start the debugger and break (pause) before executing any code.

3. Open Chrome DevTools or any other debugger tool to inspect the state of your application:

   * Go to `chrome://inspect` in Google Chrome and click on "Inspect" to open the debugging interface.

   * You can now step through the code, inspect variables, and track the flow of your application.

## Advantages of `debugger`:

* The `debugger` allows you to interactively step through the code, inspect variable values, and troubleshoot errors.

* It can be much more effective than just using `console.log` for complex applications.

---

# Debugging with VS Code Debugger

Visual Studio Code (VS Code) offers powerful debugging tools that integrate directly into the editor, allowing you to set breakpoints, step through code, and inspect variables in an interactive environment.

## Steps to Use the VS Code Debugger:

1. **Open your project** in **VS Code**.

2. **Set breakpoints** in your code by clicking in the gutter next to the line numbers where you want execution to pause.

3. **Start the debugger**:

   * Click the **Run and Debug** button on the left sidebar (or press `F5`).

   * Choose **Node.js** when prompted for the environment to run your application in debug mode.

4. **Inspect variables** and step through the code:

   * The debugger will pause at your breakpoints, and you can inspect variables, step through the code line by line, and even evaluate expressions.

5. **View the call stack** and **debug output**:

   * The Debugger window will show the call stack and give you controls to continue, step over, step into, or step out of the code.

## Advantages of VS Code Debugger:

* Provides a visual interface for debugging, making it easy to inspect and interact with your application.

* Allows you to set and manage multiple breakpoints, watch variables, and evaluate expressions dynamically.

* Offers a more robust debugging experience than `console.log` or the built-in Node.js debugger.

---

# Conclusion

* **`process.on("uncaughtException")`**: Handles uncaught exceptions that aren’t caught by any other error handler. Use cautiously as it may leave the app in an inconsistent state.

* **`process.on("unhandledRejection")`**: Catches unhandled promise rejections. Always handle promise rejections where they occur to avoid crashes.

* **Debugging**:

  * **`console.log`** is the simplest form of debugging but can become cluttered.

  * **`debugger`** provides an interactive step-through debugging experience, useful for complex applications.

  * **VS Code Debugger** is a powerful, feature-rich tool that allows you to visually debug your Node.js application with ease.

By combining these techniques, you can effectively manage and debug errors in your Node.js applications, ensuring better error handling and smoother development workflows.

