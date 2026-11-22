---
tags:
  - FORMAT
  - CSS
---
In CSS, **floats** were historically one of the primary ways to create layouts by positioning elements to the left or right of their container. While modern layout techniques such as Flexbox and Grid have largely replaced floats for layout purposes, understanding how to use **float** and **clear** is still important, especially for older projects or when you need to work with legacy code.

# How Floats Work:

The `float` property allows you to push an element to the left or right, allowing other content to flow around it. Floats were originally designed for wrapping text around images, but they are often used for creating column layouts.

## Float Property:

* `float: left;` — Floats the element to the left, allowing content to wrap around it.

* `float: right;` — Floats the element to the right, allowing content to wrap around it.

* `float: none;` (default) — Removes the float and allows the element to behave like a block-level element.

---

# Basic Example of Float (Text Wrap Around Image):

```
<div class="container">
    <img src="image.jpg" alt="Example Image" class="float-left">
    <p>This paragraph will wrap around the image because the image is floated to the left.</p>
</div>
```

```
.float-left {
    float: left;
    margin-right: 20px;  /* Add space between the image and text */
}
```

*   
  The image will float to the left of the container, and the text will wrap around it.

# Creating Multi-Column Layouts with Floats:

You can use floats to create basic multi-column layouts by floating the column elements to the left or right.

## Example: Two-Column Layout with Floats:

```
<div class="container">
    <div class="left-column">
        <p>Left column content</p>
    </div>
    <div class="right-column">
        <p>Right column content</p>
    </div>
</div>
```

```
.container {
    width: 100%;
}

.left-column {
    float: left;
    width: 50%;
    background-color: lightblue;
    padding: 10px;
}

.right-column {
    float: left;
    width: 50%;
    background-color: lightgreen;
    padding: 10px;
}
```

*   
  **`float: left;`** is applied to both columns, causing them to appear side by side (like floating left).

* Each column has a width of **50%**, so they each take up half the container's width.

---

# Clearing Floats:

When you float an element, it is removed from the normal document flow, meaning the container element may collapse if it doesn't contain any non-floating content. To fix this, you need to **clear the float**.

## Using `clear` Property:

* **`clear: left;`** — Prevents any floating elements on the left from affecting the element.

* **`clear: right;`** — Prevents any floating elements on the right from affecting the element.

* **`clear: both;`** — Prevents floating elements on both sides from affecting the element.

## Clearfix Hack:

To ensure the container "wraps" around floated elements, you can apply a clearfix method.

```
.container::after {
    content: "";
    display: block;
    clear: both;
}
```

The `::after` pseudo-element is added to the container and uses `clear: both;` to clear any floated elements inside it, ensuring the container's height expands to accommodate floated content.

# Clearing Floats Example:

```
<div class="container">
    <div class="left-column">
        <p>Left column content</p>
    </div>
    <div class="right-column">
        <p>Right column content</p>
    </div>
    <div class="clearfix"></div> <!-- Clear float -->
</div>
```

```
.container {
    width: 100%;
}

.left-column {
    float: left;
    width: 50%;
    background-color: lightblue;
    padding: 10px;
}

.right-column {
    float: left;
    width: 50%;
    background-color: lightgreen;
    padding: 10px;
}

.clearfix {
    clear: both;
}
```

Here, the `.clearfix` class is used to clear the floats, ensuring that the container properly expands around the floated columns.

---

# Common Issues with Floats:

1. **Collapsed Containers**: When you float elements, the container may collapse, as it no longer considers the floated elements in its height calculation. Use the clearfix hack to fix this issue.

2. **Overlapping Content**: If the floated elements exceed the container's width, they might overlap. Always ensure you properly manage widths and use the `clear` property where needed.

---

# Summary:

* **Floats** are used to position elements to the left or right and allow content to flow around them.

* The **`float`** property can be used for basic layouts like multi-column designs.

* **Clearing floats** is important to ensure the container properly wraps around floated content. Use the `clear` property or the clearfix hack to fix layout issues.

While floats are useful, modern layout techniques like **Flexbox** and **CSS Grid** are more powerful and flexible for creating complex layouts, but understanding floats is still important for maintaining and working with older code.

