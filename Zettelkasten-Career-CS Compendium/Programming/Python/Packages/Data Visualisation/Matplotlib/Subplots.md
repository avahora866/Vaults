---
tags:
  - FORMAT
  - Python
---
# Understanding `plt.subplots()`

* **Definition**:  
   `plt.subplots()` creates a figure and multiple subplots (axes) arranged in a grid.

* **Returns**:

  * `fig` → The entire figure (canvas).

  * `axs` → An array of axes objects (subplots) for plotting.

## Syntax:

```py
fig, axs = plt.subplots(nrows, ncols, figsize=(width, height), sharex, sharey)
```

*   
  `nrows`, `ncols` → Number of rows and columns of subplots.

* `figsize` → Size of the figure in inches `(width, height)`.

* `sharex`, `sharey` → Share the x-axis or y-axis across subplots.

---

# Examples of Using `plt.subplots()`

## Simple Grid of Subplots

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

fig, axs = plt.subplots(2, 1, figsize=(8, 6))

axs[0].plot(x, y1, color='blue', label='sin(x)')
axs[0].set_title('Sine Function')
axs[0].legend()

axs[1].plot(x, y2, color='red', label='cos(x)')
axs[1].set_title('Cosine Function')
axs[1].legend()

lt.tight_layout()  # Prevent overlapping
plt.show()
```

*   
  **Explanation**:

  * `axs[0]` and `axs[1]` represent the two subplots.

  * `plt.tight_layout()` adjusts spacing to avoid overlap.

---

## Grid with Shared Axes

You can share axes using `sharex=True` or `sharey=True` for a cleaner comparison.

```py
fig, axs = plt.subplots(2, 2, figsize=(10, 8), sharex=True, sharey=True)

for i in range(2):
    for j in range(2):
        axs[i, j].plot(x, np.sin(x + (i + j)), label=f'sin(x + {i+j})')
        axs[i, j].legend()
        axs[i, j].grid(True)

lt.suptitle('Multiple Plots with Shared Axes')  # Super title for the figure
plt.show()
```

*   
  **Explanation**:

  * `axs[i, j]` accesses individual plots in a 2x2 grid.

  * Shared axes allow consistent scaling.

---

## Single Plot with `plt.subplots()`

If `plt.subplots()` is used with `(1, 1)`, it behaves like a regular plot.

```py
fig, ax = plt.subplots(figsize=(6, 4))
ax.plot(x, y1, color='blue', linestyle='--', label='sin(x)')
ax.set_title('Single Plot Example')
ax.legend()
plt.show()
```

*   
  **Explanation**:

  * `ax` represents the single set of axes.

---

# Additional Tips for `plt.subplots()`

* **Figure-Wide Title**:

```py
fig.suptitle('My Figure Title')
```

*   
  **Adjust Spacing**:

```py
plt.tight_layout()
```

*   
  **Remove Empty Plots**:  
   If your grid has more plots than data, you can hide empty ones using:

```py
axs[1, 1].axis('off')
```

This makes `plt.subplots()` a versatile tool for visualizing multiple datasets effectively. Let me know if you'd like further adjustments or explanations\!

