---
tags:
  - FORMAT
  - CSS
---
Creating **responsive web designs** is an essential aspect of modern web development, and one of the most effective ways to ensure that a website looks good on all screen sizes is by using **Viewport units** like `vw` (viewport width) and `vh` (viewport height). These units allow you to create designs that dynamically adjust based on the dimensions of the **viewport** — the visible area of the browser window.

# What are Viewport Units?

**Viewport units** are relative units in CSS that allow you to size elements relative to the size of the **viewport**. The viewport is the area visible on the screen that holds the webpage content. Viewport units can help you create fluid, flexible designs that automatically adjust to different screen sizes.

There are four viewport units:

1. **vw** – **Viewport width** unit.

   * 1 `vw` \= 1% of the width of the viewport.

2. **vh** – **Viewport height** unit.

   * 1 `vh` \= 1% of the height of the viewport.

3. **vmin** – The smaller of either `vw` or `vh`.

4. **vmax** – The larger of either `vw` or `vh`.

However, for now, we'll focus on `vw` and `vh`, which are most commonly used.

---

# How Do Viewport Units Work?

* **1 vw** \= 1% of the **viewport's width**.

* **1 vh** \= 1% of the **viewport's height**.

So, if the viewport is **1000px wide** and **800px tall**:

* `1vw` \= 1% of 1000px \= **10px**.

* `1vh` \= 1% of 800px \= **8px**.

These units change dynamically as the size of the viewport changes, which makes them perfect for responsive web design.

---

# How to Use `vw` and `vh` in CSS

You can use **viewport units** in the same way you use other CSS units (like `px`, `em`, or `%`). Here are some common use cases:

# Sizing Elements with Viewport Units

Let’s start by sizing elements using `vw` and `vh`:

## Example 1: Fullscreen Section

You can create a section that fills the entire viewport using `100vw` and `100vh`:

```
.fullscreen-section {
    width: 100vw; /* 100% of the viewport width */
    height: 100vh; /* 100% of the viewport height */
    background-color: lightblue;
}
```

This section will cover the entire visible area of the browser window, regardless of its size. As the window resizes, the section will adjust to fit the new viewport dimensions.

## Example 2: Responsive Text Size

Using `vw` for text size ensures that text scales with the viewport width, making it responsive to different screen sizes:

```
h1 {
    font-size: 5vw; /* 5% of the viewport width */
}
```

In this example, the font size will increase on larger screens and shrink on smaller screens. This can be especially useful for headings or banners that need to adapt to various screen sizes.

---

# Creating Fluid Layouts

By using `vw` and `vh`, you can create layouts that adjust fluidly as the viewport changes. This is perfect for elements that need to stay proportional to the viewport’s size.

## Example 3: Responsive Grid

A grid container with flexible columns based on the viewport width:

```
.grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(20vw, 1fr)); /* Columns will be a minimum of 20vw */
    gap: 2vw;  /* The gap between grid items will be 2% of the viewport width */
}
```

In this example:

* The grid items will take up **at least 20% of the viewport width** but can expand if there’s more space.

* The gap between grid items will always be a percentage of the viewport width, ensuring responsiveness.

---

# Using Viewport Units for Margins and Padding

You can also use viewport units for spacing, which will make the spacing responsive based on the viewport size.

## Example 4: Dynamic Margin and Padding

Here’s an example of using `vw` for margin and `vh` for padding:

```
.container {
    margin: 5vw; /* Margin is 5% of the viewport width */
    padding: 5vh; /* Padding is 5% of the viewport height */
}
```

As the viewport changes, both the margin and padding will adjust relative to the size of the viewport, creating a fluid layout that maintains proportional spacing.

---

# Combining `vw` and `vh`

Sometimes, you might want to use both viewport width (`vw`) and viewport height (`vh`) together, especially if you're designing full-screen elements that need to take into account both width and height proportions.

## Example 5: Responsive Background Image Size

You can make a background image responsive by using both `vw` and `vh`:

```
.hero {
    background-image: url('hero.jpg');
    background-size: 100vw 100vh; /* The background image will cover the entire viewport */
    height: 100vh;
}
```

This makes the background image scale to fill the entire width and height of the viewport, adjusting dynamically as the user resizes their browser window.

---

# Using Viewport Units for Responsive Design

The combination of `vw` and `vh` can help create designs that are not only flexible but also highly adaptive to different screen sizes. For example:

## Example 6: Responsive Card Layout

```
.card {
    width: 30vw;  /* 30% of the viewport width */
    height: 40vh; /* 40% of the viewport height */
    margin: 2vw;  /* Margin based on the viewport width */
    background-color: lightgray;
}
```

In this case, each card will adjust its size based on the **width** and **height** of the viewport, creating a responsive layout for a collection of cards. As the viewport size changes, the cards resize proportionally.

---

# Advantages of Using Viewport Units

1. **Responsive Layouts**: Viewport units allow you to create layouts that automatically adjust to different screen sizes, making it easier to design for mobile, tablet, and desktop.

2. **Fluidity**: Viewport units help elements adjust fluidly to the screen dimensions without needing to rely on media queries in some cases. This is particularly useful for full-screen elements or content that needs to scale with the viewport.

3. **Consistency Across Devices**: Viewport units offer consistency in how elements appear across various devices because they are directly tied to the viewport's dimensions.

---

# Best Practices for Using Viewport Units

* **Combine with Other Units**: While `vw` and `vh` are useful for creating responsive layouts, they are often best combined with other units like `%`, `em`, and `px` to fine-tune the design. For example, you might use `vw` for font sizing but fall back on `rem` for certain spacing to keep the design consistent.

* **Avoid Extreme Values**: Be cautious about using extreme `vw` or `vh` values (e.g., 100vw or 100vh) for all elements. These might cause scrollbars or clipping on some devices or screen sizes, especially when combined with padding or margins.

* **Test Across Devices**: As with any responsive design, it’s important to test how elements behave across multiple screen sizes. Ensure that the design remains usable and accessible on all devices.

---

# Conclusion:

* **Viewport units** (`vw` and `vh`) allow you to create flexible, **responsive web designs** that adjust to the **dimensions of the viewport** (browser window).

* They are excellent for creating fluid layouts, responsive typography, and scalable design elements that adapt seamlessly to different screen sizes.

* By combining **viewport units** with other CSS techniques, you can create dynamic, modern websites that provide an excellent user experience across a variety of devices.

