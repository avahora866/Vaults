---
tags:
  - Python
  - FORMAT
---
# Data Selection in Pandas: `loc[]` and `iloc[]`

In **Pandas**, selecting data from a DataFrame can be done using two main methods: **`loc[]`** and **`iloc[]`**. Both are used to retrieve specific rows and columns, but they differ in how they operate.

---

## loc\[\]: Label-Based Selection

The **`loc[]`** method is used for selecting data by **labels**. It uses the **row and column labels** (i.e., the index and column names) to fetch data.

### Syntax:

```py
DataFrame.loc[row_label, column_label]
```

*   
  **row\_label**: Row index label or a range of labels.

* **column\_label**: Column name or a list of column names.

* You can also use `:` to select all rows or columns.

### Example:

```py
import pandas as pd

 Create a DataFrame
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    'Age': [25, 30, 35, 40, 28],
    'City': ['New York', 'Chicago', 'San Francisco', 'Boston', 'Austin']
}
df = pd.DataFrame(data, index=['A', 'B', 'C', 'D', 'E'])

print(df)
```

**Output**:

```
     Name  Age           City
A   Alice   25       New York
B     Bob   30       Chicago
C  Charlie   35  San Francisco
D   David   40        Boston
E     Eve   28        Austin
```

### Selecting a Single Row:

```py
print(df.loc['C'])
```

**Output**:

```
Name    Charlie
Age          35
City  San Francisco
Name: C, dtype: object
```

### Selecting Multiple Rows and Specific Columns:

```py
print(df.loc[['B', 'D'], ['Name', 'City']])
```

**Output**:

```
   Name    City
B   Bob  Chicago
D David  Boston
```

### Selecting a Range of Rows:

```py
print(df.loc['B':'D', 'Name':'Age'])
```

**Output**:

```
     Name  Age
B     Bob   30
C  Charlie   35
D   David   40
```

---

## iloc\[\]: Integer-Based Selection

The **`iloc[]`** method is used for selecting data by **integer index positions**. It works like traditional array indexing in Python.

### Syntax:

```py
DataFrame.iloc[row_index, column_index]
```

*   
  **row\_index**: Integer index position of the row.

* **column\_index**: Integer index position of the column.

### Example:

### Selecting a Single Row by Index:

```py
rint(df.iloc[2])  # Select third row (index 2)
```

**Output**:

```
Name    Charlie
Age          35
City  San Francisco
Name: C, dtype: object
```

### Selecting Specific Rows and Columns:

```py
rint(df.iloc[[1, 3], [0, 2]])  # Select 2nd and 4th rows, 1st and 3rd columns
```

**Output**:

```
   Name    City
B   Bob  Chicago
D David  Boston
```

### Selecting a Range Using Slicing:

```py
rint(df.iloc[1:4, 0:2])  # Rows from 1 to 3 and Columns from 0 to 1
```

**Output**:

```
     Name  Age
B     Bob   30
C  Charlie   35
D   David   40
```

---

## Differences Between `loc[]` and `iloc[]`

| Feature | `loc[]` (Label-Based) | `iloc[]` (Integer-Based) |
| ----- | ----- | ----- |
| Selection Type | Uses labels (row/column names) | Uses integer-based index positions |
| Indexing Range | Includes both endpoints | Excludes the endpoint (like Python slicing) |
| Performance | Slightly slower with large datasets | Faster as it directly accesses by index |
| Error Handling | Throws a `KeyError` for invalid labels | Throws an `IndexError` for invalid positions |
| Use Case | When labels are known | When working with numeric index positions |

---

## Conclusion

* Use **`loc[]`** when selecting data based on labels (row or column names).

* Use **`iloc[]`** when selecting data based on index positions.

* Both methods support slicing, selecting specific rows or columns, and working with conditions.

* Combining them effectively is essential for data manipulation and analysis in Pandas.

