---
tags:
  - FORMAT
  - Python
---
# Loading Data in Pandas

Pandas provides powerful functions to load data from various sources into its data structures like **DataFrame** or **Series**. Some of the most common sources include CSV files, Excel files, SQL databases, and other data formats.

## Loading Data from CSV Files

**CSV (Comma Separated Values)** files are one of the most common formats for storing tabular data.

### Using `pd.read_csv()`

```py
import pandas as pd

 Load CSV file into DataFrame
df = pd.read_csv('data.csv')

 Display the first few rows of the DataFrame
print(df.head())
```

### Key Parameters:

* `filepath_or_buffer`: Path to the CSV file.

* `sep`: Specify the delimiter (e.g., `','`, `'\t'`).

* `header`: Specify which row to use as column names (default is the first row).

* `index_col`: Column to use as the index.

* `usecols`: Load specific columns.

* `dtype`: Set the data type for specific columns.

### Example:

```py
df = pd.read_csv('data.csv', sep=',', index_col=0, usecols=['Name', 'Age'], dtype={'Age': int})
```

---

## Loading Data from Excel Files

Excel files with `.xlsx` or `.xls` extensions can also be loaded using Pandas.

### Using `pd.read_excel()`

```py
 Load Excel file into DataFrame
df = pd.read_excel('data.xlsx', sheet_name='Sheet1')

print(df.head())
```

### Key Parameters:

* `io`: Path to the Excel file.

* `sheet_name`: Specify the sheet name or index (default is the first sheet).

* `usecols`: Read specific columns.

* `dtype`: Specify data types.

### Example:

```py
df = pd.read_excel('data.xlsx', sheet_name='Sales', usecols=['Product', 'Revenue'])
```

---

## Loading Data from SQL Databases

Pandas can read data from SQL databases using SQLAlchemy or database connectors like `sqlite3`.

### Using `pd.read_sql()`

```py
import sqlite3

 Connect to SQLite database
conn = sqlite3.connect('database.db')

 Run SQL query and load into DataFrame
df = pd.read_sql('SELECT * FROM sales', conn)

print(df.head())
```

### Key Parameters:

* `sql`: SQL query or table name.

* `con`: Database connection object.

* `index_col`: Column to use as the index.

* `parse_dates`: Automatically parse date columns.

---

## Loading Data from Other Sources

Pandas also supports loading data from several other formats, including:

* **JSON:** `pd.read_json('data.json')`

* **HTML Tables:** `pd.read_html('https://example.com')`

* **Parquet:** `pd.read_parquet('data.parquet')`

* **HDF5:** `pd.read_hdf('data.h5')`

Each function has specific parameters for customization, allowing efficient data loading and preprocessing.