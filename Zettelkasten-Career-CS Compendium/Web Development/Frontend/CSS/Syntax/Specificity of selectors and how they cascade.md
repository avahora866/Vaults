---
tags:
  - FORMAT
  - CSS
---
Understanding **specificity** and how **cascading** works is crucial to mastering CSS. These two concepts determine which styles are applied to elements when multiple conflicting rules are present. Let's break down both concepts.

# Specificity of Selectors

**Specificity** refers to how the browser determines which CSS rule to apply when multiple rules could affect the same element. The more specific a selector is, the higher its specificity, and thus, its rule will override less specific ones.

## How Specificity is Calculated:

Specificity is calculated based on the **types of selectors** used in a rule, and it is often represented as a four-part number like this:

```
a, b, c, d
```

Where:

* **a** \= number of **inline styles** (i.e., `style` attribute in HTML)

 \= number of ID selectors (`#id-name`)

* **c** \= number of **class selectors**, **pseudo-classes** (`.class-name`, `:hover`, `:first-child`)

* **d** \= number of **element selectors** and **pseudo-elements** (`div`, `p`, `::before`, `::after`)

## Specificity Rules:

1. **Inline styles** (`style="..."` in HTML) always have the highest specificity. These have a specificity of `1,0,0,0`.

2. **ID selectors** are more specific than class selectors, pseudo-classes, and element selectors. They have a specificity of `0,1,0,0`.

3. **Class selectors**, pseudo-classes, and attribute selectors are next. They have a specificity of `0,0,1,0`.

4. **Element selectors** and **pseudo-elements** have the lowest specificity. They have a specificity of `0,0,0,1`.

## Example:

Let's break down the specificity of different selectors:

1. **ID Selector:**

```
header { color: blue; }
```

2.   
   Specificity: `0,1,0,0` (1 ID selector)

3. **Class Selector:**

```
.main-content { color: red; }
```

4.   
   Specificity: `0,0,1,0` (1 class selector)

5. **Element Selector:**

```
div { color: green; }
```

6.   
   Specificity: `0,0,0,1` (1 element selector)

7. **Complex Rule with Multiple Selectors:**

```
header .main-content div { color: purple; }
```

8.   
   Specificity: `0,1,1,2` (1 ID \+ 1 class \+ 2 elements)

## Example of Conflicting Rules:

```
header { color: blue; } / Specificity: 0,1,0,0 /
.main-content { color: red; } /* Specificity: 0,0,1,0 */
div { color: green; } /* Specificity: 0,0,0,1 */
```

If a `<div>` inside an element with the class `main-content` and the ID `header` has these conflicting styles, the browser will apply the style based on specificity.

or an element with `id="header"`, the rule with ID selector (`#header { color: blue; }`) will be applied since the ID selector has higher specificity than the class or element selectors.

# Cascading in CSS

The term **cascading** in CSS refers to the order in which styles are applied when multiple rules affect the same element. The cascade is determined by three main factors:

1. **Importance of the rule** (marked by `!important`).

2. **Specificity of the selectors** (as we discussed).

3. **Order of appearance in the stylesheet** (if two rules have equal specificity).

## Cascading Order (from highest to lowest priority):

1. **Inline styles**: Styles defined directly within an element's `style` attribute in HTML. They have the highest priority.

```
<div style="color: red;">This is a red text.</div>
```

2.   
   **Styles marked with `!important`**: If a rule has `!important`, it overrides other rules regardless of specificity.

```
.button { color: green !important; }
```

3.   
   **ID selectors**: These are more specific than class or element selectors.

```
header { color: blue; }
```

4.   
   **Class selectors, pseudo-classes, and attribute selectors**: These come next in specificity.

```
.button { color: yellow; }
```

5.   
   **Element selectors and pseudo-elements**: These have the lowest specificity.

```
p { color: black; }
```

6.   
   **Order of appearance**: If two rules have the same specificity, the one that appears **later** in the stylesheet takes precedence.

```
p { color: red; }
p { color: blue; } /* This will be applied */
```

## Example of Cascading:

Suppose we have the following HTML and CSS:

```
<div class="box" id="unique-box">This is a box.</div>
```

```
/* 1. Element selector */
div { color: green; } /* Specificity: 0,0,0,1 */

/* 2. Class selector */
.box { color: red; } /* Specificity: 0,0,1,0 */

/* 3. ID selector */
unique-box { color: blue; } / Specificity: 0,1,0,0 /
```

 this case, the color will be blue because the ID selector `#unique-box` has the highest specificity. The class selector `.box` and element selector `div` are overridden due to their lower specificity.

However, if we added a rule like this:

```
/* This comes after the previous rules */
div { color: purple; }
```

*   
  The color will now be **purple** because the second `div` rule appears **later** in the stylesheet, and **order of appearance** is the tiebreaker when specificity is equal.

---

# Using `!important`

The `!important` declaration can be used to give a CSS rule higher priority than others, regardless of its specificity.

## Example:

```
/* This will apply even if other rules target the same element */
p {
    color: green !important;
}
```

If another rule (with higher specificity) is applied to a `<p>` element, it will be ignored if it doesn't have `!important`.

# Best Practices:

* Avoid excessive use of `!important` as it can make your CSS harder to maintain and override.

* Instead, prefer using higher specificity or adjusting the order of the rules in your stylesheet.

* Use **inline styles** sparingly because they always have the highest specificity and can be difficult to override.

# Summary:

* **Specificity** determines which rule wins when multiple rules target the same element. The more specific the selector, the higher its specificity, and the more likely it is to apply.

* **Cascading** decides which styles are applied when conflicting rules exist, based on their order in the stylesheet, importance (`!important`), and specificity.

