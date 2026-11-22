---
tags:
  - FORMAT
  - Python
---
# Advanced Plot Types

## Scatter Plots

A **scatter plot** is used to display individual data points on a 2D plane, showing the relationship between two continuous variables.

### Syntax:

```py
plt.scatter(x, y, c='color', marker='shape', s=size)
```

*   
  `x`, `y`: Data values for the x and y axes.

* `c`: Color of the points.

* `marker`: Shape of the points (e.g., `'o'` for circle, `'s'` for square).

* `s`: Size of the points (optional).

### Example:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.random.rand(50)
y = np.random.rand(50)
olors = np.random.rand(50)  # Color by values
izes = 100  np.random.rand(50)  # Sizes of points

plt.scatter(x, y, c=colors, s=sizes, alpha=0.5, cmap='viridis')
plt.title('Scatter Plot Example')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
lt.colorbar()  # Add color bar to show the color scale
plt.show()
```

*   
  **Explanation**: The scatter plot shows the relationship between the `x` and `y` values, with color and size variations based on additional variables.

---

## Box Plots

A **box plot** (also called a **box-and-whisker plot**) is used to display the distribution of a dataset through five summary statistics: the minimum, first quartile (Q1), median (Q2), third quartile (Q3), and maximum.

### Syntax:

```py
plt.boxplot(data, notch=False, vert=True, patch_artist=False)
```

*   
  `data`: The data to plot (could be a list or array).

* `notch`: If `True`, adds a notch to the box (indicating confidence intervals for the median).

* `vert`: If `True`, the boxes are vertical. If `False`, the boxes are horizontal.

* `patch_artist`: If `True`, the boxes will be filled with color.

### Example:

```py
ata = [np.random.normal(0, 1, 100) for _ in range(3)]  # Three datasets

plt.boxplot(data, patch_artist=True, notch=True, vert=True, 
            boxprops=dict(facecolor='skyblue', color='black'))
plt.title('Box Plot Example')
plt.xticks([1, 2, 3], ['Set 1', 'Set 2', 'Set 3'])
plt.ylabel('Values')
plt.show()
```

*   
  **Explanation**: The box plot shows the spread and skewness of each dataset, with the central line representing the median, and the box showing the interquartile range (IQR).

---

## Heatmaps

A **heatmap** is a way of representing data in a 2D grid format, where values are shown in different colors. This is often used for visualizing correlations, matrices, or intensity data.

### Syntax:

```py
plt.imshow(data, cmap='viridis', interpolation='nearest')
lt.colorbar()  # Optional: Adds a color scale bar
```

*   
  `data`: The 2D array of values to plot.

* `cmap`: Color map (e.g., `'viridis'`, `'hot'`, `'cool'`).

* `interpolation`: How to interpolate between points (e.g., `'nearest'`, `'bilinear'`).

### Example:

```py
import seaborn as sns

 Generate a random 10x10 matrix
data = np.random.rand(10, 10)

plt.imshow(data, cmap='coolwarm', interpolation='nearest')
plt.title('Heatmap Example')
plt.colorbar()
plt.show()
```

*   
  **Explanation**: The heatmap visualizes the intensity of values in a matrix. The color bar indicates the value range, and each cell's color represents a data point.

---

## Contour Plots

A **contour plot** is used to represent 3D data in 2D by drawing contour lines or filled areas. Contour plots are useful for visualizing the relationship between three continuous variables, where two variables define the grid, and the third defines the contour values.

### Syntax:

```py
plt.contour(X, Y, Z, levels, cmap='viridis')
```

*   
  `X`, `Y`: 2D arrays representing the grid for the x and y axes.

* `Z`: A 2D array with values representing the contour levels.

* `levels`: Specifies the contour levels (optional).

* `cmap`: Color map for the contours.

### Example:

```py
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
X, Y = np.meshgrid(x, y)
Z = np.sin(np.sqrt(X**2 + Y**2))

plt.contour(X, Y, Z, 20, cmap='plasma')
plt.title('Contour Plot Example')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
lt.colorbar()  # Show color bar
plt.show()
```

*   
  **Explanation**: The contour plot shows lines of constant value, with the color gradient indicating how the function changes over the grid.

---

# Summary of Advanced Plot Types

## Scatter Plot

* Used for visualizing relationships between two continuous variables.

* Can add color and size to points based on additional variables.

## Box Plot

* Used for visualizing the distribution of data through quartiles.

* Displays key statistics like median, quartiles, and outliers.

## Heatmap

* Useful for visualizing 2D matrix data, showing intensity via color.

* Common in correlation matrices or intensity-based data.

## Contour Plot

* Used for visualizing three continuous variables by plotting contour lines or filled regions.

* Ideal for showing the "shape" of a 3D surface in a 2D plot.

These advanced plots help in representing complex relationships, distributions, and patterns in your data. Let me know if you'd like more details or examples\!

