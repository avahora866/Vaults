---
tags:
  - FORMAT
  - JavaScript
---
# Modifying Elements in JavaScript (`textContent`, `innerHTML`, `classList`)

JavaScript allows you to **dynamically change HTML elements** using different properties and methods. The most common ways to modify elements are:

1. **`textContent`** – Change text inside an element.

2. **`innerHTML`** – Change HTML content inside an element.

3. **`classList`** – Add, remove, toggle, or replace CSS classes.

---

## 1️⃣ Changing Text with `textContent`

The **`textContent`** property sets or gets only the **text inside an element**.  
 🔹 It does **not** interpret HTML.  
 🔹 It is **faster and safer** than `innerHTML`.

### Example: Changing Text

```
<p id="myText">Hello, World!</p>
<button onclick="changeText()">Change Text</button>
```

```javascript
function changeText() {
  document.getElementById("myText").textContent = "New text!";
}
```

✔ The paragraph text **changes to "New text\!"** when the button is clicked.

---

## 2️⃣ Changing HTML with `innerHTML`

The **`innerHTML`** property sets or gets the **HTML inside an element**.  
 🔹 It **interprets HTML tags** and renders them.  
 🔹 It can be a **security risk** (if using untrusted input).

### Example: Adding HTML Content

```
<div id="content">Old Content</div>
<button onclick="changeHTML()">Change Content</button>
```

```javascript
function changeHTML() {
  document.getElementById("content").innerHTML = "<b>New Content!</b>";
}
```

✔ The `<div>` will display **bold text**: **New Content\!**

---

## 3️⃣ Modifying Classes with `classList`

The **`classList`** property allows **adding, removing, toggling, or replacing** CSS classes dynamically.

### Example: Toggle a Class

```
<p id="text" class="blue">This is a paragraph.</p>
<button onclick="toggleClass()">Toggle Color</button>
<style>
  .blue { color: blue; }
  .red { color: red; }
</style>
```

```javascript
function toggleClass() {
  document.getElementById("text").classList.toggle("red");
}
```

✔ The paragraph **toggles between blue and red** when the button is clicked.

---

## 4️⃣ Full `classList` Methods

| Method | Description | Example |
| ----- | ----- | ----- |
| `.add("className")` | Adds a class | `element.classList.add("highlight");` |
| `.remove("className")` | Removes a class | `element.classList.remove("hidden");` |
| `.toggle("className")` | Adds class if missing, removes if present | `element.classList.toggle("dark-mode");` |
| `.contains("className")` | Checks if class exists | `element.classList.contains("active");` |
| `.replace("old", "new")` | Replaces a class | `element.classList.replace("oldClass", "newClass");` |

---

## 5️⃣ `textContent` vs `innerHTML` – Key Differences

| Feature | `textContent` | `innerHTML` |
| ----- | ----- | ----- |
| **Modifies text** | ✅ Yes | ✅ Yes |
| **Modifies HTML** | ❌ No | ✅ Yes |
| **Safer?** | ✅ Yes | ❌ No (can allow XSS attacks) |
| **Faster?** | ✅ Yes | ❌ No (parses HTML) |

---

## 6️⃣ Summary

✔ **Use `textContent`** when modifying **only text** (safer and faster).  
 ✔ **Use `innerHTML`** when modifying **HTML content** (but be cautious).  
 ✔ **Use `classList`** to dynamically change **CSS styles**.

💡 **Mastering these methods allows you to dynamically update web pages and create interactive experiences\!** 🚀

