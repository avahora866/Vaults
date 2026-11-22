---
tags:
  - Python
  - FORMAT
---
# Advanced Data Manipulation in Pandas

Pandas provides advanced data manipulation techniques for handling complex datasets efficiently. Key techniques include **windowing operations** (like rolling and expanding), **multi-indexing** for hierarchical data, and **advanced merging techniques** for combining datasets.

---

# Windowing Operations: Rolling and Expanding

Windowing operations involve applying functions over a sliding or expanding window of data, often used for time series analysis.

## Rolling Window Operations

* **`rolling()`** creates a sliding window of a fixed size to perform calculations like mean, sum, or standard deviation.

* Useful for **moving averages** and **smoothing** data.

### Syntax:

```py
DataFrame.rolling(window, min_periods=1).agg(func)
```

*   
  **`window`**: Number of observations in the window.

* **`min_periods`**: Minimum number of non-NA values required.

### Example: Rolling Mean

```py
import pandas as pd

data = {'Date': pd.date_range(start='2024-03-01', periods=10),
        'Value': [10, 15, 20, 25, 30, 35, 40, 45, 50, 55]}
df = pd.DataFrame(data).set_index('Date')

 Calculate 3-day rolling mean
df['Rolling_Mean'] = df['Value'].rolling(window=3).mean()
print(df)
```

**Output**:

```
            Value  Rolling_Mean
Date                            
2024-03-01     10           NaN
2024-03-02     15           NaN
2024-03-03     20          15.0
2024-03-04     25          20.0
2024-03-05     30          25.0
```

*   
  The first two values are `NaN` due to insufficient data for the window.

---

## Expanding Window Operations

* **`expanding()`** calculates cumulative metrics by expanding the window from the start of the data to the current point.

* Useful for cumulative sums, averages, and cumulative returns.

### Example: Expanding Mean

```py
df['Expanding_Mean'] = df['Value'].expanding().mean()
print(df)
```

**Output**:

```
            Value  Expanding_Mean
Date                             
2024-03-01     10            10.0
2024-03-02     15            12.5
2024-03-03     20            15.0
2024-03-04     25            17.5
```

*   
  The cumulative mean updates with each new data point.

---

# Multi-Indexing

## What is Multi-Indexing?

* Multi-Indexing allows you to work with multiple levels of row and column labels.

* Useful for representing hierarchical data.

## Creating a Multi-Index

You can create a Multi-Index using `pd.MultiIndex.from_arrays()` or directly using the `set_index()` method.

### Example: Multi-Index DataFrame

```py
arrays = [['New York', 'New York', 'Chicago', 'Chicago'],
          ['2024-03-01', '2024-03-02', '2024-03-01', '2024-03-02']]
index = pd.MultiIndex.from_arrays(arrays, names=('City', 'Date'))

data = {'Temperature': [30, 32, 25, 28]}
df = pd.DataFrame(data, index=index)
print(df)
```

**Output**:

```
                 Temperature
City     Date               
New York 2024-03-01         30
         2024-03-02         32
Chicago  2024-03-01         25
         2024-03-02         28
```

*   
  Data is neatly organized by city and date.

---

## Accessing Data with Multi-Index

You can use `.loc[]` to access specific data using multiple levels.

### Example:

```py
print(df.loc['New York', '2024-03-01'])
```

**Output**:

```
Temperature    30
Name: (New York, 2024-03-01), dtype: int64
```

---

# Advanced Merging Techniques

Pandas provides powerful functions for merging data using `merge()`, supporting complex joins.

## Merging on Multiple Columns

* You can merge on multiple columns using `on` with a list of column names.

### Example:

```py
df1 = pd.DataFrame({'City': ['New York', 'Chicago'], 'Date': ['2024-03-01', '2024-03-02'], 'Temp': [30, 25]})
df2 = pd.DataFrame({'City': ['New York', 'Chicago'], 'Date': ['2024-03-01', '2024-03-02'], 'Humidity': [80, 75]})

 Merge on multiple columns
merged_df = pd.merge(df1, df2, on=['City', 'Date'])
print(merged_df)
```

**Output**:

```
       City       Date  Temp  Humidity
0  New York 2024-03-01    30        80
1  Chicago  2024-03-02    25        75
```

---

## Using Different Join Types

* `merge()` supports different join operations:

  * **Inner Join**: Default; returns matching records.

  * **Left Join**: All records from the left DataFrame and matching records from the right.

  * **Right Join**: All records from the right and matching records from the left.

  * **Outer Join**: Returns all records with missing data filled with `NaN`.

### Example: Left Join

```py
left_df = pd.DataFrame({'Key': ['A', 'B', 'C'], 'Value1': [1, 2, 3]})
right_df = pd.DataFrame({'Key': ['B', 'C', 'D'], 'Value2': [4, 5, 6]})

left_join_df = pd.merge(left_df, right_df, on='Key', how='left')
print(left_join_df)
```

**Output**:

```
  Key  Value1  Value2
0   A       1     NaN
1   B       2     4.0
2   C       3     5.0
```

*   
  Missing data for `Key = A` results in `NaN`.

---

## Merging with Multi-Index

You can merge dataframes with Multi-Index using `merge()` or `join()`.

### Example: Multi-Index Merge

```py
df1 = df1.set_index(['City', 'Date'])
df2 = df2.set_index(['City', 'Date'])
merged_df = df1.join(df2)
print(merged_df)
```

**Output**:

```
                 Temp  Humidity
City     Date                  
New York 2024-03-01    30       80
Chicago  2024-03-02    25       75
```

---

# Conclusion

* **Windowing Operations**:

  * Use `rolling()` for sliding windows and `expanding()` for cumulative metrics.

* **Multi-Indexing**:

  * Efficient for hierarchical data with multi-level labels, allowing easy access and manipulation.

* **Advanced Merging**:

  * `merge()` supports complex joins (inner, outer, left, right) and multi-column or multi-index merges.

These advanced techniques enhance your ability to manipulate and analyze complex datasets effectively in Pandas.

