---
tags:
  - FORMAT
  - CSS
---
**Flexbox (Flexible Box Layout)** is a powerful layout system in CSS that allows you to design complex and responsive layouts with ease. It enables elements to be aligned and distributed within a container, even when their sizes are unknown or dynamic. Flexbox is especially useful for building one-dimensional layouts, either in a row (horizontal) or column (vertical) direction.

# Key Properties of Flexbox

## `display: flex`

* **What it does**: The `display: flex` property is used on a container to enable flexbox behavior. It tells the browser to treat the container as a flex container and the direct child elements as flex items.

* **How it works**: Once you set `display: flex` on a container, its children (the flex items) will be arranged according to flexbox rules, allowing for alignment and distribution control.

* **Example**:

```
.container {
    display: flex; /* Enables Flexbox */
}
```

---

## `flex-direction`

* **What it does**: The `flex-direction` property determines the direction in which the flex items are placed within the flex container. By default, items are arranged in a row, but you can change this using `flex-direction`.

* **Possible values**:

  * `row` (default): Items are placed in a row, from left to right.

  * `row-reverse`: Items are placed in a row, from right to left.

  * `column`: Items are stacked in a column, from top to bottom.

  * `column-reverse`: Items are stacked in a column, from bottom to top.

* **Example**:

```
.container {
    display: flex;
    flex-direction: row;  /* Items arranged horizontally (default) */
}

.container-vertical {
    display: flex;
    flex-direction: column;  /* Items arranged vertically */
}
```

---

## `justify-content`

* **What it does**: The `justify-content` property controls how the flex items are aligned along the **main axis**, which is determined by the `flex-direction` (horizontal for `row`, vertical for `column`).

* **Possible values**:

  * `flex-start`: Aligns items to the start of the flex container (default).

  * `flex-end`: Aligns items to the end of the flex container.

  * `center`: Centers the items in the flex container.

  * `space-between`: Distributes items evenly with the first item at the start and the last item at the end.

  * `space-around`: Distributes items evenly with equal space around them.

  * `space-evenly`: Distributes items with equal space between and around them.

* **Example**:

```
.container {
    display: flex;
    justify-content: space-between;  /* Items spaced evenly with start and end alignment */
}
```

---

## `align-items`

* **What it does**: The `align-items` property controls how the flex items are aligned along the **cross axis**, which is perpendicular to the main axis (vertical for `row`, horizontal for `column`).

* **Possible values**:

  * `stretch` (default): Items stretch to fill the container.

  * `flex-start`: Aligns items to the start of the cross axis.

  * `flex-end`: Aligns items to the end of the cross axis.

  * `center`: Centers items along the cross axis.

  * `baseline`: Aligns items based on their baseline (for text-heavy elements).

* **Example**:

```
.container {
    display: flex;
    align-items: center;  /* Items are vertically centered (in a row direction) */
}
```

---

## `align-self`

* **What it does**: The `align-self` property allows you to override the `align-items` setting for individual flex items. It controls how a specific item is aligned along the cross axis, overriding the alignment for the entire container.

* **Possible values**:

  * `auto` (default): Inherits the `align-items` value from the container.

  * `flex-start`: Aligns the item to the start of the cross axis.

  * `flex-end`: Aligns the item to the end of the cross axis.

  * `center`: Centers the item along the cross axis.

  * `baseline`: Aligns the item based on its baseline.

  * `stretch`: Stretches the item to fill the container.

* **Example**:

```
.item {
    align-self: flex-start;  /* Aligns this item to the start of the cross axis */
}
```

---

## `flex-wrap`

* **What it does**: The `flex-wrap` property controls whether the flex items should wrap onto multiple lines if there is not enough space in the container. By default, flex items try to stay in a single line.

* **Possible values**:

  * `nowrap` (default): Flex items will not wrap; they will stay on a single line.

  * `wrap`: Flex items will wrap onto the next line if they exceed the container's width or height.

  * `wrap-reverse`: Flex items will wrap onto the next line, but in the reverse direction.

* **Example**:

```
.container {
    display: flex;
    flex-wrap: wrap;  /* Items will wrap onto multiple lines if needed */
}
```

---

## `flex` (Shorthand for `flex-grow`, `flex-shrink`, and `flex-basis`)

* **What it does**: The `flex` property is a shorthand for three properties:

  * `flex-grow`: Defines how much a flex item will grow relative to the rest of the flex items in the container.

  * `flex-shrink`: Defines how much a flex item will shrink relative to the rest of the flex items when there is not enough space.

  * `flex-basis`: Defines the initial size of the flex item before any growth or shrinkage.

* **Possible values**:

  * The default is `0 1 auto`, meaning items will not grow but can shrink, and their size is based on their content.

  * Example values: `flex: 1`, `flex: 2`, `flex: 0.5`.

* **Example**:

```
.item {
    flex: 1;  /* This item will grow to take up available space equally with other items */
}
```

---

# `flex-grow`, `flex-shrink`, and `flex-basis` (Individual Flex Properties)

* **`flex-grow`**: Defines how much a flex item will grow relative to the other flex items when there is extra space in the container. The higher the value, the more space the item will take.

  * Example: `flex-grow: 2;` (twice as large as other items with `flex-grow: 1`).

* **`flex-shrink`**: Defines how much a flex item will shrink relative to the other flex items when there is not enough space.

  * Example: `flex-shrink: 0;` (item will not shrink).

* **`flex-basis`**: Defines the initial size of a flex item before any growth or shrinking.

  * Example: `flex-basis: 200px;` (item will start with a size of 200px).

* **Example**:

```
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 100px;  /* Item starts at 100px, then can grow or shrink */
}
```

---

# Full Example of Flexbox Layout:

```
<div class="container">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
    <div class="item">Item 4</div>
</div>
```

```
.container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;  /* Allows items to wrap if necessary */
    gap: 10px;  /* Optional: Adds space between items */
}

.item {
    flex: 1;
    margin: 5px;
    background-color: lightblue;
    text-align: center;
    padding: 20px;
}
```

*   
  In this example:

  * The container uses `display: flex` to enable flexbox layout.

  * `flex-direction: row` arranges items in a horizontal line.

  * `justify-content: space-between` places equal space between items.

  * `align-items: center` vertically aligns the items in the middle of the container.

  * `flex-wrap: wrap` allows the items to wrap onto a new line if necessary.

  * The items use `flex: 1` to grow and fill the available space evenly.

---

# Summary of Flexbox Properties:

| Property | Description |
| ----- | ----- |
| `display: flex` | Enables flexbox layout for the container. |
| `flex-direction` | Defines the direction of the main axis (row, column, row-reverse, column-reverse). |
| `justify-content` | Aligns items along the main axis (horizontal for row, vertical for column). |
| `align-items` | Aligns items along the cross axis (perpendicular to the main axis). |
| `align-self` | Overrides `align-items` for individual items. |
| `flex-wrap` | Determines whether flex items should wrap onto multiple lines. |
| `flex` | Shorthand for `flex-grow`, `flex-shrink`, and `flex-basis`. |
| `flex-grow` | Defines how much an item will grow relative to other items. |
| `flex-shrink` | Defines how much an item will shrink when there is not enough space. |
| `flex-basis` | Sets the initial size of a flex item before it is adjusted by grow or shrink. |

---

Flexbox simplifies layout building and responsiveness, allowing for easy alignment and distribution of space in containers. It's especially helpful when you need to create complex, dynamic layouts that adjust based on screen size and content size.

