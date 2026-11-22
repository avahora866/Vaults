---
tags:
  - FORMAT
  - Python
---
# Customizing Plot Colors, Markers, and Line Styles

## Plot Colors

* **Definition**: You can customize the color of lines or bars using the `color` parameter.

* **Common Colors**:

  * Named colors like `'blue'`, `'red'`, `'green'`

  Hexadecimal codes like `'#FF5733'`

  * RGB values like `(0.1, 0.2, 0.5)`

* **Example**:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

plt.plot(x, y1, color='blue', label='sin(x)')
lt.plot(x, y2, color='#FF5733', label='cos(x)')

plt.legend()
plt.title('Custom Colors Example')
plt.show()
```

---

## Plot Markers

* **Definition**: Markers are symbols representing individual data points in line plots.

* **Common Markers**:

  * `'o'` → Circle

  * `'s'` → Square

  * `'^'` → Triangle

  * `'x'` → Cross

* **Example**:

```py
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]

plt.plot(x, y, marker='o', color='purple', markersize=8, label='Data')
plt.legend()
plt.title('Markers Example')
plt.show()
```

---

## Line Styles

* **Definition**: You can customize line styles using the `linestyle` parameter.

* **Common Line Styles**:

  * `'-'` → Solid line

  * `'--'` → Dashed line

  * `':'` → Dotted line

  * `'-.'` → Dash-dot line

* **Example**:

```py
plt.plot(x, y, color='green', linestyle='--', linewidth=2, marker='s', markersize=10)
plt.title('Line Styles Example')
plt.show()
```

---

# Setting Axis Limits with `.set_xlim()` and `.set_ylim()`

* **Definition**: `set_xlim()` and `set_ylim()` are used to define the range of values shown on the x-axis and y-axis.

* **Example**:

```py
fig, ax = plt.subplots()

x = np.linspace(0, 10, 100)
y = np.sin(x)

ax.plot(x, y, color='blue', linestyle='-')

ax.set_title('Setting Axis Limits')
ax.set_xlabel('X-axis')
ax.set_ylabel('Y-axis')

x.set_xlim(2, 8)   # Limit x-axis from 2 to 8
x.set_ylim(-0.75, 0.75)  # Limit y-axis from -0.75 to 0.75

plt.grid(True)
plt.show()
```

## Additional Notes:

* You can also use `plt.xlim()` and `plt.ylim()` if not using object-oriented methods.

* `ax.relim()` and `ax.autoscale()` can be used to reset and automatically adjust limits based on the data.

These techniques will help you create more informative and visually appealing plots. Let me know if you'd like further examples or explanations\!

