---
tags:
  - Python
  - FORMAT
---
Functions allow us to reuse blocks of code.

# Defining a Function

```py
def greet(name):
    return f"Hello, {name}!"

int(greet("Alice"))  # Output: Hello, Alice!
```

# Function with Multiple Parameters

```py
def add(a, b):
    return a + b

int(add(3, 5))  # Output: 8
```

# Lambda Function (One-liner Anonymous Function)

```py
square = lambda x: x * x
int(square(4))  # Output: 16
```

# Lambda with Multiple Arguments

```py
multiply = lambda a, b: a * b
int(multiply(3, 4))  # Output: 12
```

