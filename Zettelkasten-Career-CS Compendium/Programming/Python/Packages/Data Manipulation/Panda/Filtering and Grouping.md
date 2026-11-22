---
tags:
  - FORMAT
  - Python
---
# Filtering and Grouping in Pandas: `query()` and `groupby()`

Filtering and grouping are essential operations in data analysis to extract specific subsets of data and aggregate information. Pandas provides efficient functions like **`query()`** for filtering and **`groupby()`** for grouping.

---

# Filtering Data Using `query()`

## What is `query()`?

* **`query()`** is a convenient method to filter rows using a string expression.

* It provides a cleaner and more readable alternative to using conditional statements.

## Syntax:

```py
DataFrame.query('expression')
```

*   
  **Expression**: A boolean condition (e.g., `"Age > 30"` or `"City == 'New York'"`).

* Variable names in the DataFrame are used directly without using `df['column_name']`.

---

## Example: Using `query()`

```py
import pandas as pd

 Create DataFrame
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    'Age': [25, 30, 35, 40, 28],
    'City': ['New York', 'Chicago', 'San Francisco', 'Boston', 'Austin']
}
df = pd.DataFrame(data)

 Filter rows where Age > 30
filtered_df = df.query('Age > 30')
print(filtered_df)
```

**Output**:

```
     Name  Age           City
2  Charlie   35  San Francisco
3   David   40         Boston
```

---

## Using Multiple Conditions

You can combine multiple conditions using logical operators:

* **`&`** for AND

* **`|`** for OR

* **`~`** for NOT

```py
 Filter rows where Age > 30 and City is not Chicago
result = df.query('Age > 30 & City != "Chicago"')
print(result)
```

**Output**:

```
     Name  Age           City
2  Charlie   35  San Francisco
3   David   40         Boston
```

---

## Using Variables in Query

You can pass variables using `@` in the query.

```py
age_limit = 28
city_name = 'Austin'
result = df.query('Age >= @age_limit & City == @city_name')
print(result)
```

**Output**:

```
  Name  Age   City
4  Eve   28  Austin
```

---

# Grouping Data Using `groupby()`

## What is `groupby()`?

* **`groupby()`** is used to group data based on one or more columns.

* After grouping, you can perform aggregation functions like **sum**, **mean**, **count**, **max**, or **min**.

## Syntax:

```py
DataFrame.groupby(by='column_name')[['column_to_aggregate']].agg('aggregation_function')
```

*   
  **`by`**: Column or list of columns to group by.

* **`agg()`**: Apply aggregation functions.

---

## Example: Grouping and Aggregating

```py
data = {
    'Department': ['HR', 'Finance', 'HR', 'Finance', 'HR'],
    'Employee': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    'Salary': [50000, 60000, 55000, 62000, 48000]
}
df = pd.DataFrame(data)

 Group by Department and calculate average salary
grouped_df = df.groupby('Department')['Salary'].mean()
print(grouped_df)
```

**Output**:

```
Department
Finance    61000.0
HR         51000.0
Name: Salary, dtype: float64
```

---

## Multiple Aggregations

You can apply multiple aggregation functions using `.agg()`.

```py
 Calculate both average and maximum salary by department
grouped_df = df.groupby('Department')['Salary'].agg(['mean', 'max'])
print(grouped_df)
```

**Output**:

```
            mean    max
Department              
Finance   61000.0  62000
HR        51000.0  55000
```

---

## Grouping by Multiple Columns

You can group by multiple columns by passing a list.

```py
 Group by Department and Employee
grouped_df = df.groupby(['Department', 'Employee'])['Salary'].sum()
print(grouped_df)
```

**Output**:

```
Department  Employee
Finance     Bob         60000
            David       62000
HR          Alice       50000
            Charlie     55000
            Eve         48000
Name: Salary, dtype: int64
```

---

## Using Grouped Data for Further Analysis

You can iterate over the groups using a `for` loop:

```py
for department, group in df.groupby('Department'):
    print(f"\nDepartment: {department}")
    print(group)
```

**Output**:

```
Department: Finance
  Department Employee  Salary
1   Finance     Bob   60000
3   Finance   David   62000

Department: HR
  Department Employee  Salary
0       HR   Alice     50000
2       HR  Charlie    55000
4       HR     Eve     48000
```

---

# Conclusion

* **`query()`**: Use for filtering data with clean, readable expressions. It supports multiple conditions and variable interpolation using `@`.

* **`groupby()`**: Use for grouping data by one or more columns and applying aggregation functions like **mean**, **sum**, **count**, and more.

* Combining **`query()`** and **`groupby()`** allows for powerful data manipulation and analysis in Pandas.