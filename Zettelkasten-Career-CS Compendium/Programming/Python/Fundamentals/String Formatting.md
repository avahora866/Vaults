---
tags:
  - Python
  - FORMAT
---
# String Formatting

Python provides multiple ways to format strings, but the most commonly used methods are **f-strings** and the **`.format()`** method.

## Using f-strings (Formatted String Literals)

Introduced in Python 3.6, **f-strings** are the most efficient and readable way to format strings.

```py
name = "Alice"
age = 25

# Using f-strings
formatted_string = f"My name is {name} and I am {age} years old."
print(formatted_string)
```

**Output:**

```
My name is Alice and I am 25 years old.
```

### Formatting Numbers in f-strings

```py
pi = 3.14159265
print(f"Pi rounded to 2 decimal places: {pi:.2f}")
```

**Output:**

```
Pi rounded to 2 decimal places: 3.14
```

---

## Using `.format()`

The `.format()` method provides another way to format strings.

```py
name = "Bob"
age = 30

formatted_string = "My name is {} and I am {} years old.".format(name, age)
print(formatted_string)
```

**Output:**

```
My name is Bob and I am 30 years old.
```

### Using Named Placeholders

```py
formatted_string = "My name is {name} and I am {age} years old.".format(name="Charlie", age=28)
print(formatted_string)
```

**Output:**

```
My name is Charlie and I am 28 years old.
```

### Formatting Numbers in `.format()`

```py
pi = 3.14159265
print("Pi rounded to 2 decimal places: {:.2f}".format(pi))
```

**Output:**

```
Pi rounded to 2 decimal places: 3.14
```

---


---



## Summary

| Feature | Example |
| ----- | ----- |
| **f-strings** | `f"My name is {name} and I am {age} years old."` |
| **.format()** | `"My name is {} and I am {}.".format(name, age)` |
| **map()** | `list(map(lambda x: x**2, numbers))` |
| **filter()** | `list(filter(lambda x: x > 30, numbers))` |
| **reduce()** | `reduce(lambda x, y: x + y, numbers)` |
| **sorted()** | `sorted(numbers)` |
| **Custom Sorting** | `sorted(words, key=len)` |

