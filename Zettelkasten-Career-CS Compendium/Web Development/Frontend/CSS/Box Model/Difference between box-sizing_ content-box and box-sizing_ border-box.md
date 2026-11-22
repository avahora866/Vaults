---
tags:
  - FORMAT
  - CSS
---
The `box-sizing` property in CSS determines how the width and height of an element are calculated, specifically whether or not padding and borders are included in those calculations. The two most common values for `box-sizing` are:

1. **`content-box`** (default)

2. **`border-box`**

# `box-sizing: content-box` (default)

* **What it does**: When `box-sizing` is set to `content-box` (the default value), the **width** and **height** you specify only apply to the content area of the element, **not including padding and border**.

* **How it works**:

  * The **content area** will have the specified width and height.

  * Padding and borders are **added** to the width and height you specify, increasing the total size of the element.

* **Example**:

```
div {
    width: 200px;   /* Content width */
    height: 100px;  /* Content height */
    padding: 20px;  /* Padding around content */
    border: 5px solid black;  /* Border around padding */
}
```

  *   
    The **content area** will be **200px** by **100px**.

  * The **total width** will be the content width (`200px`) plus the left and right padding (`20px + 20px = 40px`) and the left and right border (`5px + 5px = 10px`):

    * **Total width** \= `200px (content) + 40px (padding) + 10px (border)` \= **250px**.

  * Similarly, the **total height** will be:

    * **Total height** \= `100px (content) + 40px (padding) + 10px (border)` \= **150px**.

# `box-sizing: border-box`

* **What it does**: When `box-sizing` is set to `border-box`, the **width** and **height** you specify include the content, padding, and border. This means that the padding and border are **not added** to the total size of the element. The total size is fixed, and the content area shrinks to accommodate padding and borders within the defined width and height.

* **How it works**:

  * The **content area** adjusts so that the **total size** of the element, including padding and borders, matches the width and height you specify.

  * Padding and borders **do not increase** the overall width and height of the element.

* **Example**:

```
div {
    width: 200px;   /* Total width */
    height: 100px;  /* Total height */
    padding: 20px;  /* Padding around content */
    border: 5px solid black;  /* Border around padding */
    box-sizing: border-box;  /* Includes padding and border in width/height */
}
```

  *   
    The **total width** will be **200px** (including padding and border).

  * The **content area** will shrink to fit within the total width, so:

    * **Content width** \= `200px (total width) - 40px (padding) - 10px (border)` \= **150px**.

  * Similarly, the **content height** will shrink:

    * **Content height** \= `100px (total height) - 40px (padding) - 10px (border)` \= **50px**.

---

# Visual Comparison:

## `content-box`:

```
| Margin | Border | Padding | Content Area |
|--------|--------|---------|--------------|
|        |        | 20px    | 200px        |
|        |        |         |              |
|        |--------|---------|--------------|
|        | 5px    |         |              |
|        |        |         |              |
|--------|--------|---------|--------------|
```

*   
  The total size will be the content width (`200px`) plus the padding (`40px`) and border (`10px`), resulting in a total of **250px by 150px**.

## `border-box`:

```
| Margin | Border | Padding | Content Area |
|--------|--------|---------|--------------|
|        | 5px    | 20px    | 150px        |
|        |        |         |              |
|        |--------|---------|--------------|
|        |        |         |              |
|--------|--------|---------|--------------|
```

*   
  The total size is **200px by 100px** (as specified), including padding and border. The **content area** shrinks to fit within the total size.

---

# When to Use `content-box` vs. `border-box`:

1. **`content-box`**:

   * This is the default behavior. It is useful when you want the width and height to refer only to the content, and you want padding and borders to be outside the element’s size.

   * Common when you need precise control over the content area (e.g., a fixed content size).

2. **`border-box`**:

   * This is more commonly used in modern web design, especially for layouts, as it makes it easier to control the overall size of an element without worrying about padding and border pushing the element's size beyond what you specify.

   * Great for responsive design, where you want elements to stay within a specific size regardless of padding and borders.

---

# Example of Using `border-box` for Layouts:

You can apply `box-sizing: border-box` globally for all elements to simplify layout calculations:

```
* {
    box-sizing: border-box; /* Apply border-box to all elements */
}
```

This means that when you set the width and height of an element, the padding and borders will already be included in those dimensions, and you won't need to manually account for them. This helps prevent layout issues, especially when dealing with padding and borders in container elements.

---

# Summary of Differences:

* **`content-box`**:

  * The `width` and `height` apply only to the content area.

  * Padding and borders are added outside the content area, increasing the total size of the element.

* **`border-box`**:

  * The `width` and `height` include the content, padding, and border.

  * The total size of the element remains fixed, and the content area shrinks to accommodate padding and borders within the defined width and height.

By understanding the difference between `content-box` and `border-box`, you can make better decisions on how to structure and size your elements, especially when building responsive layouts.

