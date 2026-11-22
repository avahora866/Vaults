---
tags:
  - FORMAT
  - Python
---
# Data Cleaning in Pandas: Handling Null Values and Duplicates

Data cleaning is an essential step in data preprocessing, ensuring the dataset is accurate and ready for analysis. Pandas provides efficient methods to handle missing values (**null values**) and remove duplicate data.

---

# Handling Null Values

Null values in Pandas are represented as **`NaN`** (Not a Number). They can occur due to incomplete data, errors during data collection, or data extraction issues.

## Detecting Null Values

* **`isnull()`**: Returns a DataFrame of boolean values indicating `True` for null values.

* **`notnull()`**: Returns `True` for non-null values.

### Example:

```py
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', None, 'David', 'Eve'],
    'Age': [25, 30, 35, None, 28],
    'City': ['New York', None, 'San Francisco', 'Boston', 'Austin']
}

df = pd.DataFrame(data)
print(df)
```

**Output**:

```
    Name   Age           City
0  Alice  25.0       New York
1    Bob  30.0           None
2   None  35.0  San Francisco
3  David   NaN         Boston
4    Eve  28.0         Austin
```

### Detecting Null Values:

```py
print(df.isnull())
print(df.notnull())
```

---

## Removing Null Values

* **`dropna()`**: Removes rows or columns containing `NaN` values.

### Example:

```py
 Remove rows with any null values
cleaned_df = df.dropna()
print(cleaned_df)
```

**Output**:

```
    Name   Age      City
0  Alice  25.0  New York
4    Eve  28.0    Austin
```

### Additional Options:

* `axis=0`: Remove rows with null values (default).

* `axis=1`: Remove columns with null values.

* `how='all'`: Remove rows/columns only if all values are `NaN`.

* `thresh=n`: Keep rows/columns with at least `n` non-null values.

---

## Filling Null Values

* **`fillna()`**: Replaces `NaN` values with specified values.

### Example:

```py
 Fill missing values with a specific value
df_filled = df.fillna({'Name': 'Unknown', 'Age': df['Age'].mean(), 'City': 'Unknown'})
print(df_filled)
```

**Output**:

```
      Name   Age           City
0    Alice  25.0       New York
1      Bob  30.0        Unknown
2  Unknown  35.0  San Francisco
3    David  29.5         Boston
4      Eve  28.0         Austin
```

*   
  **`df['Age'].mean()`**: Replaces missing ages with the column's mean value.

---

# Handling Duplicate Data

Duplicate data can lead to misleading results in analysis. Pandas provides functions to detect and remove duplicates.

## Detecting Duplicates

* **`duplicated()`**: Returns a boolean series, marking `True` for duplicate rows (excluding the first occurrence).

### Example:

```py
data = {
    'Name': ['Alice', 'Bob', 'Alice', 'David', 'Bob'],
    'Age': [25, 30, 25, 40, 30],
    'City': ['New York', 'Chicago', 'New York', 'Boston', 'Chicago']
}

df = pd.DataFrame(data)
print(df)
```

**Output**:

```
    Name  Age      City
0  Alice   25  New York
1    Bob   30  Chicago
2  Alice   25  New York
3  David   40   Boston
4    Bob   30  Chicago
```

### Detecting Duplicates:

```py
print(df.duplicated())
```

**Output**:

```
0    False
1    False
2     True
3    False
4     True
dtype: bool
```

---

## Removing Duplicates

* **`drop_duplicates()`**: Removes duplicate rows from the DataFrame.

### Example:

```py
cleaned_df = df.drop_duplicates()
print(cleaned_df)
```

**Output**:

```
    Name  Age      City
0  Alice   25  New York
1    Bob   30  Chicago
3  David   40   Boston
```

### Additional Options:

* `keep='first'`: Keeps the first occurrence (default).

* `keep='last'`: Keeps the last occurrence.

* `keep=False`: Removes all duplicates.

```py
 Remove all instances of duplicates
df_no_duplicates = df.drop_duplicates(keep=False)
print(df_no_duplicates)
```

**Output**:

```
    Name  Age    City
3  David   40  Boston
```

---

# Conclusion

* Use **`isnull()`** and **`notnull()`** to detect missing data.

* Use **`dropna()`** to remove rows or columns with null values.

* Use **`fillna()`** to replace missing values with meaningful data.

* Use **`duplicated()`** to find duplicates and **`drop_duplicates()`** to remove them.

* Data cleaning ensures data is accurate, complete, and ready for analysis.