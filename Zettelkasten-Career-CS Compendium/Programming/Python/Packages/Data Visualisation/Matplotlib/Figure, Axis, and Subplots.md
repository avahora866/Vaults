---
tags:
  - FORMAT
  - Python
---
# Figure, Axis, and Subplots in Matplotlib

## Figure

* **Definition**: The **Figure** is the overall container that holds one or more plots or visualizations. Think of it as a canvas.

* **Example**:

```py
import matplotlib.pyplot as plt

ig = plt.figure(figsize=(8, 6))  # Create a figure with a specific size
plt.show()
```

## Axis (Axes)

* **Definition**: **Axes** represent an individual plot or chart within the Figure. It includes the x-axis, y-axis, title, labels, and the plot itself.

* **Note**: The term "Axes" refers to a single set of x and y axes, not the plural of "axis."

* **Example**:

```py
ig, ax = plt.subplots()  # Creates one figure and one axis
x.plot([1, 2, 3, 4], [1, 4, 9, 16])  # Simple line plot
ax.set_title('Example Plot')
ax.set_xlabel('X-axis')
ax.set_ylabel('Y-axis')
plt.show()
```

## Subplots

* **Definition**: **Subplots** are multiple plots displayed within one Figure. You can arrange them in rows and columns using `plt.subplots()`.

* **Example**:

```py
ig, axs = plt.subplots(2, 2, figsize=(10, 8))  # 2x2 grid of subplots

axs[0, 0].plot([1, 2, 3, 4], [1, 4, 9, 16], color='blue')
axs[0, 0].set_title('Plot 1')

axs[0, 1].bar(['A', 'B', 'C', 'D'], [3, 7, 5, 9], color='green')
axs[0, 1].set_title('Plot 2')

axs[1, 0].hist([1, 2, 2, 3, 3, 3, 4, 4, 4, 4], bins=4, color='orange')
axs[1, 0].set_title('Plot 3')

axs[1, 1].plot([1, 2, 3, 4], [4, 3, 2, 1], color='red', linestyle='--')
axs[1, 1].set_title('Plot 4')

lt.tight_layout()  # Adjust subplot spacing
plt.show()
```

## Explanation:

* `fig` → The entire figure or canvas.

* `axs` → A 2D array of axes objects (`axs[row, col]`).

* `plt.tight_layout()` → Ensures proper spacing between subplots.

# Additional Customizations

* **Figure Title**: `fig.suptitle('My Figure Title')`

* **Share Axes**: `plt.subplots(2, 2, sharex=True, sharey=True)` to share x and y axes across subplots.

* **Figure Size**: Use `figsize=(width, height)` to adjust the size.

This understanding will help you create organized and visually appealing plots. Let me know if you'd like further examples or explanations\!

