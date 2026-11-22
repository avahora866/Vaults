---
tags:
  - Python
  - FORMAT
---


Python provides powerful functions like `map()`, `filter()`, and `reduce()` to handle large lists efficiently.

# Using `map()`

`map()` applies a function to each item in an iterable (e.g., a list).

```py
numbers = [1, 2, 3, 4, 5]

 Square each number in the list
squared = list(map(lambda x: x**2, numbers))
print(squared)
```

**Output:**

```
[1, 4, 9, 16, 25]
```

# Using `filter()`

`filter()` is used to filter elements in a list based on a condition.

```py
numbers = [10, 25, 32, 47, 50]

 Get numbers greater than 30
filtered_numbers = list(filter(lambda x: x > 30, numbers))
print(filtered_numbers)
```

**Output:**

```
[32, 47, 50]
```

# Using `reduce()`

`reduce()` (from the `functools` module) applies a function cumulatively to the elements of a list.

```py
from functools import reduce

numbers = [1, 2, 3, 4, 5]

 Sum of all numbers
sum_numbers = reduce(lambda x, y: x + y, numbers)
print(sum_numbers)
```

**Output:**

```
15
```
