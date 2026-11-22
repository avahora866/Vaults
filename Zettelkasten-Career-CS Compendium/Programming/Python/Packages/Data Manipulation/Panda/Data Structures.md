---
tags:
  - FORMAT
  - Python
---
# Pandas Data Structures: Series and DataFrame

Pandas is a powerful data manipulation library in Python that provides two primary data structures for working with data: **Series** and **DataFrame**. Understanding these structures is fundamental for data analysis using Pandas.

## Series

A **Series** is a one-dimensional labeled array that can hold data of any data type, including integers, floats, strings, and Python objects. It is similar to a column in an Excel spreadsheet or a single column in a DataFrame.

### Key Characteristics of Series:

* **Labels (Index)**: Each value has an associated label (index), which allows easy data access.

* **Homogeneous Data**: Typically holds data of a single data type.

* **Data Access**: Data can be accessed using index labels or positions.

* **Supports Operations**: Mathematical operations can be performed on Series.

### Example of Series:

```py
import pandas as pd

data = [10, 20, 30, 40]
index_labels = ['a', 'b', 'c', 'd']

series = pd.Series(data, index=index_labels)
print(series)
```

**Output:**

```
a    10
b    20
c    30
d    40
dtype: int64
```

You can access elements using:

```py
rint(series['b'])  # Output: 20
rint(series[2])    # Output: 30
```

---

## DataFrame

A **DataFrame** is a two-dimensional, tabular data structure similar to a spreadsheet or SQL table. It consists of rows and columns, where each column is represented as a Series.

### Key Characteristics of DataFrame:

* **Labeled Data**: Both rows and columns have labels (indices and column names).

* **Heterogeneous Data**: Each column can have a different data type.

* **Data Manipulation**: Supports data manipulation operations such as filtering, sorting, and aggregation.

* **Missing Data Handling**: Provides tools for handling missing data.

### Example of DataFrame:

```py
data = {
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'Chicago', 'San Francisco']
}

df = pd.DataFrame(data)
print(df)
```

**Output:**

```
     Name  Age           City
0   Alice   25       New York
1     Bob   30       Chicago
2  Charlie   35  San Francisco
```

You can access data using:

```py
rint(df['Name'])       # Access a single column
rint(df.loc[1])         # Access a row using the label
rint(df.iloc[2, 1])     # Access a specific element using row and column indices
```

---

## Differences Between Series and DataFrame

| Feature | Series | DataFrame |
| ----- | ----- | ----- |
| Dimensions | 1D (One-dimensional) | 2D (Two-dimensional) |
| Data Type | Homogeneous (Single data type) | Heterogeneous (Multiple data types) |
| Structure | Single column or array | Table with rows and columns |
| Indexing | Single index | Row and column indices |
| Example Use | Time series data | Tabular data like Excel files |

Both Series and DataFrame are essential tools in Pandas, and they often work together to perform complex data manipulations and analyses.