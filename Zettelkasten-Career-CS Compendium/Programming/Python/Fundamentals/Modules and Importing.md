---
tags:
  - Python
  - FORMAT
---
# Modules and Importing

Python allows organizing code into **modules** to improve maintainability and reuse.

## Standard Library Modules

Python provides built-in modules like `math`, `random`, and `datetime`.

```py
import math

print(math.sqrt(16))  # Output: 4.0
print(math.pi)        # Output: 3.141592653589793
```

```py
import random

print(random.randint(1, 10))  # Generates a random number between 1 and 10
```

## Creating and Importing Custom Modules

You can create your own module by saving Python functions in a `.py` file.

### Step 1: Create a Module (`my_module.py`)

```py
# my_module.py
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b
```

### Step 2: Import and Use the Module

```py
import my_module

print(my_module.add(3, 5))      # Output: 8
print(my_module.multiply(4, 6)) # Output: 24
```

## Importing Specific Functions

```py
from my_module import add

print(add(10, 20))  # Output: 30
```

## Using `as` to Rename Imports

```py
import my_module as mm

print(mm.add(2, 3))  # Output: 5
```

## Importing Everything (``)

```py
from my_module import *

print(add(3, 3))        # Output: 6
print(multiply(2, 5))   # Output: 10
```

However, it's better to explicitly import only what is needed for clarity.

## Installing and Importing External Modules

For third-party modules, use `pip`:

```
pip install requests
```

Then, use it in Python:

```py
import requests

response = requests.get("https://api.github.com")
print(response.status_code)
```

---

## Summary

| Concept | Example |
| ----- | ----- |
| Basic Function | `def greet(name): return f"Hello, {name}!"` |
| Default Arguments | `def greet(name="Guest"):` |
| `*args` (Multiple Arguments) | `def sum_numbers(*args):` |
| `**kwargs` (Keyword Arguments) | `def person_info(**kwargs):` |
| Import Standard Module | `import math` |
| Create & Import Custom Module | `import my_module` |
| Import Specific Function | `from my_module import add` |
| Install & Use External Module | `pip install requests` |

