---
tags:
  - FORMAT
  - Python
---
# Data Inspection in Pandas

Once you have loaded data into a Pandas **DataFrame** or **Series**, inspecting it is crucial for understanding its structure and contents. Pandas provides several built-in methods for this purpose.

## head()

The `head()` method is used to display the first few rows of a DataFrame. By default, it shows the top **5 rows**, but you can specify the number of rows as needed.

### Syntax:

```py
DataFrame.head(n=5)
```

*   
  `n`: Number of rows to display (default is `5`).

### Example:

```py
import pandas as pd

 Example DataFrame
data = {'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
        'Age': [25, 30, 35, 40, 28],
        'City': ['New York', 'Chicago', 'San Francisco', 'Boston', 'Austin']}
df = pd.DataFrame(data)

print(df.head())
```

**Output**:

```
     Name  Age           City
0   Alice   25       New York
1     Bob   30       Chicago
2  Charlie   35  San Francisco
3   David   40        Boston
4     Eve   28        Austin
```

---

## tail()

The `tail()` method is similar to `head()`, but it displays the last few rows of the DataFrame.

### Syntax:

```py
DataFrame.tail(n=5)
```

*   
  `n`: Number of rows to display (default is `5`).

### Example:

```py
print(df.tail(3))
```

**Output**:

```
     Name  Age           City
2  Charlie   35  San Francisco
3   David   40        Boston
4     Eve   28        Austin
```

---

## info()

The `info()` method provides a concise summary of the DataFrame, including:

* Number of rows and columns

* Column names

* Data types of each column

* Memory usage

### Syntax:

```py
DataFrame.info()
```

### Example:

```py
df.info()
```

**Output**:

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5 entries, 0 to 4
Data columns (total 3 columns):
#   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   Name    5 non-null      object
 1   Age     5 non-null      int64
 2   City    5 non-null      object
dtypes: int64(1), object(2)
memory usage: 248.0 bytes
```

*   
  **Non-Null Count**: Helps identify missing values.

* **Dtype**: Shows the data type of each column.

* **Memory Usage**: Useful for large datasets.

---

## describe()

The `describe()` method provides summary statistics for numeric columns by default. It includes metrics like:

* **Count**: Number of non-null values

* **Mean**: Average of values

* **Standard Deviation (std)**: Spread of data

* **Min and Max**: Minimum and maximum values

* **Percentiles (25%, 50%, 75%)**: Quartile values

### Syntax:

```py
DataFrame.describe(include=None)
```

*   
  `include='all'`: Provide statistics for all columns, including categorical data.

### Example:

```py
df.describe()
```

**Output**:

```
             Age
count   5.000000
mean   31.600000
std     5.979130
min    25.000000
25%    28.000000
50%    30.000000
75%    35.000000
max    40.000000
```

### For all columns including categorical data:

```py
df.describe(include='all')
```

**Output**:

```
        Name  Age      City
count      5    5         5
unique     5    5         5
top     Alice   25  New York
freq       1    1         1
```

*   
  **Unique**: Number of unique values

* **Top**: Most frequent value

* **Freq**: Frequency of the most common value

---

## Conclusion

* **`head()`** and **`tail()`** are ideal for quickly checking the beginning and end of data.

* **`info()`** is great for getting an overview of the dataset’s structure.

* **`describe()`** helps to get statistical summaries, especially for numeric data.