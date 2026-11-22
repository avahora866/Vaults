---
tags:
  - FORMAT
  - CSS
---
In CSS, **selectors**, **properties**, and **values** are the fundamental components that work together to define how an HTML element should appear on a web page. Let’s go through each of these terms in detail:

# Selectors

A **selector** is the part of a CSS rule that identifies the HTML element(s) you want to apply styles to. It "selects" the element(s) based on their type, class, ID, or other attributes.

## Types of Selectors:

* **Element Selector (Type Selector)**: Targets all elements of a specific HTML tag.

  * Example: `p { ... }` — This targets all `<p>` (paragraph) elements.

* **Class Selector**: Targets all elements with a specific class attribute.

  * Example: `.my-class { ... }` — This targets all elements with the class `my-class`.

* **ID Selector**: Targets a specific element with a unique ID.

 Example: `#my-id { ... }` — This targets the element with the ID `my-id`.

* **Universal Selector**: Targets all elements on the page.

  * Example: `* { ... }` — This targets all elements (not commonly used for specific styling).

* **Attribute Selector**: Targets elements based on a specific attribute or attribute value.

  * Example: `input[type="text"] { ... }` — This targets all `<input>` elements with a `type="text"` attribute.

## Example:

```
/* Element selector */
h1 { color: red; }

/* Class selector */
.button { background-color: blue; }

/* ID selector */
header { font-size: 24px; }

/* Universal selector */
* { margin: 0; }
```

# Properties

A **property** in CSS refers to the specific style or aspect of the element that you want to change. Properties define what kind of styling will be applied to the selected HTML element(s). They control aspects like colors, font sizes, spacing, alignment, borders, and many more.

## Common CSS Properties:

* **color**: Defines the color of the text.

* **background-color**: Sets the background color of an element.

* **font-size**: Specifies the size of the text.

* **margin**: Defines the space around an element.

* **padding**: Defines the space inside an element, between its content and its border.

* **border**: Specifies the border around an element.

* **text-align**: Defines the alignment of the text within an element (left, center, right).

## Example:

```
h1 {
    color: blue;         /* Property: color */
    font-size: 36px;     /* Property: font-size */
    margin: 20px;        /* Property: margin */
}
```

# Values

A **value** is the setting that is applied to a property. It tells the browser how to style the selected element. Values can be keywords (like `red`, `center`), measurements (like `20px`, `2em`), or specific data (like URLs for background images or font names).

## Types of Values:

* **Keywords**: These are predefined values. For example, `red`, `blue`, `center`, `bold`, etc.

* **Length values**: These specify a size or distance. Examples include `px` (pixels), `em`, `rem`, `%, vh`, `vw`, etc.

  * Example: `20px`, `2em`, `50%`, etc.

* **Color values**: Colors can be defined in various formats: named colors, hexadecimal values, RGB, RGBA, HSL, etc.

 Example: `red`, `#ff0000`, `rgb(255, 0, 0)`, `rgba(255, 0, 0, 0.5)`, `hsl(0, 100%, 50%)`

* **URLs**: Used for setting images or other external resources.

  * Example: `background-image: url('image.jpg');`

## Example:

```
/* Property: color */
color: red;          /* Value: red (a color keyword) */

/* Property: font-size */
font-size: 18px;     /* Value: 18px (a length in pixels) */

/* Property: background-color */
ckground-color: #f0f0f0;  / Value: #f0f0f0 (a hexadecimal color code) /

/* Property: margin */
margin: 10px 20px;   /* Value: 10px 20px (top and right margins) */
```

---

# Putting It All Together:

A CSS rule consists of a **selector**, followed by a **declaration block**. The declaration block contains one or more **property** and **value** pairs.

## Full Example:

```
/* This rule targets all <h1> elements */
h1 {
    color: green;        /* 'color' is the property, 'green' is the value */
    font-size: 32px;     /* 'font-size' is the property, '32px' is the value */
    text-align: center;  /* 'text-align' is the property, 'center' is the value */
}
```

In this example:

* The **selector** is `h1` (meaning all `<h1>` elements).

* The **properties** are `color`, `font-size`, and `text-align`.

* The **values** for these properties are `green`, `32px`, and `center`, respectively.

# Summary:

elector: Identifies the HTML element(s) to apply styles to (e.g., `h1`, `.my-class`, `#my-id`).

* **Property**: Specifies what part of the element you want to style (e.g., `color`, `font-size`, `background-color`).

* **Value**: Defines the setting for that property (e.g., `red`, `18px`, `center`).

These three components work together to define how an element should look and behave on a webpage.

