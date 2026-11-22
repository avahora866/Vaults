---
tags:
  - FORMAT
  - JavaScript
---
# Handling User Input in JavaScript (`value`, `focus`, `blur`)

User input is essential in web applications, and JavaScript provides various ways to handle it using properties and event listeners. The most common properties and events include:

* **`value`** – Retrieves or sets the input field’s value.

* **`focus`** – Moves the cursor to an input field.

* **`blur`** – Detects when an input field loses focus.

---

## Getting and Setting Input Value (`value`)

The `value` property allows you to read or update the value of an input field.

### Example: Get Input Value

```
<input type="text" id="nameInput" placeholder="Enter your name">
<button onclick="showValue()">Submit</button>
<p id="output"></p>
```

```javascript
function showValue() {
  const inputValue = document.getElementById("nameInput").value;
  document.getElementById("output").textContent = "Hello, " + inputValue;
}
```

When the button is clicked, it displays the entered text.

### Example: Set Input Value

```javascript
document.getElementById("nameInput").value = "Default Name";
```

This sets a default value in the input field.

---

## Handling Focus (`focus`)

The `focus()` method moves the cursor to an input field automatically.

### Example: Auto-focus on Page Load

```
<input type="text" id="username" placeholder="Enter username">
```

```javascript
window.onload = function() {
  document.getElementById("username").focus();
};
```

When the page loads, the cursor is placed inside the input field.

### Example: Focus on Button Click

```javascript
function focusInput() {
  document.getElementById("username").focus();
}
```

Calling `focusInput()` places the cursor inside the input field.

---

## Handling Blur (`blur`)

The `blur()` method detects when an input field loses focus (user clicks away).

### Example: Change Border Color on Blur

```
<input type="text" id="email" placeholder="Enter email">
```

```javascript
document.getElementById("email").addEventListener("blur", function() {
  this.style.border = "2px solid red";
});
```

When the user clicks away, the input field gets a red border.

---

## Combining Focus and Blur

```
<input type="text" id="userInput" placeholder="Type something">
```

```javascript
const inputField = document.getElementById("userInput");

inputField.addEventListener("focus", function() {
  this.style.backgroundColor = "lightyellow";
});

inputField.addEventListener("blur", function() {
  this.style.backgroundColor = "white";
});
```

When the user **clicks inside**, the background turns **light yellow**. When they **click outside**, it turns **white** again.

---

## Summary of Methods

| Property/Method | Description | Example |
| ----- | ----- | ----- |
| `value` | Gets or sets input field value | `input.value = "New text";` |
| `focus()` | Moves cursor to the input field | `input.focus();` |
| `blur()` | Detects when input loses focus | `input.blur();` |

---

## Key Takeaways

* Use **`value`** to get and set user input.

* Use **`focus()`** to move the cursor to an input field automatically.

* Use **`blur()`** to trigger actions when the user leaves an input field.

These methods help create interactive and user-friendly web forms.

