---
tags:
  - FORMAT
  - JavaScript
---
# Window and Document Objects in JavaScript

In JavaScript, the **`window`** and **`document`** objects are fundamental to interacting with the browser and manipulating web pages. Understanding these objects is crucial for DOM manipulation and browser interaction.

---

## The `window` Object

The `window` object represents the **browser window** and serves as the global object for JavaScript. It provides access to the browser’s environment, including the document, alert dialogs, timers, and more.

### Properties of the `window` Object

* **`window.document`**: Refers to the `document` object, which represents the web page content.

* **`window.innerHeight`**: Returns the inner height of the browser window (excluding toolbars).

* **`window.innerWidth`**: Returns the inner width of the browser window.

* **`window.location`**: Represents the current URL of the page.

* **`window.localStorage`**: Provides access to the browser's local storage for saving data.

* **`window.alert()`**: Displays an alert dialog box with a specified message.

* **`window.setTimeout()`** and **`window.setInterval()`**: Methods to set timeouts and intervals.

### Example: Using the `window` Object

```javascript
// Accessing the window's width and height
console.log(window.innerWidth);  // Outputs the width of the browser window
console.log(window.innerHeight); // Outputs the height of the browser window

// Changing the current URL
window.location.href = "https://www.example.com";  // Navigates to another webpage

// Using setTimeout() to execute a function after 2 seconds
window.setTimeout(() => {
  console.log("This runs after 2 seconds.");
}, 2000);
```

The `window` object acts as the **global object** in JavaScript, meaning all global variables and functions are properties of `window`. For example, `alert()` is actually a method of `window`, so `window.alert()` and `alert()` are equivalent.

---

## The `document` Object

The `document` object represents the **HTML or XML document** loaded into the browser. It is the primary way to access and manipulate the content (DOM) of a webpage.

### Properties and Methods of the `document` Object

* **`document.getElementById(id)`**: Returns the element with the specified ID.

* **`document.querySelector(selector)`**: Returns the first element that matches the CSS selector.

* **`document.querySelectorAll(selector)`**: Returns all elements that match the CSS selector.

* **`document.createElement(tagName)`**: Creates a new HTML element.

* **`document.body`**: Represents the `<body>` element of the document.

* **`document.title`**: Gets or sets the title of the document (the text inside the `<title>` tag).

* **`document.forms`**: Accesses all the forms in the document.

* **`document.cookie`**: Allows reading and writing cookies associated with the page.

### Example: Accessing and Manipulating HTML Elements

```
<!DOCTYPE html>
<html>
  <head>
    <title>Document Object Example</title>
  </head>
  <body>
    <h1 id="header">Hello, World!</h1>
    <button onclick="changeText()">Change Text</button>

    <script>
      function changeText() {
        const header = document.getElementById("header");
        header.textContent = "Text Changed!";
      }
    </script>
  </body>
</html>
```

*   
  In this example, when the user clicks the button, the text content of the `<h1>` element is changed via `document.getElementById()`.

### Example: Creating and Adding New Elements

```javascript
const newDiv = document.createElement("div");
newDiv.textContent = "This is a new div.";
document.body.appendChild(newDiv);  // Appends the new div to the body
```

*   
  Here, a new `<div>` element is created and added to the document’s body.

---

## Key Differences Between `window` and `document` Objects

| Feature | `window` Object | `document` Object |
| ----- | ----- | ----- |
| **Represents** | The browser window and environment | The HTML content of the page |
| **Global Object** | Yes, it is the global object in JavaScript | No, it is a property of the `window` object |
| **Main Use** | Interacting with the browser (e.g., navigation, alerts, timers) | Accessing and manipulating the DOM (e.g., HTML elements, events) |
| **Common Methods** | `window.alert()`, `window.setTimeout()`, `window.location` | `document.getElementById()`, `document.createElement()`, `document.querySelector()` |
| **Properties** | `window.innerWidth`, `window.localStorage` | `document.body`, `document.title` |

---

## Key Takeaways

* The **`window` object** is the global object representing the browser environment. It provides methods for interacting with the browser window and the web page.

* The **`document` object** is used to interact with the DOM, enabling you to access and modify HTML elements and content within the web page.

* Both objects are essential for web development, with **`window`** for browser-specific functionality and **`document`** for DOM manipulation.

