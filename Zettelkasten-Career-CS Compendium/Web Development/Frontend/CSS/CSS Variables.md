---
tags:
  - FORMAT
  - CSS
---
Absolutely\! CSS variables (also known as **custom properties**) allow you to store values (like colors, sizes, or any CSS value) in one place and reuse them throughout your stylesheet. This makes your CSS easier to maintain, more organized, and flexible—especially in larger projects.

---

# ✅ Defining CSS Variables

CSS variables are defined using the `--` prefix, and they’re usually declared inside a `:root` selector so they apply globally.

```
:root {
--main-color: #3498db;
  --font-size-large: 24px;
  --padding: 1rem;
}
```

*   
  `:root` is a pseudo-class that targets the highest-level element in the document (usually `<html>`), making the variables accessible everywhere.

---

# ✅ Using CSS Variables

You use a variable with the `var()` function:

```
h1 {
  color: var(--main-color);
  font-size: var(--font-size-large);
  padding: var(--padding);
}
```

---

# ✅ Benefits of Using CSS Variables

* **Consistency**: Use the same color or spacing across multiple elements.

* **Easy Updates**: Change the value in one place and it updates everywhere.

* **Responsive Design**: You can redefine variables inside media queries.

---

# ✅ Example: Theme with CSS Variables

```
:root {
--bg-color: #ffffff;
--text-color: #333;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
}
```

---

# ✅ Overriding Variables

You can override variables in specific elements or media queries:

```
@media (prefers-color-scheme: dark) {
  :root {
  --bg-color: #121212;
  --text-color: #f1f1f1;
  }
}
```

This allows you to build **themes** or **responsive styles** easily.

---

# ✅ Fallback Values

You can even set a fallback value if the variable isn’t defined:

```
p {
  color: var(--primary-color, blue);
}
```

In this case, if `--primary-color` isn’t defined, it will default to `blue`.

---

# ✅ Summary

* Define with `--name`, usually in `:root`.

* Use with `var(--name)`.

* Great for theme management and large projects.

* Supports fallback values and overrides.

Let me know if you want a live example or how to use variables with JavaScript too\!

