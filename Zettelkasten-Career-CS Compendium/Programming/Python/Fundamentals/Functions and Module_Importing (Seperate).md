---
tags:
  - Python
  - FORMAT
---
# Functions

Functions in Python allow code reuse, making programs more modular and easier to maintain.

## Writing Reusable Functions

A function is defined using the `def` keyword. It can take parameters and return values.

```py
# Simple function
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))
```

Functions can have default parameters:

```py
def greet(name="Guest"):
    return f"Hello, {name}!"

print(greet())       # Output: Hello, Guest!
print(greet("Bob"))  # Output: Hello, Bob!
```

## Function Arguments (`args`, `kwargs`)

### Using `args` (Arbitrary Positional Arguments)

`*args` allows a function to accept multiple positional arguments as a tuple.

```py
def sum_numbers(*args):
    return sum(args)

print(sum_numbers(1, 2, 3, 4))  # Output: 10
```

### Using `kwargs` (Arbitrary Keyword Arguments)

`**kwargs` allows passing multiple keyword arguments as a dictionary.

```py
def person_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

person_info(name="Alice", age=25, city="New York")
```

### Combining `args` and `kwargs`

You can mix both in a function.

```py
def show_details(name, *args, **kwargs):
    print(f"Name: {name}")
    print(f"Additional info: {args}")
    print(f"Extra details: {kwargs}")

show_details("Alice", "Engineer", 30, city="New York", country="USA")
```

---

