---
tags:
  - FORMAT
  - CSS
---
A CSS rule is composed of two main parts: the **selector** and the **declaration block**. Inside the declaration block, there are **properties** and **values**. Let’s break down the structure of a CSS rule in detail:

# Structure of a CSS Rule:

```
selector {
    property: value;
}
```

# Selector

The **selector** is the part of the rule that targets the HTML element(s) you want to style. It identifies which element(s) the CSS rule will apply to.

xample: `p`, `.class-name`, `#id-name`, `div`, `h1`, etc.

* **Types of Selectors**:

  * **Element selector**: Targets HTML elements (e.g., `p`, `h1`, `div`).

  * **Class selector**: Targets elements with a specific class attribute (e.g., `.my-class`).

 ID selector: Targets a specific element with a unique ID (e.g., `#my-id`).

  * **Universal selector**: Targets all elements (e.g., `*`).

## Example:

```
p { ... } /* Targets all <p> (paragraph) elements */
.my-class { ... } /* Targets elements with the class "my-class" */
my-id { ... } / Targets the element with the ID "my-id" /
```

# Declaration Block

The **declaration block** is enclosed in curly braces `{}` and contains one or more **property** and **value** pairs. Each pair defines a style for the targeted element.

* **Property**: The specific style you want to apply to the element (e.g., `color`, `font-size`, `margin`).

* **Value**: The setting or value for that property (e.g., `red`, `16px`, `10px`).

## Example:

```
color: red; /* 'color' is the property, 'red' is the value */
font-size: 16px; /* 'font-size' is the property, '16px' is the value */
margin: 10px; /* 'margin' is the property, '10px' is the value */
```

# Multiple Declarations

A CSS rule can have multiple property-value pairs inside the declaration block, separated by semicolons. Each property defines a different style.

## Example:

```
h1 {
    color: blue;       /* Text color */
    font-size: 24px;   /* Font size */
    text-align: center; /* Text alignment */
}
```

This rule targets all `<h1>` elements, setting their text color to blue, font size to 24 pixels, and text alignment to the center.

# Putting It All Together

Here’s a complete CSS rule that demonstrates how selectors, properties, and values work:

```
/* CSS rule to style a paragraph element */
p {
    color: green;         /* Text color */
    font-size: 18px;      /* Font size */
    line-height: 1.5;     /* Line height */
    text-align: justify;  /* Text alignment */
}
```

# Explanation:

* **Selector**: `p` — Targets all `<p>` elements (paragraphs).

* **Declaration Block**: `{ color: green; font-size: 18px; line-height: 1.5; text-align: justify; }`

  * **Property-Value Pairs**:

    * `color: green;` — The text color is set to green.

    * `font-size: 18px;` — The font size is set to 18 pixels.

    * `line-height: 1.5;` — The line height is set to 1.5 times the font size.

    * `text-align: justify;` — The text is justified (aligned to both left and right margins).

# Summary of Key Parts:

1. **Selector**: The target element(s) you want to style.

2. **Declaration Block**: Contained in `{}`, it holds the property-value pairs.

3. **Property**: The style you want to apply (e.g., color, font-size, margin).

4. **Value**: The setting for the property (e.g., red, 20px, 10px).

This structure allows you to customize the look of HTML elements by applying various styles. You can combine multiple CSS rules to achieve complex layouts and designs for web pages.

