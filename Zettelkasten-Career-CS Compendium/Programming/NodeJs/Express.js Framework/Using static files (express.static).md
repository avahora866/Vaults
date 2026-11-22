---
tags:
  - FORMAT
  - NodeJs
---
In Express.js, serving **static files** (like images, CSS files, JavaScript files, and other assets) is a common requirement for web applications. Express provides a built-in middleware function called **`express.static`** to handle this task.

# What are Static Files?

Static files are files that do not change dynamically and are served as-is to the client. Examples include:

* **Images**: JPEG, PNG, GIF, SVG, etc.

* **CSS Files**: Stylesheets that define the look and feel of the website.

* **JavaScript Files**: Client-side scripts that enhance the interactivity of the website.

* **Fonts and other assets**: Any other files that do not change in response to user input.

# Serving Static Files with `express.static`

To serve static files in an Express application, you use the `express.static` middleware, which is configured to serve files from a specific directory.

## Basic Usage of `express.static`:

```javascript
const express = require('express');
const app = express();

// Serve static files from the 'public' folder
app.use(express.static('public'));

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

In this example:

* `express.static('public')` tells Express to serve static files from the `public` directory.

* When a client requests a file (e.g., `/images/photo.jpg`), Express will look for it inside the `public/images` directory and serve it if it exists.

## Directory Structure Example:

```
/my-app
  /public
    /images
      photo.jpg
    /styles
      style.css
    /scripts
      app.js
  server.js
```

If you have this directory structure and you start the Express server as shown above, the following URLs will be served:

* **Image**: `http://localhost:3000/images/photo.jpg`

* **CSS File**: `http://localhost:3000/styles/style.css`

* **JavaScript File**: `http://localhost:3000/scripts/app.js`

# Serving Multiple Static Directories

You can use `express.static` multiple times to serve static files from different directories. This can be helpful if you want to serve assets like images from one folder and stylesheets from another.

## Example:

```javascript
app.use('/images', express.static('images'));
app.use('/styles', express.static('styles'));
```

In this case:

* The static files in the **`images`** directory will be served at `/images`.

* The static files in the **`styles`** directory will be served at `/styles`.

Now, if you visit:

* `http://localhost:3000/images/photo.jpg` → will serve the `photo.jpg` from the `images` directory.

* `http://localhost:3000/styles/style.css` → will serve the `style.css` from the `styles` directory.

# Setting Cache Control for Static Files

When serving static files, it is important to control how they are cached by the browser for performance optimization. You can configure caching headers using the `maxAge` option.

## Example with Cache Control:

```javascript
app.use(express.static('public', {
  maxAge: '1d'  // Cache static files for 1 day
}));
```

In this example, static files will be cached by the browser for **1 day**. The `maxAge` option can be specified in milliseconds (e.g., `maxAge: 86400000` for 1 day) or in a string format (e.g., `'1d'` for 1 day, `'1h'` for 1 hour, etc.).

# Serving a Custom Directory

By default, Express serves static files from the specified directory. However, you can also set a **custom URL path** for serving those files.

## Example:

```javascript
app.use('/assets', express.static('public'));
```

This will serve files from the `public` directory, but the client will access them via the `/assets` URL path:

* `http://localhost:3000/assets/images/photo.jpg`

* `http://localhost:3000/assets/styles/style.css`

# Using Static Middleware for a Single File

If you only need to serve a single file (like a specific HTML file), you can use the `res.sendFile()` method instead of `express.static`.

## Example:

```javascript
app.get('/about', (req, res) => {
  res.sendFile(__dirname + '/public/about.html');
});
```

This will serve the **`about.html`** file when the client accesses the `/about` route.

# Handling Errors for Missing Files

If a file is requested but does not exist, Express will automatically respond with a **404 Not Found** error. However, you can also customize this behavior by creating a custom error handler after setting up your static middleware.

## Example:

```javascript
app.use(express.static('public'));

// Custom 404 error handler for missing files
app.use((req, res, next) => {
  res.status(404).send('File Not Found');
});
```

# Security Considerations

* **Serving sensitive files**: You should avoid serving sensitive files, such as configuration files, credentials, or other private data, as static assets. Make sure that only publicly accessible files (like images, CSS, JavaScript) are exposed.

* **Directory Traversal**: Be cautious of directory traversal attacks, where a malicious user might try to access files outside the static directory by manipulating the URL (e.g., `../../etc/passwd`). Ensure that your static file serving is restricted to a specific folder using `express.static`.

---

# Summary of `express.static` Usage:

* `express.static` is a built-in middleware function used to serve static files (e.g., images, CSS, JS) in Express applications.

* You can serve static files from a specific directory by calling `app.use(express.static('directoryPath'))`.

* Static files can be served from multiple directories or under different URL paths.

* You can control caching behavior using the `maxAge` option.

* **404 errors** are automatically handled by Express if the requested file doesn’t exist, but you can customize error handling for static files.

By using `express.static`, you can easily serve assets and static resources in your Express app while also optimizing performance and security.

