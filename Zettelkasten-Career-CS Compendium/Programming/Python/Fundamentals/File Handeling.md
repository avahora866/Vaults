---
tags:
  - Python
---
Python provides built-in support for file handling with functions like `open()`, `read()`, `write()`, and libraries such as `csv` and `json` for structured data formats.

---

# Reading and Writing Files

## TXT Files (Plain Text)

Text files can be read and written using the built-in `open()` function.

## Writing to a TXT File:

```python
# Writing to a text file
with open("example.txt", "w") as file:
    file.write("Hello, this is a text file!\n")
    file.write("Python makes file handling easy.")
```

## Reading from a TXT File:

```python
# Reading from a text file
with open("example.txt", "r") as file:
    content = file.read()
    print(content)
```

## Reading Line by Line:

```python
with open("example.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() removes newline characters
```

---

## CSV Files (Comma-Separated Values)

CSV files are structured text files that store tabular data. Python’s `csv` module provides tools to read and write them.

## Writing to a CSV File:

```python
import csv

data = [
    ["Name", "Age", "City"],
    ["Alice", 25, "New York"],
    ["Bob", 30, "Los Angeles"],
    ["Charlie", 35, "Chicago"]
]

with open("example.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerows(data)
```

## Reading from a CSV File:

```python
with open("example.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

## Using `DictWriter` and `DictReader`:

```python
# Writing CSV as dictionary
with open("example_dict.csv", "w", newline="") as file:
    fieldnames = ["Name", "Age", "City"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    
    writer.writeheader()
    writer.writerow({"Name": "Alice", "Age": 25, "City": "New York"})
    writer.writerow({"Name": "Bob", "Age": 30, "City": "Los Angeles"})
```

```python
# Reading CSV as dictionary
with open("example_dict.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row)
```

---

## JSON Files (JavaScript Object Notation)

JSON is a popular format for storing structured data. Python’s `json` module makes it easy to work with JSON files.

## Writing to a JSON File:

```python
import json

data = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

with open("example.json", "w") as file:
    json.dump(data, file, indent=4)  # `indent=4` makes it readable
```

## Reading from a JSON File:

```python
with open("example.json", "r") as file:
    data = json.load(file)
    print(data)
```

---

# Handling Large Files Efficiently

When working with large files, reading the entire file at once can be inefficient. Instead, we can use generators, streaming, or chunk processing.

## Using Generators (Efficient Line-by-Line Processing)

Instead of reading the whole file into memory, read line by line:

```python
def read_large_file(file_path):
    with open(file_path, "r") as file:
        for line in file:
            yield line.strip()  # Yield one line at a time

# Usage
for line in read_large_file("large_file.txt"):
    print(line)
```

## Reading Large CSV Files in Chunks

Pandas can be used to read large CSV files in chunks:

```python
import pandas as pd

chunk_size = 1000  # Read 1000 rows at a time
for chunk in pd.read_csv("large_file.csv", chunksize=chunk_size):
    print(chunk.head())  # Process chunk
```

## Streaming JSON Data (Efficient for Large JSON Files)

Instead of loading a large JSON file into memory at once, stream it:

```python
import json

def stream_large_json(file_path):
    with open(file_path, "r") as file:
        for line in file:
            yield json.loads(line)  # Process one JSON object at a time

# Usage
for item in stream_large_json("large.json"):
    print(item)
```

---

# Summary

| File Type | Writing | Reading | Efficient Handling |
| ----- | ----- | ----- | ----- |
| TXT | `write()`, `writelines()` | `read()`, `readline()`, `readlines()` | Line-by-line processing with `yield` |
| CSV | `csv.writer()`, `DictWriter()` | `csv.reader()`, `DictReader()` | `pandas.read_csv(chunksize=...)` |
| JSON | `json.dump()` | `json.load()` | Streaming with `yield json.loads(line)` |

