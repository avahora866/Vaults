---
tags:
  - FORMAT
  - NodeJs
---
Sure\! In Node.js, the `http` module allows you to create an HTTP server to handle web requests. This is the foundation of web applications in Node.js, enabling you to respond to HTTP requests and send data back to the client.

# Creating an HTTP Server

You can use the `http.createServer()` method to create a server. This method takes a **callback function** that will be triggered whenever the server receives an HTTP request.

# Basic Syntax

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // request (req) and response (res) are passed to the callback
});
```

The callback function receives two arguments:

* `req` (request): The incoming request from the client (contains information like URL, headers, query params, etc.).

* `res` (response): The outgoing response that you send back to the client (you can write status codes, headers, and content).

---

# Creating a Basic Server

Here’s a simple example where the server responds with "Hello, World\!" to every request:

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200; // HTTP status code
  res.setHeader('Content-Type', 'text/plain'); // Response header
  res.end('Hello, World!'); // Response body
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

# Key Components

1. **Listening for Requests**:  
    The `server.listen()` method is used to make the server listen for incoming requests on a specific port.

```javascript
server.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

2.   
   **Setting HTTP Status Code**:  
    You can set the status code of the response (e.g., `200 OK`, `404 Not Found`, etc.).

```javascript
res.statusCode = 200; // A success response
```

3.   
   **Setting Response Headers**:  
    You can set headers (like `Content-Type`, `Content-Length`, etc.) using `res.setHeader()`.

```javascript
res.setHeader('Content-Type', 'text/plain');
```

4.   
   **Sending Response**:  
    The `res.end()` method is used to send the response body and close the connection.

```javascript
res.end('Hello, World!');
```

---

# Handling Different Routes

To respond differently based on the URL or HTTP method, you can inspect the `req` object. For example:

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  const url = req.url; // URL of the request
  const method = req.method; // HTTP method (GET, POST, etc.)

  res.setHeader('Content-Type', 'text/plain');
  
  if (url === '/' && method === 'GET') {
    res.statusCode = 200;
    res.end('Welcome to the home page!');
  } else if (url === '/about' && method === 'GET') {
    res.statusCode = 200;
    res.end('This is the about page!');
  } else {
    res.statusCode = 404;
    res.end('Page not found');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

# Explanation:

* **`req.url`**: This gives the requested URL (e.g., `/about`, `/home`).

* **`req.method`**: This gives the HTTP method (e.g., GET, POST).

* Depending on the URL and HTTP method, you can return different responses.

---

# Handling Query Parameters

If you want to handle query parameters, you can use the `url` module to parse the URL and extract the query string.

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const query = parsedUrl.query; // Extract query parameters as an object
  
  res.setHeader('Content-Type', 'text/plain');
  res.statusCode = 200;

  if (parsedUrl.pathname === '/greet') {
    const name = query.name || 'Guest';
    res.end(`Hello, ${name}!`);
  } else {
    res.statusCode = 404;
    res.end('Page not found');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

With this, if you visit `http://localhost:3000/greet?name=Alice`, the response will be "Hello, Alice\!".

---

# Summary

* `http.createServer()` creates a basic HTTP server.

* The server listens for requests, processes them, and responds.

* You can handle routing, query parameters, and HTTP methods using `req.url`, `req.method`, and `url.parse()`.

* The server can run on a specific port with `server.listen()`.

Let me know if you need help with a more complex HTTP server setup or adding additional features like handling POST requests.

