---
tags:
  - FORMAT
  - Python
---
In Python, user input is collected using the `input()` function. This function waits for the user to type something and press Enter, then returns the input as a string.

# Basic Example:

```python
name = input("Enter your name: ")
print("Hello, " + name + "!")
```

- The string inside `input()` is a prompt message displayed to the user.
- The user types their input and presses Enter.
- The input is stored as a string in the variable `name`.

# Handling Numeric Input:

Since `input()` always returns a string, you need to convert it if you're expecting a number:

```python
e = int(input("Enter your age: "))  # Converts input to an integer
ight = float(input("Enter your height in meters: "))  # Converts input to a float
print("You are", age, "years old and", height, "meters tall.")
```

# Handling Multiple Inputs:

You can use `split()` to take multiple inputs in one line:

```python
x, y = input("Enter two numbers separated by space: ").split()
 y = int(x), int(y)  # Convert strings to integers
print("Sum:", x + y)
```

# Advanced Input Handling:

To avoid errors, always validate user input:

```python
while True:
    try:
        num = int(input("Enter a number: "))
      break  # Exit loop if input is valid
    except ValueError:
        print("Invalid input! Please enter a valid number.")
```