---
tags:
  - Python
  - FORMAT
---
# Error Handling

Python provides error handling mechanisms using `try-except` blocks to catch and handle exceptions without crashing the program.

## Try-Except Blocks

### Basic Try-Except

```py
try:
    result = 10 / 0  # This will raise a ZeroDivisionError
except ZeroDivisionError:
    print("Error: Cannot divide by zero.")
```

### Catching Multiple Exceptions

```py
try:
    num = int(input("Enter a number: "))
    result = 10 / num
except ZeroDivisionError:
    print("Error: Division by zero is not allowed.")
except ValueError:
    print("Error: Invalid input. Please enter a number.")
```

### Using `else` and `finally`

* `else` runs if no exception occurs.  
* `finally` runs whether or not an exception occurs.

```py
try:
    file = open("example.txt", "r")
    content = file.read()
    print(content)
except FileNotFoundError:
    print("Error: File not found.")
else:
    print("File read successfully.")
finally:
    file.close()  # Ensures file is closed
```

---

## Handling Exceptions in Data Processing

### Ignoring Errors and Continuing Processing

```py
data = ["10", "20", "hello", "30"]

processed_data = []
for item in data:
    try:
        processed_data.append(int(item))  # Convert to integer
    except ValueError:
        print(f"Skipping invalid data: {item}")

print(processed_data)  # Output: [10, 20, 30]
```

### Raising Custom Exceptions

```py
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative.")
    return f"Valid age: {age}"

try:
    print(validate_age(-5))
except ValueError as e:
    print(f"Error: {e}")
```

---

# Logging

Logging helps track errors and debug issues without printing messages directly in the program.

## Basic Logging

```py
import logging

logging.basicConfig(level=logging.DEBUG)

logging.debug("This is a debug message.")
logging.info("This is an info message.")
logging.warning("This is a warning message.")
logging.error("This is an error message.")
logging.critical("This is a critical message.")
```

## Logging to a File

```py
logging.basicConfig(filename="app.log", level=logging.ERROR)

try:
    result = 10 / 0
except ZeroDivisionError:
    logging.error("Attempted to divide by zero.", exc_info=True)
```

## Using Logging in Data Processing

```py
def process_data(data):
    try:
        return int(data)
    except ValueError:
        logging.warning(f"Invalid data encountered: {data}")
        return None

data = ["10", "20", "hello", "30"]
processed_data = [process_data(item) for item in data]
print(processed_data)  # Output: [10, 20, None, 30]
```

---

## Summary

| Concept | Example |
| ----- | ----- |
| **Try-Except** | `try: x = 10/0 except ZeroDivisionError:` |
| **Multiple Exceptions** | `except (ZeroDivisionError, ValueError):` |
| **`else` and `finally`** | `finally: file.close()` |
| **Custom Exception** | `raise ValueError("Invalid age")` |
| **Basic Logging** | `logging.debug("Message")` |
| **Logging to a File** | `logging.basicConfig(filename="app.log")` |

