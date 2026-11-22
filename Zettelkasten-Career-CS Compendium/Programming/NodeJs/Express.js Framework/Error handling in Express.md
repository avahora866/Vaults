---
tags:
  - FORMAT
  - NodeJs
---
Error handling is an important aspect of building robust and reliable applications in **Express.js**. In Express, error handling is used to catch and respond to errors that occur during the processing of requests. This ensures that the application behaves predictably and provides useful feedback to users when something goes wrong.

Here's a breakdown of how to handle errors in Express.

---

# Handling Errors in Route Handlers

In Express, errors can occur in the route handlers (for example, when querying a database or processing a request). You can use a `try-catch` block to handle errors in **asynchronous code** (such as using `async/await`) or simply handle synchronous errors directly.

## Synchronous Error Handling:

If an error occurs inside a synchronous function, you can simply throw it or use `next()` to pass it to the next middleware for error handling.

### Example of synchronous error handling:

```javascript
app.get('/', (req, res) => {
  throw new Error('Something went wrong!');
});
```

In this case, the error will be passed to the error-handling middleware.

---

# Handling Errors in Asynchronous Code (async/await)

For **asynchronous operations** (such as making a database query, calling an external API, or reading a file), you need to handle errors properly. Using `async/await` makes it easier to work with asynchronous code, and `try-catch` can be used to catch errors that may occur.

### Example of asynchronous error handling:

```javascript
app.get('/user/:id', async (req, res, next) => {
  try {
    const userId = req.params.id;
    // Simulating an async operation (e.g., fetching data from a database)
    const user = await getUserFromDatabase(userId);
    if (!user) {
      throw new Error('User not found');
    }
    res.json(user);
  } catch (err) {
    next(err);  // Pass error to the next error-handling middleware
  }
});
```

Here:

* `getUserFromDatabase(userId)` is an asynchronous operation.

* If there’s an error (e.g., user not found), it's caught in the `catch` block and passed to the `next()` function.

---

# Error Handling Middleware

Express has a built-in mechanism to handle errors using **error-handling middleware**. This middleware is defined with **four parameters**: `err`, `req`, `res`, and `next`.

The error-handling middleware is placed at the **end** of your middleware stack. It catches any errors passed from previous route handlers or middleware.

## Error Handling Middleware Syntax:

```javascript
app.use((err, req, res, next) => {
  // Handle the error
  res.status(500).json({
    message: err.message || 'Something went wrong!',
    stack: err.stack
  });
});
```

*   
  The **`err`** parameter contains the error object.

* **`req`** and **`res`** are the standard request and response objects.

* **`next`** is used to pass control to the next middleware, but in error-handling middleware, it is not usually needed.

## Example of Error Handling Middleware:

```javascript
// Route that causes an error
app.get('/error', (req, res, next) => {
  const err = new Error('This is a custom error');
  err.status = 400;  // Optionally set a custom error status code
  next(err);  // Pass the error to the error-handling middleware
});

// Error-handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);  // Log the error stack
  res.status(err.status || 500).json({
    message: err.message || 'Something went wrong!',
    stack: err.stack  // You can optionally include the stack trace
  });
});
```

*   
  The first route causes an error and calls `next(err)`, which passes the error to the error-handling middleware.

* The error-handling middleware catches the error and responds with a **status code** and **message**.

---

# Custom Error Handling

You can define custom error types by creating your own error classes. This allows you to manage and handle different types of errors more easily.

## Example of Custom Error Class:

```javascript
class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = 'NotFoundError';
    this.status = 404;  // Set a custom status code
  }
}

app.get('/user/:id', (req, res, next) => {
  const user = null;  // Simulating a user lookup that returns null
  if (!user) {
    return next(new NotFoundError('User not found'));
  }
  res.json(user);
});

// Error-handling middleware
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    message: err.message,
    name: err.name
  });
});
```

*   
  In this case, `NotFoundError` is a custom error class that extends the native `Error` class and adds a custom `status` code and `name`.

* If a user is not found, we pass an instance of `NotFoundError` to `next()`, and the error-handling middleware will process it.

---

# Default Error Handling in Express

By default, if an error occurs and is not caught by any middleware or route handler, Express will send a **500 Internal Server Error** response. However, it’s a good practice to create a custom error-handling middleware to provide more useful information (e.g., custom error messages, status codes).

---

# Handling Validation Errors

Validation errors often happen when incoming data doesn't meet certain criteria. It's common to use libraries like **Joi**, **express-validator**, or **celebrate** for validating input. In case of validation failure, you can handle errors like other types.

## Example with `express-validator`:

```javascript
const { body, validationResult } = require('express-validator');

app.post('/user', [
  body('email').isEmail().withMessage('Invalid email format'),
  body('name').notEmpty().withMessage('Name is required')
], (req, res, next) => {
  const errors = validationResult(req);  // Get validation errors
  if (!errors.isEmpty()) {
    const error = new Error('Validation failed');
    error.status = 400;
    error.errors = errors.array();  // Attach errors array to the error object
    return next(error);
  }
  
  const user = req.body;
  res.status(201).send(`User ${user.name} created successfully!`);
});

// Error-handling middleware
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    message: err.message,
    errors: err.errors || []
  });
});
```

In this case, **validation errors** are captured using `express-validator`. If validation fails, we create a custom error and pass it to the error-handling middleware.

---

# Sending Custom Error Responses

You can also send more detailed error responses, including the error code, message, and additional data (like validation errors, error codes, etc.).

## Example:

```javascript
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    error: {
      message: err.message,
      code: err.status || 500,
      stack: process.env.NODE_ENV === 'development' ? err.stack : {}  // Only show stack trace in development
    }
  });
});
```

In this example:

* The error response includes the **message**, **status code**, and **stack trace** (only in development mode).

---

# Conclusion

Error handling in Express can be broken down into the following key steps:

1. **Synchronous Error Handling**: Use `throw` to generate errors, or call `next()` with an error object.

2. **Asynchronous Error Handling**: Use `try-catch` blocks around asynchronous code and call `next(err)` to pass errors to the middleware.

3. **Error Handling Middleware**: Use a middleware with four parameters (`err, req, res, next`) at the end of your middleware stack to handle errors.

4. **Custom Error Handling**: Create custom error classes for more structured error handling and better clarity.

5. **Validation Errors**: Handle validation errors gracefully and provide useful feedback to the client.

By setting up a robust error-handling strategy, you ensure your application is resilient, and users get informative responses when something goes wrong.

