---
tags:
  - FORMAT
  - NodeJs
---
Sure\! In an HTTP server, you can work with **query parameters** and **request bodies** to handle incoming data from the client. Here's a breakdown of both:

# Query Parameters

Query parameters are parts of the URL that provide additional information to the server. They are commonly used in `GET` requests to filter or modify the response.

## Structure of a URL with Query Parameters:

```
http://localhost:3000/search?name=John&age=30
```

In this URL:

* **Path**: `/search`

* **Query parameters**: `name=John` and `age=30`

The query parameters appear after the `?` symbol and are separated by `&`.

# Working with Query Parameters in Node.js

To work with query parameters, we can use the `url` module to parse the URL and extract the query string.

## Example: Handling Query Parameters

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true); // Parse the URL and query string
  const queryParams = parsedUrl.query; // Access the query parameters as an object
  
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  
  if (parsedUrl.pathname === '/greet') {
    const name = queryParams.name || 'Guest';  // Default to 'Guest' if no name
    const age = queryParams.age || 'unknown';  // Default to 'unknown' if no age
    res.end(`Hello, ${name}! You are ${age} years old.`);
  } else {
    res.statusCode = 404;
    res.end('Page not found');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

## When you visit:

```
http://localhost:3000/greet?name=John&age=30
```

The server will respond with:

```
Hello, John! You are 30 years old.
```

If you visit:

```
http://localhost:3000/greet?name=Alice
```

The server will respond with:

```
Hello, Alice! You are unknown years old.
```

# Important Notes:

* The `url.parse()` method (with the `true` flag) automatically parses the query string into an object.

* You can access individual query parameters as properties of `parsedUrl.query`.

---

# Request Body

The **request body** is where data is sent in **POST**, **PUT**, or **PATCH** requests. This is typically used when the client sends data to the server for processing (e.g., submitting a form or sending JSON data).

* For `GET` requests, data is typically passed through query parameters.

* For `POST`, `PUT`, or `PATCH` requests, data is usually sent in the body of the request.

# Reading the Request Body

In order to read the request body, you'll have to accumulate the incoming chunks of data, especially for large requests.

## Example: Handling JSON Data in a POST Request

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.method === 'POST' && req.headers['content-type'] === 'application/json') {
    let body = '';

    // Listen for data chunks
    req.on('data', chunk => {
      body += chunk; // Append chunks to body
    });

    // Once the entire body is received
    req.on('end', () => {
      try {
        const parsedData = JSON.parse(body);  // Parse the JSON body
        console.log('Received JSON data:', parsedData);
        
        res.statusCode = 200;
        res.setHeader('Content-Type', 'application/json');
        res.end(JSON.stringify({
          message: 'Data received',
          data: parsedData
        }));
      } catch (err) {
        res.statusCode = 400;  // Bad request
        res.end('Invalid JSON data');
      }
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

## When you send a POST request with JSON data (e.g., using `curl` or Postman\`):

```
{
  "name": "John",
  "age": 30
}
```

The server will respond with:

```
{
  "message": "Data received",
  "data": {
    "name": "John",
    "age": 30
  }
}
```

## How it works:

1. **`req.on('data', chunk)`**: As the request body is sent in chunks, we collect all the data into the `body` variable.

2. **`req.on('end', ...)`**: When all the data has been received, the `end` event is triggered. At this point, we parse the body (in this case, as JSON) and send a response back.

3. **Error handling**: If the body can't be parsed into valid JSON, we send a `400` (Bad Request) error response.

---

# Parsing URL-Encoded Data (Form Data)

If the request contains form data (sent as `application/x-www-form-urlencoded`), you can parse it similarly by splitting the key-value pairs.

## Example: Handling Form Data (x-www-form-urlencoded)

```javascript
const http = require('http');
const querystring = require('querystring');

const server = http.createServer((req, res) => {
  if (req.method === 'POST' && req.headers['content-type'] === 'application/x-www-form-urlencoded') {
    let body = '';

    req.on('data', chunk => {
      body += chunk;
    });

    req.on('end', () => {
      const parsedData = querystring.parse(body); // Parse URL-encoded data
      console.log('Received form data:', parsedData);

      res.statusCode = 200;
      res.setHeader('Content-Type', 'application/json');
      res.end(JSON.stringify({
        message: 'Form data received',
        data: parsedData
      }));
    });
  } else {
    res.statusCode = 400;
    res.end('Only POST requests with form data are accepted');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

## Example of a POST Request with Form Data:

```
name=John&age=30
```

This will be parsed and handled as:

```
{
  "message": "Form data received",
  "data": {
    "name": "John",
    "age": "30"
  }
}
```

---

# Summary of Key Concepts

* **Query Parameters** are passed in the URL (e.g., `/greet?name=John`). You can use `url.parse()` to parse the query string and access it as an object.

* **Request Body** is used for POST, PUT, or PATCH requests to send data. It can be in various formats (e.g., JSON, URL-encoded). You can collect the body in chunks and parse it once fully received.

* For **JSON data**, use `JSON.parse()` to parse the body.

* For **URL-encoded form data**, use `querystring.parse()` to parse the body.

Let me know if you need help with more complex data handling or have any specific use cases\!

