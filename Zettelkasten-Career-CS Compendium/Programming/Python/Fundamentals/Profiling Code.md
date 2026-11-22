---
tags:
  - Python
  - FORMAT
  - Concurrency
---
# Profiling Code in Python: `timeit` and `cProfile`

Profiling helps analyze code performance by measuring execution time and identifying bottlenecks. Python provides two main tools for profiling:

1. **`timeit`** \- Measures execution time for small code snippets.  
2. **`cProfile`** \- Provides detailed profiling for functions and scripts.

---

# Using `timeit` for Performance Timing

The **`timeit`** module is useful for benchmarking short code snippets. It runs the code multiple times to get an accurate average execution time.

### Example 1: Measuring Execution Time of a Simple Loop

```py
import timeit

code_snippet = """
sum([i for i in range(1000)])
"""

execution_time = timeit.timeit(code_snippet, number=10000)
print(f"Execution time: {execution_time:.5f} seconds")
```

### Explanation:

* `timeit.timeit()` runs the given code snippet **10,000 times** and returns the total execution time.  
* The **lower the time, the better the performance**.

### Example 2: Comparing List Comprehension vs Generator Expression

```py
setup_code = "nums = range(1000)"

list_time = timeit.timeit("sum([i for i in nums])", setup=setup_code, number=10000)
gen_time = timeit.timeit("sum(i for i in nums)", setup=setup_code, number=10000)

print(f"List comprehension: {list_time:.5f} seconds")
print(f"Generator expression: {gen_time:.5f} seconds")
```

### Expected Output:

```
List comprehension: 0.27000 seconds
Generator expression: 0.18000 seconds
```

* **Generator expressions** (`sum(i for i in nums)`) are often faster than **list comprehensions** (`sum([i for i in nums])`) because they avoid creating a full list in memory.

---

# Using `cProfile` for Detailed Performance Analysis

The **`cProfile`** module provides an in-depth analysis of function calls, execution time, and performance bottlenecks.

### Example: Profiling a Function with `cProfile`

```py
import cProfile

def slow_function():
    total = 0
    for i in range(10000):
        for j in range(100):
            total += i * j
    return total

cProfile.run("slow_function()")
```

### Expected Output (Example):

```
         2 function calls in 0.050 seconds

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.050    0.050    0.050    0.050 script.py:4(slow_function)
        1    0.000    0.000    0.050    0.050 <string>:1(<module>)
```

### Explanation:

* `ncalls`: Number of times the function was called.  
* `tottime`: Time spent in the function itself (excluding sub-calls).  
* `cumtime`: Total time spent in the function (including sub-calls).  
* Helps identify **which function is the slowest**.

---

# Summary

| Tool | Use Case | Example |
| ----- | ----- | ----- |
| `timeit` | Measures execution time of small code snippets | `timeit.timeit("sum(range(1000))", number=10000)` |
| `cProfile` | Provides detailed function profiling | `cProfile.run("slow_function()")` |

