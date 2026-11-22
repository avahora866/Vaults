---
tags:
  - FORMAT
  - CSS
---
Absolutely\! Let's break down how to define **columns**, **rows**, and **gaps** in a CSS Grid layout using `grid-template-columns`, `grid-template-rows`, and `grid-gap` (now just `gap` in modern CSS).

---

# `grid-template-columns`

This property defines the number and width of **columns** in the grid container.

## Example:

```
.container {
  display: grid;
  grid-template-columns: 200px 100px 1fr;
}
```

This creates 3 columns:

* First is `200px` wide

* Second is `100px`

* Third takes up remaining space (`1fr` means 1 fraction of the remaining space)

You can also repeat columns:

```
grid-template-columns: repeat(3, 1fr);
```

This creates 3 equal-width columns.

---

# `grid-template-rows`

This defines the number and height of **rows** in the grid.

## Example:

```
.container {
  grid-template-rows: 100px auto 50px;
}
```

This creates 3 rows:

* First is `100px` tall

* Second grows with content (`auto`)

* Third is `50px` tall

You can also use `repeat()` here too:

```
grid-template-rows: repeat(2, 150px);
```

Creates 2 rows, each 150px tall.

---

# `gap` (formerly `grid-gap`)

Defines spacing **between** grid items. It can be used for both rows and columns.

## Example:

```
.container {
  gap: 20px; /* 20px between all rows and columns */
}
```

Or set them individually:

```
.container {
  row-gap: 10px;
  column-gap: 30px;
}
```

This gives 10px vertical space and 30px horizontal space between items.

---

# Example All Together

```
.container {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-template-rows: 100px auto;
  gap: 20px;
}
```

This sets up:

* 2 columns (1 fraction and 2 fractions of the space)

* 2 rows (first 100px, second auto-sized)

* 20px space between all items

---

Let me know if you'd like to walk through a real-world example like a photo grid or content layout\!

