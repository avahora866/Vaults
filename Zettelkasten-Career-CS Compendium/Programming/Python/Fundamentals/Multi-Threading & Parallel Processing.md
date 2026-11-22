---
tags:
  - Python
  - FORMAT
---
# Multi-Threading & Parallel Processing in Python

Python provides two main modules for concurrency:

* **`threading`**: Used for I/O-bound tasks (like network requests, file I/O)  
* **`multiprocessing`**: Used for CPU-bound tasks (like heavy computations)

---

# Multi-Threading (For I/O-Bound Tasks)

The `threading` module in Python allows multiple threads to run concurrently. It is useful for I/O-bound operations such as:

* Downloading files  
* Reading/writing files  
* Making API requests

However, due to Python’s **Global Interpreter Lock (GIL)**, threads do not run in true parallel but instead take turns executing. This makes **multi-threading ineffective for CPU-bound tasks**.

## Example: Using `threading` for an I/O-bound Task

Let's create multiple threads to simulate downloading files.

```py
import threading
import time

def download_file(file_number):
    print(f"Starting download {file_number}...")
    time.sleep(2)  # Simulating a network delay
    print(f"Download {file_number} completed!")

# Creating multiple threads
threads = []
for i in range(1, 6):
    thread = threading.Thread(target=download_file, args=(i,))
    threads.append(thread)
    thread.start()

# Waiting for all threads to complete
for thread in threads:
    thread.join()

print("All downloads completed!")
```

### Explanation:

* We create 5 threads, each downloading a "file".  
* `time.sleep(2)` simulates a delay (like waiting for a network response).  
* `thread.start()` starts each thread.  
* `thread.join()` ensures all threads finish before proceeding.

### Output (Order may vary):

```
Starting download 1...
Starting download 2...
Starting download 3...
Starting download 4...
Starting download 5...
Download 1 completed!
Download 2 completed!
Download 3 completed!
Download 4 completed!
Download 5 completed!
All downloads completed!
```

Threads run concurrently, allowing other tasks to execute while waiting for I/O.

---

# Parallel Processing (For CPU-Bound Tasks)

For CPU-heavy tasks (like matrix calculations, image processing), we use the `multiprocessing` module. Unlike `threading`, `multiprocessing` **spawns multiple processes**, each with its own Python interpreter, bypassing the GIL and enabling true parallel execution.

## Example: Using `multiprocessing` for a CPU-bound Task

Let's calculate squares of numbers using multiple processes.

```py
import multiprocessing
import time

def square_number(n):
    print(f"Processing {n}...")
    time.sleep(1)  # Simulating heavy computation
    return n * n

if __name__ == "__main__":
    numbers = [1, 2, 3, 4, 5]

    # Create a pool of workers
    with multiprocessing.Pool(processes=3) as pool:
        results = pool.map(square_number, numbers)

    print("Results:", results)
```

### Explanation:

* We use `multiprocessing.Pool` to create a pool of worker processes.  
* `.map()` distributes work across multiple processes.  
* **Each process runs independently**, achieving true parallel execution.

### Output:

```
Processing 1...
Processing 2...
Processing 3...
Processing 4...
Processing 5...
Results: [1, 4, 9, 16, 25]
```

Since we use 3 worker processes, 3 numbers are processed simultaneously.

---

# When to Use `threading` vs `multiprocessing`

| Task Type | Best Module | Example Use Case |
| ----- | ----- | ----- |
| I/O-Bound Tasks | `threading` | Web scraping, file reading, database queries |
| CPU-Bound Tasks | `multiprocessing` | Image processing, machine learning, mathematical computations |

