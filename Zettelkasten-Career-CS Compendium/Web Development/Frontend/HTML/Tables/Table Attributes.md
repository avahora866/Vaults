---
tags:
  - FORMAT
  - HTML
---
In HTML, when working with tables, you can customize their appearance using various attributes. Some common table attributes include `border`, `cellpadding`, and `cellspacing`. These attributes help define the table's visual style and the spacing between table cells.

# `border` Attribute

The **`border`** attribute is used to define the **border width** of the table. It specifies how thick the lines separating the cells will be. The value is typically a number that represents the width of the border in pixels.

## Example:

```
<table border="1">
  <tr>
    <th>Product</th>
    <th>Price</th>
    <th>Stock</th>
  </tr>
  <tr>
    <td>Apple</td>
    <td>$1.00</td>
    <td>50</td>
  </tr>
  <tr>
    <td>Banana</td>
    <td>$0.50</td>
    <td>100</td>
  </tr>
</table>
```

In this example, `border="1"` adds a thin border of 1 pixel around the table and its cells.

* **`border="1"`**: Creates a simple 1-pixel border around the table.

* **`border="5"`**: Creates a thicker 5-pixel border.

**Note**: The `border` attribute is an **HTML attribute**, but it is now considered outdated in modern HTML, and CSS is preferred for styling tables. You can achieve the same result using CSS.

## CSS Alternative for Border:

```
<table style="border: 1px solid black;">
  <!-- Table content -->
</table>
```

# `cellpadding` Attribute

The **`cellpadding`** attribute defines the **space between the content of a cell and its border**. It is used to control the amount of padding inside each table cell, making the content of the cells appear more spaced out and less cramped.

## Example:

```
<table border="1" cellpadding="10">
  <tr>
    <th>Product</th>
    <th>Price</th>
    <th>Stock</th>
  </tr>
  <tr>
    <td>Apple</td>
    <td>$1.00</td>
    <td>50</td>
  </tr>
  <tr>
    <td>Banana</td>
    <td>$0.50</td>
    <td>100</td>
  </tr>
</table>
```

In this example:

* **`cellpadding="10"`** adds **10 pixels** of padding inside each cell, both vertically and horizontally, making the table content appear less crowded.

**Note**: The `cellpadding` attribute is also considered outdated, and using CSS is recommended for better control.

## CSS Alternative for Cell Padding:

```
<table style="border: 1px solid black; border-collapse: collapse;">
  <tr>
    <th style="padding: 10px;">Product</th>
    <th style="padding: 10px;">Price</th>
    <th style="padding: 10px;">Stock</th>
  </tr>
  <tr>
    <td style="padding: 10px;">Apple</td>
    <td style="padding: 10px;">$1.00</td>
    <td style="padding: 10px;">50</td>
  </tr>
</table>
```

# `cellspacing` Attribute

The **`cellspacing`** attribute defines the **space between the cells** in the table. It sets the amount of space between adjacent cells, making the cells appear further apart from each other.

## Example:

```
<table border="1" cellspacing="10">
  <tr>
    <th>Product</th>
    <th>Price</th>
    <th>Stock</th>
  </tr>
  <tr>
    <td>Apple</td>
    <td>$1.00</td>
    <td>50</td>
  </tr>
  <tr>
    <td>Banana</td>
    <td>$0.50</td>
    <td>100</td>
  </tr>
</table>
```

In this example:

* **`cellspacing="10"`** adds **10 pixels** of space between each table cell.

**Note**: Like `cellpadding`, the `cellspacing` attribute is also outdated, and CSS should be used instead for better control.

## CSS Alternative for Cell Spacing:

```
<table style="border: 1px solid black; border-collapse: separate; border-spacing: 10px;">
  <tr>
    <th style="padding: 10px;">Product</th>
    <th style="padding: 10px;">Price</th>
    <th style="padding: 10px;">Stock</th>
  </tr>
  <tr>
    <td style="padding: 10px;">Apple</td>
    <td style="padding: 10px;">$1.00</td>
    <td style="padding: 10px;">50</td>
  </tr>
</table>
```

Here, **`border-spacing: 10px;`** ensures there is 10px of space between the cells. Additionally, **`border-collapse: separate;`** is required when using `border-spacing` to ensure the spacing between cells is applied.

# Summary of Table Attributes

| Attribute | Description | Example Usage |
| ----- | ----- | ----- |
| **`border`** | Defines the thickness of the table border. | `<table border="1">` |
| **`cellpadding`** | Specifies the space between the content of each cell and the cell's border. | `<table cellpadding="10">` |
| **`cellspacing`** | Defines the space between the cells in the table. | `<table cellspacing="10">` |

# Best Practice: Use CSS for Styling Tables

While the HTML attributes `border`, `cellpadding`, and `cellspacing` are still supported, they are now considered outdated. It’s recommended to use CSS for styling tables because it provides more flexibility and control over table appearance. Here’s how you would use CSS instead:

```
<table style="border: 1px solid black; border-collapse: collapse; border-spacing: 10px;">
  <tr>
    <th style="padding: 10px;">Product</th>
    <th style="padding: 10px;">Price</th>
    <th style="padding: 10px;">Stock</th>
  </tr>
  <tr>
    <td style="padding: 10px;">Apple</td>
    <td style="padding: 10px;">$1.00</td>
    <td style="padding: 10px;">50</td>
  </tr>
</table>
```

In this CSS version:

* **`border: 1px solid black;`**: Defines the border of the table.

* **`border-collapse: collapse;`**: Combines borders of adjacent cells into a single line.

* **`border-spacing: 10px;`**: Adds space between cells.

* **`padding: 10px;`**: Adds padding inside the cells.

# Conclusion:

* **`border`** sets the thickness of the table’s border.

* **`cellpadding`** controls the space inside each cell (between the content and the border).

* **`cellspacing`** defines the space between the cells themselves.

Although HTML attributes like `border`, `cellpadding`, and `cellspacing` are still functional, CSS is the preferred method to style tables for better flexibility and maintainability.

