---
tags:
  - FORMAT
  - Python
---
# Reshaping Data in Pandas: `pivot()`, `pivot_table()`, `stack()`, and `unstack()`

Reshaping data is a common operation in data analysis to transform data into a more readable or analyzable format. Pandas provides powerful tools like **`pivot()`**, **`pivot_table()`**, **`stack()`**, and **`unstack()`** to achieve this.

---

# Pivoting Data Using `pivot()`

## What is `pivot()`?

* **`pivot()`** is used to reshape data by turning unique values from one column into multiple columns (similar to an Excel pivot table).

* It requires the data to be structured with unique values for the specified column combinations.

## Syntax:

```py
DataFrame.pivot(index='column1', columns='column2', values='column3')
```

*   
  **`index`**: Column to use as the row labels.

* **`columns`**: Column whose unique values will become new columns.

* **`values`**: Column whose values will populate the table.

---

## Example: Using `pivot()`

```py
import pandas as pd

data = {
    'Date': ['2024-01-01', '2024-01-01', '2024-01-02', '2024-01-02'],
    'City': ['New York', 'Chicago', 'New York', 'Chicago'],
    'Temperature': [30, 25, 32, 28]
}
df = pd.DataFrame(data)

 Pivot the data
pivot_df = df.pivot(index='Date', columns='City', values='Temperature')
print(pivot_df)
```

**Output**:

```
City         Chicago  New York
Date                           
2024-01-01        25        30
2024-01-02        28        32
```

*   
  **`pivot()`** makes it easy to view data in a tabular format.

---

# Using `pivot_table()` for Aggregation

## What is `pivot_table()`?

* **`pivot_table()`** is similar to `pivot()` but supports aggregation when there are duplicate values.

* It’s useful for summarizing large datasets.

## Syntax:

```py
DataFrame.pivot_table(index='column1', columns='column2', values='column3', aggfunc='mean')
```

*   
  **`aggfunc`**: Aggregation function (e.g., `mean`, `sum`, `count`, `min`, `max`).

---

## Example: Using `pivot_table()`

```py
data = {
    'Date': ['2024-01-01', '2024-01-01', '2024-01-02', '2024-01-02', '2024-01-02'],
    'City': ['New York', 'Chicago', 'New York', 'Chicago', 'Chicago'],
    'Temperature': [30, 25, 32, 28, 29]
}
df = pd.DataFrame(data)

 Create pivot table with mean temperature
pivot_table_df = df.pivot_table(index='Date', columns='City', values='Temperature', aggfunc='mean')
print(pivot_table_df)
```

**Output**:

```
City         Chicago  New York
Date                           
2024-01-01      25.0      30.0
2024-01-02      28.5      32.0
```

*   
  When there are multiple values for the same `Date` and `City`, `pivot_table()` aggregates using the mean.

---

# Stacking Data Using `stack()`

## What is `stack()`?

* **`stack()`** converts columns into rows, creating a **multi-level index**.

* It’s useful for making data long or "tidy."

## Syntax:

```py
DataFrame.stack(level=-1, dropna=True)
```

*   
  **`level`**: Specifies which level to stack (default is the last level).

* **`dropna`**: Removes missing values (`NaN`) if `True`.

---

## Example: Using `stack()`

```py
data = {
    'City': ['New York', 'Chicago'],
    '2024-01-01': [30, 25],
    '2024-01-02': [32, 28]
}
df = pd.DataFrame(data).set_index('City')

 Stack the columns into rows
stacked_df = df.stack()
print(stacked_df)
```

**Output**:

```
City       2024-01-01    30
           2024-01-02    32
Chicago    2024-01-01    25
           2024-01-02    28
dtype: int64
```

*   
  Now the data is in a long format with a **MultiIndex**.

---

# Unstacking Data Using `unstack()`

## What is `unstack()`?

* **`unstack()`** performs the reverse of `stack()`.

* It converts row data (from a MultiIndex) into columns.

## Syntax:

```py
DataFrame.unstack(level=-1, fill_value=None)
```

*   
  **`level`**: Specifies which level to unstack (default is the last level).

* **`fill_value`**: Specifies a value to replace missing data.

---

## Example: Using `unstack()`

```py
 Unstack the stacked data
unstacked_df = stacked_df.unstack()
print(unstacked_df)
```

**Output**:

```
         2024-01-01  2024-01-02
City                          
New York          30          32
Chicago           25          28
```

*   
  The data returns to a wide format.

---

# Key Differences Between Methods

| Method | Description | Best Use Case |
| ----- | ----- | ----- |
| `pivot()` | Reshape data without aggregation | Data with unique values for each combination of index and columns |
| `pivot_table()` | Reshape data with aggregation | Data with duplicates, needing aggregation |
| `stack()` | Convert columns to rows | Convert wide data to long format |
| `unstack()` | Convert rows to columns | Convert long data back to wide format |

---

# Conclusion

* Use **`pivot()`** for reshaping data without duplicates, perfect for a clean tabular representation.

* Use **`pivot_table()`** when you have duplicates and need to apply aggregation functions.

* Use **`stack()`** to transform wide data into a long format, making it easier to analyze or plot.

* Use **`unstack()`** to revert long data into a wide format for easier viewing or reporting.

