---
tags:
  - FORMAT
  - NodeJs
---
Sure\! In an HTTP server, handling requests and responses is central to interacting with clients (usually web browsers or other HTTP clients). Node.js provides the `http` module to manage this, and the `request` (`req`) and `response` (`res`) objects are key to handling HTTP requests and responses.

# The Request (`req`) Object

The `req` object contains information about the incoming request, such as the request URL, HTTP method, headers, query parameters, and more.

## Key Properties of `req`:

* **`req.url`**: The URL part of the incoming request, which can include the path and query string.

* **`req.method`**: The HTTP method (e.g., `GET`, `POST`, `PUT`, `DELETE`).

* **`req.headers`**: An object containing the headers of the request.

* **`req.query`**: An object that contains the query string parameters if the URL includes any (this is more easily accessed with a package like `querystring` or `url`).

* **`req.body`**: Contains the data sent in the body of the request (useful in `POST` or `PUT` requests).

## Example: Accessing Request Data

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  console.log(`Request URL: ${req.url}`);
  console.log(`Request Method: ${req.method}`);
  console.log(`Request Headers:`, req.headers);

  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Request received');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

---

# The Response (`res`) Object

The `res` object is used to send the response back to the client. It contains methods for setting the HTTP status code, headers, and body content.

## Key Methods of `res`:

* **`res.statusCode`**: Sets the HTTP status code of the response (e.g., `200`, `404`).

* **`res.setHeader(name, value)`**: Sets a specific header in the response.

* **`res.write(data)`**: Sends part of the response body (can be used to send multiple chunks of data).

* **`res.end(data)`**: Finalizes the response and sends the content to the client. If you pass data, it will be the body of the response.

## Example: Sending a Response

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, world!');  // Send response body
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

---

# Handling Different HTTP Methods

You can handle different HTTP methods (`GET`, `POST`, `PUT`, etc.) to perform different actions based on the request method.

## Example: Handling `GET` and `POST` Requests

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // Handling GET request
  if (req.method === 'GET') {
    if (req.url === '/') {
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('Welcome to the home page!');
    } else if (req.url === '/about') {
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('This is the about page.');
    } else {
      res.statusCode = 404;
      res.end('Page not found');
    }
  }

  // Handling POST request
  else if (req.method === 'POST') {
    let body = '';

    req.on('data', chunk => {
      body += chunk; // Collecting chunks of data from request body
    });

    req.on('end', () => {
      console.log('Received POST data:', body);
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('Data received');
    });
  } else {
    res.statusCode = 405;  // Method Not Allowed
    res.end('Method Not Allowed');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

---

# Handling Query Parameters

You can extract query parameters from the URL, especially useful for `GET` requests.

## Example: Handling Query Parameters

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);  // Parse the URL and query string
  const queryParams = parsedUrl.query; // Access query parameters as an object

  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');

  if (parsedUrl.pathname === '/greet') {
    const name = queryParams.name || 'Guest';  // Default to 'Guest' if no 'name' param
    res.end(`Hello, ${name}!`);
  } else {
    res.statusCode = 404;
    res.end('Page not found');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

When you visit `http://localhost:3000/greet?name=Alice`, the response will be `"Hello, Alice!"`.

---

# Handling POST Data (Parsing JSON, Form Data)

For `POST` requests, data can be sent in the body (e.g., JSON, form data). You need to read the incoming data and handle it.

## Example: Parsing JSON Data in POST Request

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.method === 'POST' && req.headers['content-type'] === 'application/json') {
    let body = '';

    req.on('data', chunk => {
      body += chunk;
    });

    req.on('end', () => {
      const parsedData = JSON.parse(body);  // Parse JSON data from the body
      console.log('Received data:', parsedData);

      res.statusCode = 200;
      res.setHeader('Content-Type', 'application/json');
      res.end(JSON.stringify({ message: 'Data received', data: parsedData }));
    });
  } else {
    res.statusCode = 400;  // Bad request
    res.end('Only POST requests with JSON data are accepted');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

This server only accepts `POST` requests with JSON data.

---

# Sending Different Content Types

You can set the `Content-Type` header to indicate the type of data you’re sending, whether it’s plain text, HTML, JSON, or something else.

## Example: Sending HTML Response

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/html');
  res.end('<h1>Hello, this is an HTML response!</h1>');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

---

# Summary

* The `req` object represents the incoming request and provides information like URL, HTTP method, headers, and body.

* The `res` object represents the outgoing response and is used to send back the HTTP status, headers, and body.

* Different HTTP methods (e.g., `GET`, `POST`) can be handled to perform different actions.

* Query parameters, request bodies, and content types can be handled to provide more dynamic responses.

Let me know if you want more examples or help with specific types of requests\!

