---
tags:
  - Python
  - FORMAT
---
# Using Generators Instead of Lists (Lazy Evaluation)

## What is Lazy Evaluation?

Lazy evaluation means **values are generated only when needed**, rather than all at once. This is useful when working with large datasets, as it **saves memory** and improves performance.

### Lists vs Generators

* **Lists** store all elements in memory.  
* **Generators** produce items **one at a time** using `yield`, saving memory.

---

## Creating Generators Instead of Lists

### Example 1: Using a List (Consumes More Memory)

```py
def generate_squares_list(n):
    return [i * i for i in range(n)]

squares = generate_squares_list(5)
print(squares)
```

### Output:

```
[0, 1, 4, 9, 16]
```

### Issue:

* **All values are stored in memory at once**.  
* If `n` is large (e.g., `n=1_000_000`), this consumes a lot of RAM.

---

### Example 2: Using a Generator (Efficient, Lazy Evaluation)

```py
def generate_squares_generator(n):
    for i in range(n):
        yield i  i  # Generates values one at a time

squares_gen = generate_squares_generator(5)
print(list(squares_gen))  # Convert to a list for display
```

### Output:

```
[0, 1, 4, 9, 16]
```

### Benefits of Using a Generator:

✅ **No large memory usage** – Numbers are computed **one at a time**  
 ✅ **Better for big data processing**

---

## Using `next()` to Get Values One by One

Instead of converting the generator to a list, use `next()`:

```py
squares_gen = generate_squares_generator(5)
print(next(squares_gen))  # Output: 0
print(next(squares_gen))  # Output: 1
print(next(squares_gen))  # Output: 4
```

If all elements are consumed, calling `next()` again raises `StopIteration`.

---

## Example: Infinite Generator (Lazy Evaluation)

Generators are great for **infinite sequences**, like Fibonacci numbers:

```py
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b  # Update Fibonacci numbers

fib_gen = fibonacci()

# Get the first 5 Fibonacci numbers
for _ in range(5):
    print(next(fib_gen))
```

### Output:

```
0
1
1
2
3
```

### Why Use a Generator Here?

* **A list would be infinite and crash** 💥  
* The generator **only produces numbers when requested** ✅

---

## Key Takeaways

| Feature | List | Generator |
| ----- | ----- | ----- |
| **Memory Usage** | High (stores all elements) | Low (lazy evaluation) |
| **Performance** | Slower for large data | Faster, processes items one at a time |
| **Best For** | Small datasets | Large or infinite sequences |

