---
tags:
  - Python
  - FORMAT
---
# Variables

A variable is a name that stores a value. In Python, you don’t need to declare the type explicitly.

```py
= 10  # Integer
= 3.14  # Float
me = "Alice"  # String
_valid = True  # Boolean
```

# Data Types

## Integer (`int`)

Whole numbers (positive, negative, or zero).

```py
num = 42
int(type(num))  # Output: <class 'int'>
```

## Float (`float`)

Numbers with decimal points.

```py
pi = 3.14
int(type(pi))  # Output: <class 'float'>
```

## String (`str`)

A sequence of characters enclosed in quotes.

```py
text = "Hello, World!"
int(type(text))  # Output: <class 'str'>
```

## Boolean (`bool`)

Represents `True` or `False`.

```py
is_raining = False
int(type(is_raining))  # Output: <class 'bool'>
```

## List (`list`)

An ordered collection of items (mutable).

```py
fruits = ["apple", "banana", "cherry"]
uits.append("orange")  # Adds an item to the list
int(fruits)  # Output: ['apple', 'banana', 'cherry', 'orange']
```

## Tuple (`tuple`)

An ordered collection (immutable).

```py
colors = ("red", "green", "blue")
int(colors[0])  # Output: red
```

## Dictionary (`dict`)

A collection of key-value pairs.

```py
person = {"name": "Alice", "age": 25}
int(person["name"])  # Output: Alice
```

## Set (`set`)

An unordered collection of unique items.

```py
unique_numbers = {1, 2, 3, 3, 2, 1}
int(unique_numbers)  # Output: {1, 2, 3}
```
