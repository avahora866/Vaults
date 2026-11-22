---
tags:
  - FORMAT
  - Python
---
# Merging and Joining in Pandas: `merge()` and `concat()`

In Pandas, combining data from multiple DataFrames is a common operation. You can achieve this using functions like **`merge()`** and **`concat()`**.

---

# Merging DataFrames Using `merge()`

## What is `merge()`?

* **`merge()`** is similar to SQL joins.

* It combines DataFrames based on common columns or indexes.

* You can specify different types of joins: **inner**, **left**, **right**, and **outer**.

## Syntax:

```py
pd.merge(left, right, how='type_of_join', on='common_column')
```

*   
  **`left`** and **`right`**: DataFrames to be merged.

* **`how`**: Type of join (`'inner'`, `'left'`, `'right'`, `'outer'`).

* **`on`**: Column(s) to merge on.

---

## Types of Joins in `merge()`

### Inner Join (Default)

* Returns only matching records from both DataFrames.

```py
import pandas as pd

df1 = pd.DataFrame({'Employee': ['Alice', 'Bob', 'Charlie'], 'Department': ['HR', 'Finance', 'IT']})
df2 = pd.DataFrame({'Employee': ['Alice', 'Bob', 'David'], 'Salary': [50000, 60000, 55000]})

 Perform an inner join
result = pd.merge(df1, df2, on='Employee', how='inner')
print(result)
```

**Output**:

```
  Employee Department  Salary
0   Alice        HR   50000
1     Bob    Finance   60000
```

---

### Left Join

* Returns all records from the left DataFrame and matching records from the right.

* Unmatched rows will have `NaN` for missing values.

```py
result = pd.merge(df1, df2, on='Employee', how='left')
print(result)
```

**Output**:

```
  Employee Department   Salary
0   Alice        HR  50000.0
1     Bob    Finance  60000.0
2 Charlie         IT      NaN
```

---

### Right Join

* Returns all records from the right DataFrame and matching records from the left.

```py
result = pd.merge(df1, df2, on='Employee', how='right')
print(result)
```

**Output**:

```
  Employee Department  Salary
0   Alice        HR   50000
1     Bob    Finance   60000
2   David       NaN   55000
```

---

### Outer Join

* Returns all records from both DataFrames. Missing values are filled with `NaN`.

```py
result = pd.merge(df1, df2, on='Employee', how='outer')
print(result)
```

**Output**:

```
  Employee Department   Salary
0   Alice        HR  50000.0
1     Bob    Finance  60000.0
2 Charlie         IT      NaN
3   David        NaN  55000.0
```

---

## Merging on Multiple Columns

You can merge using multiple columns by passing a list to the `on` parameter.

```py
df1 = pd.DataFrame({'Employee': ['Alice', 'Bob'], 'Department': ['HR', 'Finance'], 'Year': [2024, 2024]})
df2 = pd.DataFrame({'Employee': ['Alice', 'Bob'], 'Year': [2024, 2023], 'Salary': [50000, 58000]})

result = pd.merge(df1, df2, on=['Employee', 'Year'], how='inner')
print(result)
```

**Output**:

```
  Employee Department  Year  Salary
0   Alice        HR  2024  50000
```

---

# Concatenating DataFrames Using `concat()`

## What is `concat()`?

* **`concat()`** is used to stack DataFrames vertically or horizontally.

* It is useful for combining DataFrames with the same or similar columns.

## Syntax:

```py
pd.concat([df1, df2], axis=0, ignore_index=True)
```

*   
  **`axis=0`**: Vertical stacking (default).

* **`axis=1`**: Horizontal stacking (side-by-side).

* **`ignore_index=True`**: Reset the index after concatenation.

---

## Example 1: Vertical Concatenation

```py
df1 = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})
df2 = pd.DataFrame({'Name': ['Charlie', 'David'], 'Age': [35, 40]})

 Concatenate vertically
result = pd.concat([df1, df2], ignore_index=True)
print(result)
```

**Output**:

```
    Name  Age
0  Alice   25
1    Bob   30
2 Charlie   35
3  David   40
```

---

## Example 2: Horizontal Concatenation

```py
df1 = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})
df2 = pd.DataFrame({'City': ['New York', 'Chicago'], 'Salary': [50000, 60000]})

 Concatenate horizontally
result = pd.concat([df1, df2], axis=1)
print(result)
```

**Output**:

```
    Name  Age      City  Salary
0  Alice   25  New York  50000
1    Bob   30   Chicago  60000
```

---

## Example 3: Concatenating with Missing Columns

If DataFrames have different columns, missing columns will be filled with `NaN`.

```py
df1 = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})
df2 = pd.DataFrame({'Name': ['Charlie', 'David'], 'City': ['Austin', 'Boston']})

 Concatenate with missing columns
result = pd.concat([df1, df2], ignore_index=True)
print(result)
```

**Output**:

```
    Name   Age    City
0  Alice  25.0     NaN
1    Bob  30.0     NaN
2 Charlie   NaN  Austin
3  David    NaN  Boston
```

---

# Conclusion

* **`merge()`**: Best for combining DataFrames based on common columns using different types of joins (**inner**, **left**, **right**, **outer**).

* **`concat()`**: Best for stacking DataFrames vertically or horizontally, even with different columns.

* **Choose `merge()`** when you need to match specific data, and **`concat()`** when you want to append or extend data.