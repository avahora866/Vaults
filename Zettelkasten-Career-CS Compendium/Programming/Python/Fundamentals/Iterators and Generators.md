---
tags:
  - Python
---
# Iterators

An **iterator** is an object that represents a stream of data and supports two methods: `__iter__()` and `__next__()`. The `__iter__()` method initializes the iterator, and the `__next__()` method returns the next value in the sequence.

## Using `iter()` and `next()`

The `iter()` function returns an iterator object, and `next()` retrieves the next item from the iterator.

## Example: Using `iter()` and `next()`

```python
# Create a list
numbers = [1, 2, 3, 4, 5]

# Create an iterator using iter()
iterator = iter(numbers)

# Use next() to get values one by one
print(next(iterator))  # Output: 1
print(next(iterator))  # Output: 2
print(next(iterator))  # Output: 3
```

## Stopping Iteration (StopIteration Exception)

Once the iterator has no more items, it raises a `StopIteration` exception.

```python
# Continue getting next item until StopIteration
try:
    while True:
        print(next(iterator))
except StopIteration:
    print("Iteration complete")
```

---

# Generators

A **generator** is a special type of iterator that is defined using a function and the `yield` keyword. Generators allow you to produce values lazily (on-the-fly), which is useful when handling large datasets or streams of data.

## Creating Generators with `yield`

The `yield` keyword is used to return values from a function. Unlike `return`, `yield` allows the function to pause and resume, enabling the function to produce a sequence of results.

## Example: Simple Generator

```python
def count_up_to(limit):
    count = 1
    while count <= limit:
        yield count
        count += 1

# Create generator
counter = count_up_to(5)

# Get values one by one using next()
print(next(counter))  # Output: 1
print(next(counter))  # Output: 2
print(next(counter))  # Output: 3
```

## Example: Generator for Large Datasets (Handling Large Files)

Generators are particularly useful when working with large datasets because they allow you to process data without loading everything into memory at once.

```python
# Example: Generator to read a large file line by line
def read_large_file(file_name):
    with open(file_name, "r") as file:
        for line in file:
            yield line.strip()  # Yield each line

# Assuming "large_file.txt" exists
for line in read_large_file("large_file.txt"):
    print(line)
```

## Example: Fibonacci Sequence Generator

Generators can be used to generate an infinite sequence of numbers, such as the Fibonacci sequence.

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Generate Fibonacci numbers
fib_gen = fibonacci()

for _ in range(10):  # Print the first 10 Fibonacci numbers
    print(next(fib_gen))
```

## Advantages of Generators:

1. **Memory Efficiency**: Since generators yield values one by one, they do not store the entire sequence in memory.  
2. **Lazy Evaluation**: Values are computed only when requested, which is ideal for working with large datasets or streaming data.

---

# Summary

| Concept | Example |
| ----- | ----- |
| **Creating an Iterator** | `iterator = iter([1, 2, 3])` |
| **Using `next()`** | `print(next(iterator))` |
| **Handling StopIteration** | `try: while True: next(iterator) except StopIteration:` |
| **Creating a Generator** | `def count_up_to(limit): yield count` |
| **Using Generators** | `for line in read_large_file("file.txt"):` |
| **Infinite Sequence with Generators** | `def fibonacci(): yield a` |

