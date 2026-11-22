---
tags:
  - FORMAT
  - HTML
---
Creating tables in HTML is essential for displaying structured data, such as lists, spreadsheets, or reports. The `<table>`, `<tr>`, `<th>`, and `<td>` tags are the core components used to define and structure a table in HTML. Here’s an explanation of how to use each of these tags to build a table.

# `<table>` Tag

The `<table>` element is the container that holds all the other table elements. It defines a table structure in HTML.

## Basic Structure:

```
<table>
  <!-- Table rows and columns go here -->
</table>
```

# `<tr>` Tag (Table Row)

The `<tr>` (table row) tag defines a row in the table. Inside each `<tr>`, you place one or more `<th>` (table header) or `<td>` (table data) elements to define the table headers and data cells.

## Example:

```
<table>
  <tr>
    <th>Header 1</th>
    <th>Header 2</th>
    <th>Header 3</th>
  </tr>
</table>
```

In this example, the `<tr>` defines a row with three header cells (using `<th>` tags).

# `<th>` Tag (Table Header)

The `<th>` tag defines a table header cell. The text inside a `<th>` is typically bold and centered by default. It is used to provide titles or descriptions for the columns.

* By default, the content of a `<th>` element is centered and bold.

* It is generally used in the first row to describe the table's data.

## Example:

```
<table>
  <tr>
    <th>Product</th>
    <th>Price</th>
    <th>Stock</th>
  </tr>
</table>
```

In this example, the first row defines the headers of the table: "Product", "Price", and "Stock".

# `<td>` Tag (Table Data)

The `<td>` (table data) tag defines a standard data cell inside a table. Each `<td>` element represents a single piece of data in a table row. The `<td>` element can contain text, images, links, or other HTML elements.

## Example:

```
<table>
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

* The first `<tr>` defines the table headers.

* The next two `<tr>` elements define the rows of data (with `<td>` for individual data cells).

# Putting It All Together: Complete Table Example

Here is an example of a fully structured HTML table:

```
<table border="1">
  <!-- Table Header Row -->
  <tr>
    <th>Product</th>
    <th>Price</th>
    <th>Stock</th>
  </tr>
  <!-- Table Row 1 -->
  <tr>
    <td>Apple</td>
    <td>$1.00</td>
    <td>50</td>
  </tr>
  <!-- Table Row 2 -->
  <tr>
    <td>Banana</td>
    <td>$0.50</td>
    <td>100</td>
  </tr>
  <!-- Table Row 3 -->
  <tr>
    <td>Orange</td>
    <td>$0.75</td>
    <td>80</td>
  </tr>
</table>
```

# Explanation of the Example:

* **`<table border="1">`**: This adds a border to the table for visibility (you can remove the `border` attribute or style it with CSS for better control).

* **First `<tr>`**: Defines the header row, using `<th>` elements to label the columns.

* **Subsequent `<tr>`**: Defines rows of data using `<td>` elements for each data point in the table.

# Key Tags Used in HTML Tables

| Tag | Description | Example Usage |
| ----- | ----- | ----- |
| `<table>` | Creates a table container. | `<table>` ... `</table>` |
| `<tr>` | Defines a row in the table. | `<tr>` ... `</tr>` |
| `<th>` | Defines a header cell in a table, typically bold and centered. | `<th>Product</th>` |
| `<td>` | Defines a data cell in a table. | `<td>Apple</td>` |

# Optional: Adding a Caption with `<caption>`

You can add a caption to your table for better accessibility or to describe the content of the table. Use the `<caption>` element to define a title for the table.

## Example:

```
<table>
  <caption>Fruit Price List</caption>
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
</table>
```

In this example, the `<caption>` tag adds a "Fruit Price List" title to the table, which is displayed above the table.

# Optional: Spanning Cells with `colspan` and `rowspan`

* **`colspan`**: Allows a `<td>` or `<th>` to span across multiple columns.

* **`rowspan`**: Allows a `<td>` or `<th>` to span across multiple rows.

## Example of `colspan` and `rowspan`:

```
<table border="1">
  <tr>
    <th colspan="2">Product Info</th>
    <th>Stock</th>
  </tr>
  <tr>
    <td colspan="2">Apple</td>
    <td>50</td>
  </tr>
  <tr>
    <td rowspan="2">Banana</td>
    <td>Price</td>
    <td>$0.50</td>
  </tr>
  <tr>
    <td>Stock</td>
    <td>100</td>
  </tr>
</table>
```

*   
  **`colspan="2"`**: The first `<th>` in the first row spans across two columns.

* **`rowspan="2"`**: The "Banana" cell in the second row spans across two rows.

# Summary

* **`<table>`**: The table container.

* **`<tr>`**: Defines a table row.

* **`<th>`**: Defines a header cell, typically for column titles.

* **`<td>`**: Defines a data cell, used to hold the content in a table row.

* **`<caption>`**: Optional, adds a title to the table.

* **`colspan`** and **`rowspan`**: Optional attributes that allow a cell to span across multiple columns or rows.

Tables are a great way to organize and present data clearly on a webpage, and with the use of these tags and attributes, you can create simple or complex tables to suit your needs.

