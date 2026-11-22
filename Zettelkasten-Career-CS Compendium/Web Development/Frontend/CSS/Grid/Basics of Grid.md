---
tags:
  - FORMAT
  - CSS
---
Absolutely\! CSS Grid is a powerful layout system designed specifically for creating **two-dimensional layouts**—both **rows and columns**. It’s ideal for more complex designs like dashboards, galleries, or full-page layouts.

---

# ✅ What Is CSS Grid?

CSS Grid allows you to layout elements in a grid of **rows and columns**, giving you precise control over placement, alignment, and spacing.

To use it, you turn a container into a grid with:

```
.container {
  display: grid;
}
```

---

# ✅ Defining Rows and Columns

You define the structure of your grid using:

## `grid-template-columns`

Sets the number and size of columns.

```
.container {
  display: grid;
  grid-template-columns: 200px 200px;
}
```

This creates **2 columns**, each 200px wide.

## `grid-template-rows`

Sets the number and size of rows.

```
.container {
  grid-template-rows: 100px 100px;
}
```

This creates **2 rows**, each 100px tall.

---

# ✅ Using `fr` Unit

The `fr` (fractional unit) lets columns or rows take up **a portion of remaining space**.

```
.container {
  grid-template-columns: 1fr 2fr;
}
```

Here, the first column gets 1 part, and the second gets 2 parts of the remaining space.

---

# ✅ Placing Grid Items

Items are placed in the grid in the order they appear in the HTML, filling each grid cell by default. You can explicitly position items using:

```
.item {
  grid-column: 1 / 3; /* spans columns 1 to 2 */
  grid-row: 1 / 2;     /* occupies first row */
}
```

---

# ✅ Gap Between Items

Use `gap`, `column-gap`, and `row-gap` to add spacing:

```
.container {
  gap: 20px;
}
```

This adds 20px spacing between all rows and columns.

---

# ✅ Auto-Placement

You can let Grid **automatically place items** and handle rows/columns dynamically:

```
.container {
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
}
```

This creates as many columns as will fit, each at least 150px wide.

---

# ✅ Example

```
<div class="grid">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>
```

```
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.item {
  background: lightgray;
  padding: 20px;
  text-align: center;
}
```

This creates a 3-column grid with equal-width columns and gaps between them.

---

# ✅ Why Use CSS Grid?

* It’s perfect for **2D layouts** (rows \+ columns)

* Reduces reliance on floats and positioning

* Pairs well with Flexbox (which is better for 1D layouts)

---

Let me know if you want to see a responsive grid example or combine it with Flexbox for a full layout\!

