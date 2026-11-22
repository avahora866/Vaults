---
tags:
  - FORMAT
  - Python
---
# Basic Plotting with Matplotlib

Matplotlib is a powerful library for creating visualizations in Python. The most commonly used module in Matplotlib is `pyplot`, typically imported as:

```py
import matplotlib.pyplot as plt
```

## Line Plot with `plt.plot()`

* **Purpose**: Used to plot continuous data as a line.

* **Example**:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y, label='sin(x)', color='blue', linestyle='-', linewidth=2)
plt.title('Line Plot Example')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
plt.legend()
plt.grid(True)
plt.show()
```

## Bar Plot with `plt.bar()`

* **Purpose**: Used to plot categorical data using rectangular bars.

* **Example**:

```py
categories = ['A', 'B', 'C', 'D']
values = [3, 7, 5, 9]

plt.bar(categories, values, color='purple')
plt.title('Bar Plot Example')
plt.xlabel('Categories')
plt.ylabel('Values')
plt.grid(axis='y')
plt.show()
```

## Histogram with `plt.hist()`

* **Purpose**: Used to visualize the distribution of a dataset.

* **Example**:

```py
data = np.random.randn(1000)

plt.hist(data, bins=20, color='green', edgecolor='black')
plt.title('Histogram Example')
plt.xlabel('Values')
plt.ylabel('Frequency')
plt.grid(axis='y')
plt.show()
```

# Customizing Plots

Here are some common ways to customize your plots:

* **Title**: `plt.title('Your Title')`

* **X-axis Label**: `plt.xlabel('X-axis Label')`

* **Y-axis Label**: `plt.ylabel('Y-axis Label')`

* **Legend**: `plt.legend()` — Add `label` in the plotting function to use this

* **Grid Lines**: `plt.grid(True)` — Customize further with parameters like `axis='x'` or `axis='y'`

* **Line Styles**: Use `linestyle`, `linewidth`, and `color` in `plt.plot()`

* **Bar Color and Edge Color**: `color='blue'`, `edgecolor='black'` in `plt.bar()` and `plt.hist()`

* **Transparency**: Use `alpha` parameter for transparency (`alpha=0.7`)

These basic tools will help you create clear and informative visualizations using Matplotlib\! Let me know if you'd like additional details or further examples.

