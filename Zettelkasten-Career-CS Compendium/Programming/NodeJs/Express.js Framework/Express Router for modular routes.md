---
tags:
  - Comprehend
  - FORMAT
  - NodeJs
---
In **Express.js**, the **Router** is a powerful tool that allows you to create **modular** and **maintainable** route handlers. Instead of defining all your routes in a single file, you can break them into smaller, more manageable chunks by using **Express Router**. This helps keep your application clean, organized, and scalable.

# What is Express Router?

The **Express Router** is an instance of a middleware and routing system that allows you to define a group of routes separately from the main application (often in different files or modules). It’s similar to a mini Express application where you can define routes, middleware, and even error handling.

By using **Express Router**, you can organize routes logically based on features, routes, or modules of your application.

# Basic Example of Using Express Router

To begin with, you need to **import** and create an instance of **Router**.

## Example of Basic Setup:

```javascript
const express = require('express');
const app = express();
const router = express.Router();

// Define a route using the router
router.get('/hello', (req, res) => {
  res.send('Hello from the Router!');
});

// Use the router in your application
app.use('/api', router);

// Start the server
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

In this example:

* **Router instance**: We create an instance of the router using `express.Router()`.

* **Route definition**: We define a route (e.g., `/hello`) in the router.

* **Mounting the router**: We mount the router on a specific URL path `/api` using `app.use('/api', router)`. Now, the route `/hello` will be accessible at `http://localhost:3000/api/hello`.

# Modularizing Routes with Express Router

A major benefit of using the router is **modularization**. This allows you to break down your route handlers into separate files, making your code more maintainable and organized.

## Example Directory Structure:

```
/my-app
  /routes
    users.js
    posts.js
  server.js
```

## Step 1: Create Modular Routes (users.js)

In `routes/users.js`, you can define the user-related routes:

```javascript
const express = require('express');
const router = express.Router();

// Get all users
router.get('/', (req, res) => {
  res.send('List of users');
});

// Get a specific user
router.get('/:id', (req, res) => {
  res.send(`User with ID ${req.params.id}`);
});

module.exports = router;
```

## Step 2: Create Another Route File (posts.js)

In `routes/posts.js`, define the post-related routes:

```javascript
const express = require('express');
const router = express.Router();

// Get all posts
router.get('/', (req, res) => {
  res.send('List of posts');
});

// Create a new post
router.post('/', (req, res) => {
  res.send('New post created');
});

module.exports = router;
```

## Step 3: Import and Use Routes in the Main Application (server.js)

In the main server file (`server.js`), you can import these modular routes and use them:

```javascript
const express = require('express');
const app = express();

// Import modular route handlers
const usersRouter = require('./routes/users');
const postsRouter = require('./routes/posts');

// Use the routers for specific paths
app.use('/api/users', usersRouter);  // All user-related routes will be under /api/users
app.use('/api/posts', postsRouter);  // All post-related routes will be under /api/posts

// Start the server
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

In this setup:

* **`/api/users`** routes are handled by the `usersRouter` module.

* **`/api/posts`** routes are handled by the `postsRouter` module.

# Benefits of Using Express Router for Modular Routes

1. **Code Organization**:

   * By splitting route handlers into separate files (modules), your code becomes more organized and easier to manage. You can group related routes together, such as all user-related routes in one file and post-related routes in another.

2. **Separation of Concerns**:

   * The Router helps you achieve **Separation of Concerns** (SoC). Each file/module focuses on a specific responsibility, like handling routes related to users or posts, without cluttering the main server file.

3. **Reusability**:

   * You can reuse the router in multiple places within your application or even across multiple applications. For example, the `usersRouter` can be used for user management across different microservices.

4. **Scalability**:

   * As your application grows, you can keep adding new route files (modules) as needed without making the main application file too large or complex. You can also organize route modules by features (authentication, posts, comments, etc.).

# Router Middleware

Just like with the main app, you can apply middleware to the router. Middleware functions can be applied globally for all routes in a router, or you can apply them to specific routes.

## Example of Router Middleware:

```javascript
// Middleware that runs for all routes in this router
router.use((req, res, next) => {
  console.log('This middleware runs for all routes in the router');
  next();
});

// Specific route with its own middleware
router.get('/:id', (req, res, next) => {
  console.log(`Fetching user with ID: ${req.params.id}`);
  next();
}, (req, res) => {
  res.send(`User with ID ${req.params.id}`);
});
```

# Using Router for HTTP Methods (GET, POST, PUT, DELETE)

You can use the router to handle different HTTP methods such as GET, POST, PUT, and DELETE for specific routes.

## Example:

```javascript
router.get('/', (req, res) => {
  res.send('Get all users');
});

router.post('/', (req, res) => {
  res.send('Create a new user');
});

router.put('/:id', (req, res) => {
  res.send(`Update user with ID ${req.params.id}`);
});

router.delete('/:id', (req, res) => {
  res.send(`Delete user with ID ${req.params.id}`);
});
```

# Router Parameters and Query Parameters

You can use **route parameters** (e.g., `/users/:id`) and **query parameters** (e.g., `/users?id=1`) in your router as you would in regular Express routes.

## Example:

```javascript
router.get('/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User with ID: ${userId}`);
});

router.get('/', (req, res) => {
  const { age } = req.query;  // Get query parameter
  res.send(`Users with age: ${age}`);
});
```

# Conclusion

Using **Express Router** for modular routes has several advantages, especially as your application grows. It allows you to:

* Organize your route handlers into separate modules, making your application more maintainable.

* Keep the main server file clean and easy to understand.

* Scale the application efficiently by adding new route files for different features or modules.

By leveraging **express.Router()**, you can create well-structured, organized, and scalable Express applications that are easier to maintain and extend.

