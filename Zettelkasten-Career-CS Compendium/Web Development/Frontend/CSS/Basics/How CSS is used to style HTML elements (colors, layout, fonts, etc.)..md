---
tags:
  - FORMAT
  - CSS
---
CSS is used to style HTML elements by applying various rules and properties. These rules target specific HTML elements and define how they should appear on the webpage. Below are examples of how CSS can style HTML elements, focusing on colors, layout, fonts, and other aspects.

# Colors

CSS allows you to set colors for text, backgrounds, borders, etc., using color names, hexadecimal values, RGB, or HSL.

**Example**:

```
/* Set text color */
p {
    color: blue;
}

/* Set background color */
body {
  background-color: #f0f0f0; / light gray /
}

/* Set border color */
div {
    border: 2px solid rgb(255, 0, 0); /* red */
}
```

# Fonts

CSS is used to style text by setting properties like font family, size, weight, and line height.

**Example**:

```
/* Set font family */
h1 {
    font-family: 'Arial', sans-serif;
}

/* Set font size and weight */
p {
    font-size: 18px;
    font-weight: bold;
}

/* Set line height */
p {
    line-height: 1.5;
}
```

# Text Alignment and Decoration

CSS can align text, underline it, or apply other effects like strikethrough.

**Example**:

```
/* Align text to the center */
h1 {
    text-align: center;
}

/* Add underline to links */
a {
    text-decoration: underline;
}

/* Remove underline from links when hovered */
a:hover {
    text-decoration: none;
}
```

# Layout and Positioning

CSS is used to control the position of elements on the page. You can use properties like `margin`, `padding`, `display`, and positioning techniques such as Flexbox or Grid.

**Example (Margins, Padding, and Box Model)**:

```
/* Set margin around an element */
div {
    margin: 20px;
}

/* Set padding inside an element */
p {
    padding: 10px;
}
```

**Example (Positioning)**:

```
/* Position an element absolutely */
div {
    position: absolute;
    top: 50px;
    left: 100px;
}

/* Center a div horizontally and vertically using Flexbox */
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
```

**Example (Flexbox Layout)**:

```
.container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.item {
    flex: 1;
}
```

# Borders

CSS allows you to define borders around elements with specific thickness, style, and color.

**Example**:

```
/* Set a solid red border */
div {
    border: 2px solid red;
}

/* Set a dotted border */
p {
    border: 1px dotted blue;
}

/* Set rounded corners */
img {
    border-radius: 10px;
}
```

# Backgrounds

CSS provides several options for setting background images, colors, and patterns.

**Example**:

```
/* Set a solid background color */
body {
    background-color: lightblue;
}

/* Set a background image */
div {
    background-image: url('image.jpg');
    background-size: cover;
}

/* Set a gradient background */
header {
    background: linear-gradient(to right, red, yellow);
}
```

# Hover Effects

CSS allows you to define changes in appearance when a user hovers over an element.

**Example**:

```
/* Change background color on hover */
button:hover {
    background-color: green;
}

/* Change text color on hover */
a:hover {
    color: orange;
}
```

# Transitions and Animations

You can add smooth transitions between property changes or animate elements for dynamic effects.

**Example (Transition)**:

```
/* Apply transition effect to background color */
button {
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: red;
}
```

**Example (Animation)**:

```
/* Keyframes animation */
@keyframes example {
    0% { background-color: red; }
    50% { background-color: yellow; }
    100% { background-color: green; }
}

/* Apply animation */
div {
    animation: example 4s infinite;
}
```

These are just some of the fundamental ways CSS is used to style HTML elements. By combining these properties and techniques, web designers can create visually rich and well-organized web pages that respond to different devices and user interactions.

