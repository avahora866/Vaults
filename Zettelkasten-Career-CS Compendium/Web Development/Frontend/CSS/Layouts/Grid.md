---
tags:
  - FORMAT
  - CSS
---


**CSS Grid Layout** is a two-dimensional layout system that allows you to create complex and responsive web designs. Unlike Flexbox, which is primarily for one-dimensional layouts (rows or columns), CSS Grid allows you to design layouts with both rows and columns, giving you more control over placement and alignment. It’s ideal for building grid-based layouts, such as image galleries, dashboards, or web pages with complex structures.

# Key Properties of CSS Grid

## `display: grid`

* **What it does**: The `display: grid` property is used on a container element to enable CSS Grid. Once you apply this property, the container's children become **grid items** that can be placed in a two-dimensional grid structure.

* **How it works**: The grid container defines the layout with rows and columns, and the grid items automatically fit into the grid according to the rules you define.

* **Example**:

```
.container {
    display: grid; /* Enables the grid layout */
}
```

---

## `grid-template-columns`

* **What it does**: The `grid-template-columns` property defines the number and size of the columns in the grid container. This property is used to set the widths of the grid columns.

* **How it works**: You can define the size of each column, and the grid will automatically divide the available space based on your specifications. You can use fixed units like pixels (`px`), relative units like percentages (`%`), or flexible units like `fr` (fractional units).

* **Example**:

```
.container {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;  /* Creates 3 columns with different sizes */
}
```

*   
  In this example:

  * The first column takes up 1 fraction of the available space.

  * The second column takes up 2 fractions (double the space of the first).

  * The third column takes up 1 fraction of the space.

---

## `grid-template-rows`

* **What it does**: The `grid-template-rows` property defines the number and size of the rows in the grid container. Similar to `grid-template-columns`, this property controls the height of each row in the grid.

* **How it works**: You can define the size of each row, and the grid will adjust accordingly. The row sizes can also be set using fixed units, percentages, or fractional units (`fr`).

* **Example**:

```
.container {
    display: grid;
    grid-template-rows: 100px auto 200px;  /* Creates 3 rows with different heights */
}
```

*   
  In this example:

  * The first row is 100px tall.

  * The second row automatically adjusts its height based on content (`auto`).

  * The third row is 200px tall.

---

## `grid-gap` (or `gap`)

* **What it does**: The `grid-gap` (or simply `gap`) property defines the spacing between rows and columns in the grid. It allows you to control the space between grid items without having to manually add margins.

* **How it works**: You can define both row gaps and column gaps independently. If you want equal space between both rows and columns, you can use the shorthand `gap`.

* **Example**:

```
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);  /* 3 equal columns */
    grid-gap: 10px;  /* Adds 10px gap between rows and columns */
}
```

*   
  In this example:

  * `grid-gap` sets both the row and column gaps to 10px.

* You can also set row and column gaps individually:

```
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);  
    grid-row-gap: 15px;  /* Gap between rows */
    grid-column-gap: 10px;  /* Gap between columns */
}
```

---

# Full Example: Basic CSS Grid Layout

Here’s an example where we use all the key properties to create a basic grid layout with 3 columns and 2 rows:

```
<div class="container">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
    <div class="item">Item 4</div>
    <div class="item">Item 5</div>
    <div class="item">Item 6</div>
</div>
```

```
.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;  /* 3 equal columns */
    grid-template-rows: auto auto;  /* 2 rows that adjust based on content */
    grid-gap: 20px;  /* 20px gap between rows and columns */
}

.item {
    background-color: lightblue;
    padding: 20px;
    text-align: center;
  border: 1px solid #ddd;
}
```

*   
  **In this example**:

  * The `.container` is set to `display: grid`.

  * `grid-template-columns: 1fr 1fr 1fr` creates three equally sized columns.

  * `grid-template-rows: auto auto` creates two rows, where the height of each row adjusts based on the content inside it.

  * `grid-gap: 20px` sets a 20px gap between each grid item both horizontally and vertically.

# Shorthand Notation for `grid-template-columns` and `grid-template-rows`

* You can also use the `repeat()` function to simplify the code. For example, instead of specifying the same column size repeatedly, you can use `repeat()` to create a series of columns or rows.

* **Example**:

```
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);  /* 3 equal columns */
    grid-template-rows: repeat(2, auto);  /* 2 rows that adjust based on content */
    grid-gap: 10px;
}
```

This is equivalent to:

```
grid-template-columns: 1fr 1fr 1fr;
grid-template-rows: auto auto;
```

---

# Additional Grid Layout Properties

## `grid-template-areas`

* **What it does**: The `grid-template-areas` property allows you to define named grid areas within the grid container. This helps in creating complex layouts with more human-readable code.

* **How it works**: You assign a name to each grid area, and then use `grid-area` on individual items to position them in the layout.

* **Example**:

```
.container {
    display: grid;
    grid-template-areas:
        "header header header"
        "sidebar content content"
        "footer footer footer";
    grid-gap: 10px;
}

.header {
    grid-area: header;
}

.sidebar {
    grid-area: sidebar;
}

.content {
    grid-area: content;
}

.footer {
    grid-area: footer;
}
```

*   
  In this example, we define a grid layout with areas for a header, sidebar, content, and footer.

---

# Summary of Key Properties

| Property | Description |
| ----- | ----- |
| `display: grid` | Enables the grid layout for the container. |
| `grid-template-columns` | Defines the size of columns in the grid. |
| `grid-template-rows` | Defines the size of rows in the grid. |
| `grid-gap` or `gap` | Specifies the space between grid items (both rows and columns). |
| `grid-template-areas` | Defines named areas within the grid for more intuitive layout management. |
| `grid-column` | Defines the horizontal placement of a grid item. |
| `grid-row` | Defines the vertical placement of a grid item. |

---

# Summary of CSS Grid Layout:

CSS Grid allows you to easily build layouts with both rows and columns. You can control the size of rows and columns, set gaps between items, and even place items in named areas. It’s a flexible and powerful layout technique that makes complex web designs much easier to manage and maintain compared to older layout methods (like floats or tables).

