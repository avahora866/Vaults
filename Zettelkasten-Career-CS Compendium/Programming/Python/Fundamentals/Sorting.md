---
tags:
  - Python
  - FORMAT
---
# Sorting

Python provides the `sorted()` function, which allows sorting lists with custom key functions.

## Sorting a List of Numbers

```py
numbers = [5, 2, 8, 1, 9]
sorted_numbers = sorted(numbers)
print(sorted_numbers)
```

**Output:**

```
[1, 2, 5, 8, 9]
```

## Sorting a List of Strings Alphabetically

```py
words = ["banana", "apple", "cherry"]
sorted_words = sorted(words)
print(sorted_words)
```

**Output:**

```
['apple', 'banana', 'cherry']
```

## Sorting with a Custom Key Function

### Sorting by String Length

```py
words = ["banana", "apple", "cherry", "kiwi"]
sorted_by_length = sorted(words, key=len)
print(sorted_by_length)
```

**Output:**

```
['kiwi', 'apple', 'banana', 'cherry']
```

### Sorting a List of Dictionaries

```py
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]

# Sort by grade
sorted_students = sorted(students, key=lambda student: student["grade"])
print(sorted_students)
```

**Output:**

```
[{'name': 'Charlie', 'grade': 78}, {'name': 'Alice', 'grade': 85}, {'name': 'Bob', 'grade': 92}]
```

### Sorting in Descending Order

```py
numbers = [5, 2, 8, 1, 9]
sorted_desc = sorted(numbers, reverse=True)
print(sorted_desc)
```

**Output:**

```
[9, 8, 5, 2, 1]
```

---