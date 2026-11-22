---
tags:
  - FORMAT
  - CSS
---
To manipulate each part of the **CSS Box Model**, you can use specific properties to control the size and space of the content area, padding, border, and margin. These properties allow you to adjust the layout of elements and ensure proper spacing between them.

# Properties to Manipulate the Box Model:

1. **Content Area (Width and Height)**

   * The `width` and `height` properties control the **content area** of an element. These properties only apply to the content, not including padding, border, or margin.

2. **Example:**

```
div {
    width: 300px;   /* Content width */
    height: 150px;  /* Content height */
}
```

3.   
   **Padding**

   * Padding is the space between the content and the border. It can be adjusted using the `padding` property. You can specify padding for all sides at once, or use individual properties for each side (`padding-top`, `padding-right`, `padding-bottom`, `padding-left`).

4. **Example (all sides):**

```
div {
    padding: 20px;  /* 20px of padding on all sides */
}
```

5.   
   **Example (individual sides):**

```
div {
    padding-top: 10px;
    padding-right: 15px;
    padding-bottom: 20px;
    padding-left: 25px;
}
```

   *   
     Padding increases the size of the element’s content area by adding space around the content.

6. **Border**

   * Borders are the lines surrounding the padding and content area. You can manipulate the **border width**, **border style**, and **border color** using the `border` property or individual border properties (`border-top`, `border-right`, `border-bottom`, `border-left`).

7. **Example (all sides):**

```
div {
    border: 5px solid black;  /* 5px wide, solid, black border on all sides */
}
```

8.   
   **Example (individual sides):**

```
div {
    border-top: 3px dashed blue;
    border-right: 2px solid green;
    border-bottom: 4px dotted red;
    border-left: 1px solid black;
}
```

   *   
     The **border** property increases the overall size of the element by adding space between the content and the margin.

9. **Margin**

   * Margins create space between the element and its surrounding elements or the edge of its container. You can control the **margin** for all sides at once or individually (`margin-top`, `margin-right`, `margin-bottom`, `margin-left`).

10. **Example (all sides):**

```
div {
    margin: 20px;  /* 20px margin on all sides */
}
```

11.   
    **Example (individual sides):**

```
div {
    margin-top: 10px;
    margin-right: 15px;
    margin-bottom: 20px;
    margin-left: 25px;
}
```

    *   
      **Margins** do not affect the element's size but change the spacing between elements and their surroundings.

---

# How to Adjust Box Model Parts in Practice

Let’s explore how you can manipulate the box model components to control element layout and spacing.

---

# Example 1: Manipulating the Box Model with Padding, Border, and Margin

```
div {
    width: 300px;        /* Content width */
    height: 150px;       /* Content height */
    padding: 20px;       /* Padding inside the content area */
    border: 5px solid black; /* Border around the padding and content */
    margin: 10px;        /* Margin outside the border */
}
```

*   
  **Content area** will be `300px` by `150px`.

* **Padding** adds 20px space around the content, making the total width and height:

  * Width \= `300px (content) + 20px (padding left) + 20px (padding right) = 340px`

  * Height \= `150px (content) + 20px (padding top) + 20px (padding bottom) = 190px`

* **Border** adds 5px around the element, so the total width and height becomes:

  * Width \= `340px (width + padding) + 5px (border left) + 5px (border right) = 350px`

  * Height \= `190px (height + padding) + 5px (border top) + 5px (border bottom) = 200px`

* **Margin** creates space outside the element, but it does not affect the total size of the element itself (it affects positioning).

**Total size**:

* Width \= 350px

* Height \= 200px

---

# Example 2: Using `box-sizing` to Control the Element's Size

By default, the `width` and `height` properties refer to the **content area** only, but with the `box-sizing: border-box` property, the total size (content \+ padding \+ border) is taken into account when you define `width` and `height`.

## Example with `box-sizing: border-box`:

```
div {
    width: 300px;         /* Content width, but total width is 300px including padding and border */
    height: 150px;        /* Content height */
    padding: 20px;        /* Padding inside the content area */
    border: 5px solid black; /* Border around the padding and content */
    margin: 10px;         /* Margin outside the border */
    box-sizing: border-box;  /* Ensures the total size of the element includes padding and border */
}
```

*   
  With `box-sizing: border-box`, the total width and height of the element will be exactly `300px` by `150px`, even including the padding and border. The content area will shrink to fit within the defined width and height.

**Total size**:

* Width \= 300px (including padding and border)

* Height \= 150px (including padding and border)

This is useful when you want to control the total size of an element without worrying about padding and borders causing the element to be larger than intended.

---

# Example 3: Manipulating Individual Box Model Parts

Here’s an example where we manipulate padding, border, margin, and width/height individually to understand their effect:

```
div {
    width: 250px;         /* Content width */
    height: 120px;        /* Content height */
    padding-top: 10px;    /* Padding on top */
    padding-right: 20px;  /* Padding on the right */
    padding-bottom: 10px; /* Padding on bottom */
    padding-left: 30px;   /* Padding on the left */
    border-top: 5px solid blue;  /* Border on top */
    border-right: 3px dashed green;  /* Border on right */
    border-bottom: 2px solid red;   /* Border on bottom */
    border-left: 4px dotted purple; /* Border on left */
    margin-top: 15px;     /* Margin on top */
    margin-right: 25px;   /* Margin on the right */
    margin-bottom: 20px;  /* Margin on bottom */
    margin-left: 10px;    /* Margin on the left */
}
```

*   
  **Width and height** control the content size.

* **Padding** adds space inside the content area, pushing the content inward.

* **Borders** are drawn around the padding and content, increasing the total size.

* **Margins** create space between this element and other surrounding elements, but do not change the element’s own size.

# Visual Representation:

* **Content**: 250px by 120px

* **Padding**: Adds space around the content.

* **Border**: Surrounds the padding and content.

* **Margin**: Adds space between the element and its neighboring elements.

---

# Summary of How to Manipulate the Box Model:

* **Content area**: Set using `width` and `height`.

* **Padding**: Use `padding`, `padding-top`, `padding-right`, `padding-bottom`, and `padding-left` to control space inside the element around the content.

* **Border**: Use `border`, `border-top`, `border-right`, `border-bottom`, and `border-left` to style the border around the element.

* **Margin**: Use `margin`, `margin-top`, `margin-right`, `margin-bottom`, and `margin-left` to create space outside the element between it and other elements.

By controlling these properties, you can precisely define how your elements are sized, spaced, and positioned in a layout.

