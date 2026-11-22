---
tags:
  - FORMAT
  - JavaScript
---
# Fetch API for Making HTTP Requests

The **Fetch API** is a modern, JavaScript API that allows you to make network requests to retrieve or send data to a server. It is widely used in web development for interacting with APIs, loading resources, and making asynchronous requests in a cleaner and more flexible manner compared to older methods like `XMLHttpRequest`.

---

## What is the Fetch API?

The Fetch API provides an interface for fetching resources (e.g., JSON data, HTML, images) over the network. It is based on **Promises**, making it easier to work with asynchronous operations and handle success or failure of requests in a clean and concise way.

The basic syntax of the `fetch()` function is:

```javascript
fetch(url, options)
  .then(response => response.json())  // Process the response
  .then(data => console.log(data))    // Use the data
  .catch(error => console.log(error)); // Handle errors
```

---

## Basic Usage of `fetch()`

The `fetch()` function accepts at least one argument, which is the URL of the resource you want to retrieve. Optionally, you can provide a second argument that includes various options like HTTP method, headers, body, etc.

### Basic Example: Fetching Data from an API

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json(); // Parse the response as JSON
  })
  .then(data => console.log(data)) // Handle the data
  .catch(error => console.error('There was a problem with the fetch operation:', error)); // Handle any errors
```

### Explanation:

* `fetch('https://api.example.com/data')`: Sends a GET request to the specified URL.

* `.then(response => response.json())`: Converts the response into JSON format.

* `.catch(error => ...)`: Catches and handles any errors that may occur during the fetch operation.

---

## Making Different Types of HTTP Requests with `fetch()`

By default, `fetch()` makes a **GET** request. However, you can specify different HTTP methods like **POST**, **PUT**, **DELETE**, etc., by passing an options object.

### POST Request Example: Sending Data

To send data, such as JSON, to a server, you need to specify the HTTP method (POST) and include a body with the request.

```javascript
const data = { username: 'john_doe', password: '12345' };

fetch('https://api.example.com/login', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',  // Specify the content type
  },
  body: JSON.stringify(data),  // Convert JavaScript object to JSON string
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### Explanation:

* `method: 'POST'`: Specifies that this is a POST request.

* `headers`: Specifies the content type (in this case, JSON).

* `body: JSON.stringify(data)`: Sends the data as a JSON string in the request body.

---

## Handling Response Status and Errors

When using `fetch()`, it’s important to check the **response status** to handle errors properly. The `fetch()` function does not reject on HTTP error statuses (like 404 or 500); instead, it resolves the promise normally, and you have to check the `response.ok` property to determine if the request was successful.

### Example: Checking Response Status

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Failed to fetch data');
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

*   
  `response.ok` checks if the status code is in the range 200-299, which indicates a successful response.

---

## Working with JSON Data

The Fetch API makes it easy to handle data in different formats like JSON, text, or even binary data. For JSON, you can call `.json()` on the response to parse it into a JavaScript object.

### Example: Parsing JSON Response

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())  // Parses the JSON response
  .then(data => {
    console.log(data); // Use the parsed data
  })
  .catch(error => {
    console.error('There was an error with the fetch operation:', error);
  });
```

---

## Additional Fetch Options

You can customize the request with various options, such as headers, credentials, cache settings, and more.

### Example: Custom Headers and Credentials

```javascript
fetch('https://api.example.com/data', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer your-token-here',  // Add an Authorization header
    'Accept': 'application/json'                // Specify accepted response format
  },
  credentials: 'same-origin'  // Send cookies for same-origin requests
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### Common Options in the `fetch()` Request Object:

* **method**: The HTTP method (`GET`, `POST`, `PUT`, etc.).

* **headers**: Custom headers for the request.

* **body**: The body of the request (used with `POST`, `PUT`, etc.).

* **credentials**: Controls cookies and authentication (`same-origin`, `include`, `omit`).

* **cache**: Determines how the request is cached (`default`, `no-store`, etc.).

---

## Handling Multiple Fetch Requests: Chaining and Parallel Execution

You can chain multiple `fetch()` requests together using `.then()`, or execute them in parallel using `Promise.all()`.

### Example: Chaining Multiple Requests

```javascript
fetch('https://api.example.com/user')
  .then(response => response.json())
  .then(userData => {
    return fetch(`https://api.example.com/user/${userData.id}/posts`);
  })
  .then(response => response.json())
  .then(postsData => console.log(postsData))
  .catch(error => console.error('Error:', error));
```

### Example: Parallel Requests with `Promise.all()`

```javascript
Promise.all([
  fetch('https://api.example.com/user'),
  fetch('https://api.example.com/posts')
])
  .then(responses => Promise.all(responses.map(response => response.json())))
  .then(([userData, postsData]) => {
    console.log(userData, postsData);
  })
  .catch(error => console.error('Error:', error));
```

---

## CORS (Cross-Origin Resource Sharing)

When making requests to a different domain, the browser enforces **CORS** policy, which may block the request unless the server explicitly allows cross-origin requests. You may need to handle CORS issues, typically by enabling CORS on the server or using techniques like **JSONP** or **proxy servers**.

### Example: Handling CORS Error

If a CORS error occurs, the response will not be successful, and you should handle the error gracefully in your code:

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (response.status === 200) {
      return response.json();
    }
    throw new Error('CORS Error: Access Denied');
  })
  .catch(error => console.error('Error:', error));
```

---

## Key Takeaways

* **The Fetch API** provides a simple and flexible way to make HTTP requests in JavaScript, leveraging Promises for asynchronous programming.

* It supports multiple HTTP methods (GET, POST, PUT, DELETE) and can handle response data in various formats (JSON, text, etc.).

* Use **`.then()`** for handling responses and **`.catch()`** for handling errors.

* **CORS** can restrict cross-origin requests, and you'll need to handle it if you're making requests to external APIs.

* The **`fetch()`** function makes handling requests easier and more readable compared to older techniques like `XMLHttpRequest`.

