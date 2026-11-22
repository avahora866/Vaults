---
tags:
  - FORMAT
  - CSS
---
Definitely\! CSS Grid gives you precise control over where items go using `grid-template-areas`, `grid-column`, and `grid-row`. Here’s how each one works and how they help structure your layout:

---

# `grid-template-areas`

This lets you define a visual layout using **named areas**. It makes complex layouts more readable and easier to manage.

## Step 1: Define Grid Areas in the Container

```
.container {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-template-rows: auto auto;
  grid-template-areas:
    "header header"
    "sidebar main";
}
```

This layout has:

* 2 columns (1 fraction and 2 fractions wide)

* 2 rows

* A **header** spanning two columns

* A **sidebar** and **main** content area

## Step 2: Assign Grid Items to Areas

```
.header {
  grid-area: header;
}

.sidebar {
  grid-area: sidebar;
}

.main {
  grid-area: main;
}
```

The class names correspond to the area names from `grid-template-areas`.

---

# `grid-column`

This property places an item in **specific columns** of the grid.

```
.item {
  grid-column: 1 / 3; /* starts at column line 1, ends at line 3 */
}
```

Or use shorthand:

```
.item {
  grid-column-start: 1;
  grid-column-end: 3;
}
```

---

# `grid-row`

This places an item in **specific rows** of the grid.

```
.item {
  grid-row: 2 / 4; /* spans from row line 2 to line 4 */
}
```

Or with shorthand:

```
.item {
  grid-row-start: 2;
  grid-row-end: 4;
}
```

---

# Example Using All Three

```
<div class="container">
  <div class="header">Header</div>
  <div class="sidebar">Sidebar</div>
  <div class="main">Main Content</div>
</div>
```

```
.container {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-rows: auto 1fr;
  grid-template-areas:
    "header header"
    "sidebar main";
  gap: 20px;
}

.header {
  grid-area: header;
  background: lightgray;
}

.sidebar {
  grid-area: sidebar;
  background: lightblue;
}

.main {
  grid-area: main;
  background: lightgreen;
}
```

---

# Why Use These?

* `grid-template-areas`: great for **semantic**, visual control

* `grid-column` / `grid-row`: useful for **manual placement** or **overrides**

* They can be combined for powerful layout flexibility

Let me know if you'd like to see an example that adapts on mobile too.

