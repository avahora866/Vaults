---
tags:
  - FORMAT
  - JavaScript
---
# Throwing Custom Errors in JavaScript (`throw new Error()`)

In JavaScript, you can create custom errors using the `throw` statement. This allows you to generate errors intentionally, providing more control over error handling in your applications.

---

## What is the `throw` Statement?

The `throw` statement lets you **generate a custom error** by throwing an error object. This can be used to handle specific situations where you want to enforce certain conditions or alert the developer to issues that don't fall under the usual built-in JavaScript errors.

### Syntax

```javascript
throw new Error("Error message");
```

*   
  **`new Error(message)`** creates a new error with a specified message.

* The **`throw`** keyword throws that error, which can then be caught using a `try...catch` block or handled by higher-level error management.

---

## Creating Custom Errors with `throw`

### Basic Example: Throwing an Error

```javascript
function checkAge(age) {
  if (age < 18) {
    throw new Error("Age must be 18 or older.");
  } else {
    console.log("Access granted.");
  }
}

try {
  checkAge(15);  // This will throw an error
} catch (error) {
  console.log("Error: " + error.message);  // "Error: Age must be 18 or older."
}
```

*   
  Here, the function `checkAge` throws an error if the age is less than 18\.

* The error is then caught in the `catch` block and handled appropriately.

---

## Customizing Error Objects

JavaScript’s `Error` object can be extended with additional properties or customized for more detailed error information.

### Example: Throwing a Custom Error with a Custom Message

```javascript
function validateNumber(num) {
  if (num < 0) {
    throw new Error("Number must be positive.");
  }
  return num;
}

try {
  validateNumber(-5);  // This will throw an error
} catch (error) {
  console.log("Custom Error: " + error.message);  // "Custom Error: Number must be positive."
}
```

In this case, the `throw new Error()` is used to create a custom error with a specific message.

---

## Custom Error Types

You can also create **custom error classes** that extend the built-in `Error` class to create more meaningful and specific error types.

### Example: Creating a Custom Error Class

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);  // Call the parent Error constructor
    this.name = "ValidationError";  // Custom error name
  }
}

function validateEmail(email) {
  const regex = /\S+@\S+\.\S+/;
  if (!regex.test(email)) {
    throw new ValidationError("Invalid email format.");
  }
}

try {
  validateEmail("invalidemail.com");  // This will throw a custom error
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(`${error.name}: ${error.message}`);  // "ValidationError: Invalid email format."
  } else {
    console.log("An unknown error occurred.");
  }
}
```

*   
  Here, we create a custom error class `ValidationError`, which extends the built-in `Error` class.

* The error is thrown with a specific type (`ValidationError`), allowing you to distinguish it from other errors.

---

## Accessing Error Properties

When you throw a custom error, it behaves similarly to built-in errors:

* **`message`** – The error message you provide.

* **`name`** – The name of the error (e.g., `ValidationError` in the example above).

* **`stack`** – A stack trace that shows where the error occurred in the code.

You can access these properties for better debugging and logging.

### Example: Accessing Error Stack

```javascript
try {
  throw new Error("Something went wrong");
} catch (error) {
  console.log(error.stack);  // Provides the stack trace with the error location
}
```

---

## Summary of `throw new Error()`

| Statement | Description | Example |
| ----- | ----- | ----- |
| `throw new Error(message)` | Creates and throws a custom error | `throw new Error("Something went wrong");` |
| `new Error(message)` | Constructs a new error object with a message | `new Error("Custom error message");` |
| `instanceof` | Used to check if the error is of a specific type | `if (error instanceof ValidationError)` |

---

## Key Takeaways

* Use **`throw new Error(message)`** to create and throw custom errors.

* You can create **custom error types** by extending the `Error` class.

* Handling errors with `try...catch` allows you to catch these custom errors and handle them effectively.

* By throwing custom errors, you can make your code more readable and maintainable, especially in complex applications.

