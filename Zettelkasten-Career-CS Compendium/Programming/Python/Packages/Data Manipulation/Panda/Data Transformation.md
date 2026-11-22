---
tags:
  - FORMAT
  - Python
---
# Data Transformation in Pandas

Data transformation involves modifying, manipulating, and restructuring data to prepare it for analysis. Pandas provides various functions to perform transformations like adding, renaming, or dropping columns, as well as applying functions using **`apply()`**, **`map()`**, and **`applymap()`**.

---

# Adding Columns

You can easily add new columns to a DataFrame by assigning values to them.

## Example: Adding a Column

```py
import pandas as pd

 Create a DataFrame
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35]}
df = pd.DataFrame(data)

 Add a new column 'City'
df['City'] = ['New York', 'Chicago', 'San Francisco']

 Add a calculated column
df['Age in 5 Years'] = df['Age'] + 5

print(df)
```

**Output**:

```
     Name  Age           City  Age in 5 Years
0   Alice   25       New York              30
1     Bob   30       Chicago              35
2  Charlie   35  San Francisco              40
```

---

# Renaming Columns

You can rename columns using **`rename()`** by passing a dictionary that maps old column names to new ones.

## Example: Renaming Columns

```py
df.rename(columns={'Name': 'Full Name', 'Age': 'Current Age'}, inplace=True)
print(df)
```

**Output**:

```
  Full Name  Current Age           City  Age in 5 Years
0    Alice           25       New York              30
1      Bob           30       Chicago              35
2  Charlie           35  San Francisco              40
```

*   
  **`inplace=True`** applies the changes directly to the DataFrame.

---

# Dropping Columns

You can remove unnecessary columns using **`drop()`**.

## Example: Dropping Columns

```py
 Drop a single column
df_dropped = df.drop('Age in 5 Years', axis=1)
print(df_dropped)

 Drop multiple columns
df_dropped_multiple = df.drop(['City', 'Current Age'], axis=1)
print(df_dropped_multiple)
```

*   
  **`axis=1`** means dropping columns (for rows, use `axis=0`).

* `inplace=True` can be added if you want to modify the DataFrame directly.

---

# Applying Functions Using apply(), map(), and applymap()

## apply()

* Used for applying functions **along rows or columns** in a DataFrame.

* Works on both Series (one-dimensional) and DataFrame (two-dimensional).

### Example on a Column (Series):

```py
 Apply a lambda function to increase age by 10%
df['Age Adjusted'] = df['Current Age'].apply(lambda x: x * 1.1)
print(df)
```

**Output**:

```
  Full Name  Current Age           City  Age in 5 Years  Age Adjusted
0    Alice           25       New York              30          27.5
1      Bob           30       Chicago              35          33.0
2  Charlie           35  San Francisco              40          38.5
```

### Example on Rows:

```py
 Apply function along rows (axis=1)
def combine_info(row):
    return f"{row['Full Name']} from {row['City']}"

df['Info'] = df.apply(combine_info, axis=1)
print(df)
```

**Output**:

```
  Full Name  Current Age           City  Info
0    Alice           25       New York  Alice from New York
1      Bob           30       Chicago  Bob from Chicago
2  Charlie           35  San Francisco  Charlie from San Francisco
```

---

## map()

* **`map()`** is used **only on Series** (single columns) to apply a function or mapping.

* Useful for element-wise operations.

### Example:

```py
 Create a Series
s = pd.Series([1, 2, 3, 4, 5])

 Apply a function using map()
print(s.map(lambda x: x ** 2))
```

**Output**:

```
0     1
1     4
2     9
3    16
4    25
dtype: int64
```

### Mapping Values with a Dictionary:

```py
city_map = {'New York': 'NY', 'Chicago': 'CHI', 'San Francisco': 'SF'}
df['City Code'] = df['City'].map(city_map)
print(df)
```

**Output**:

```
  Full Name  Current Age           City City Code
0    Alice           25       New York       NY
1      Bob           30       Chicago      CHI
2  Charlie           35  San Francisco       SF
```

---

## applymap()

* **`applymap()`** is used to apply functions **element-wise** on every cell of a DataFrame.

* Best for DataFrames with numerical data.

### Example:

```py
 Example DataFrame with numerical values
df_numbers = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

 Apply a function to each element
print(df_numbers.applymap(lambda x: x ** 2))
```

**Output**:

```
    A   B
0   1  16
1   4  25
2   9  36
```

---

# Conclusion

* **Adding Columns**: Use `df['New Column'] = value`.

* **Renaming Columns**: Use `df.rename()`.

* **Dropping Columns**: Use `df.drop()`.

* **apply()**: Apply functions row-wise or column-wise, useful for complex logic.

* **map()**: Apply element-wise functions to Series or map values using a dictionary.

* **applymap()**: Apply element-wise functions to entire DataFrames, ideal for numerical data.

These functions make Pandas a versatile library for data transformation, enabling efficient data cleaning and manipulation.