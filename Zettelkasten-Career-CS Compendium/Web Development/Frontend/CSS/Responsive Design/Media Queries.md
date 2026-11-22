---
tags:
  - FORMAT
  - CSS
---
**Media queries** in CSS are a powerful tool that allows you to apply different styles depending on the **characteristics of the device** or **viewport** (such as width, height, orientation, etc.). One of the most common uses of media queries is to create **responsive designs**, ensuring that your website looks good on various devices like desktops, tablets, and mobile phones.

# What is a Media Query?

A media query consists of two main parts:

1. **Media Type**: The type of device (e.g., `screen`, `print`, `all`).

2. **Condition**: The condition or test that is checked, such as the width of the viewport.

In CSS, media queries are used with the `@media` rule to apply styles based on certain conditions. The condition could be based on things like the screen width, height, device orientation (landscape/portrait), or more.

# Using Media Queries with `max-width`

The `max-width` media query allows you to apply styles to devices whose screen width is **less than or equal to** the specified value. This is commonly used for **mobile-first responsive design**, where you design for small screens first, and then progressively add styles for larger screens using **media queries**.

# Basic Syntax of a Media Query

Here’s the general syntax for a media query with `max-width`:

```
@media (max-width: 768px) {
    /* Styles go here */
}
```

In this example:

* `@media` is the rule to define a media query.

* `(max-width: 768px)` is the condition, which means "apply the styles inside this block if the viewport width is 768px or less."

* The styles inside the `{}` will only be applied when the condition is true.

# Example: Using `@media (max-width: 768px)`

Here’s a simple example of how you might use `@media` to change the layout of a webpage when the screen width is 768px or smaller (common for tablets and mobile devices):

```
/* Default styles (for larger screens) */
body {
    font-size: 16px;
    background-color: lightblue;
}

.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
}

/* Styles for devices with max-width 768px (e.g., tablets and phones) */
@media (max-width: 768px) {
    body {
        font-size: 14px; /* Smaller font size on small screens */
        background-color: lightgreen; /* Change background color */
    }

    .container {
        grid-template-columns: 1fr; /* Stack items in a single column */
    }
}
```

# Explanation of Example:

1. **Default Styles (for larger screens)**:

   * The body has a font size of `16px` and a light blue background color.

   * The `.container` uses a **three-column grid layout**.

2. **Media Query for `max-width: 768px`**:

   * When the screen width is **768px or less** (like a tablet or mobile device):

     * The body’s font size is reduced to `14px` and the background color changes to light green.

     * The `.container` layout changes to a **single column** layout instead of three columns.

# More Examples of Media Queries

## Targeting Smaller Screens (Max-width)

This media query applies styles only when the screen width is **768px or less** (e.g., mobile devices):

```
@media (max-width: 768px) {
    .header {
        font-size: 14px;
        padding: 10px;
    }

    .sidebar {
        display: none;  /* Hide sidebar on small screens */
    }
}
```

## Targeting Larger Screens (Min-width)

You can also use `min-width` to apply styles to devices that are **larger than a certain width** (e.g., for desktops):

```
@media (min-width: 1024px) {
    .container {
        width: 80%;
        margin: 0 auto;  /* Center container */
    }

    .nav {
        display: flex;
        justify-content: space-between;
    }
}
```

In this example, the styles will only apply when the screen width is **1024px or larger** (e.g., desktops).

## Combining `min-width` and `max-width`

You can also combine multiple media queries for more specific conditions, such as targeting a specific range of screen sizes:

```
/* Styles for devices between 768px and 1024px */
@media (min-width: 768px) and (max-width: 1024px) {
    .sidebar {
        width: 250px;
    }

    .main-content {
        margin-left: 260px;
    }
}
```

This media query applies styles for devices with a screen width between **768px and 1024px**, such as tablets in landscape mode.

---

# Best Practices for Using Media Queries

1. **Mobile-First Approach**: Start by designing for the smallest screens (mobile-first) and use `min-width` for larger screens. This approach reduces the need for unnecessary overrides.

2. **Avoid Overlapping Rules**: Be cautious when using multiple media queries with overlapping conditions. Styles can conflict if you're not careful, especially if you have a mix of `min-width` and `max-width` queries.

3. **Use Logical Grouping**: Group styles together that apply to similar screen sizes. This makes your code more readable and maintainable.

4. **Test on Multiple Devices**: Always test your design on various devices and viewport sizes to ensure that the media queries behave as expected.

---

# Example: Mobile-First Design

Here’s a simple example of a mobile-first approach using `@media` queries:

```
/* Base styles for mobile devices (default) */
body {
    font-size: 14px;
    background-color: lightblue;
}

.container {
    width: 100%;
    padding: 10px;
}

/* For larger screens (tablets and above) */
@media (min-width: 768px) {
    body {
        font-size: 16px;
    }

    .container {
        width: 75%;
        margin: 0 auto;
    }
}

/* For even larger screens (desktops) */
@media (min-width: 1024px) {
    .container {
        width: 60%;
    }
}
```

# Explanation of Mobile-First Design:

* **Base Styles**: The default styles (mobile-first) apply to all devices. The body has a font size of `14px`, and the container takes up 100% of the screen width.

* **Tablet & Larger Screens**: When the viewport width reaches `768px` or more, the font size is increased to `16px`, and the container width is reduced to 75%.

* **Desktops**: When the screen is `1024px` or larger, the container width is set to 60%.

---

# Conclusion:

* Use `@media (max-width: 768px)` to apply styles for devices with a screen width of **768px or smaller**, typically mobile phones and tablets.

* **Media queries** allow for **responsive design**, ensuring your layout adapts to different screen sizes, providing a better user experience on all devices.

* Combining `min-width`, `max-width`, and other conditions gives you full control over your design's responsiveness.

By using **media queries**, you can create flexible layouts that adjust dynamically, making your website or application user-friendly across a wide range of devices.

