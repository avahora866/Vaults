---
tags:
  - FORMAT
  - Python
---
# Saving Plots with `plt.savefig()`

## Basic Syntax

```py
plt.savefig('filename', dpi=300, format='png', bbox_inches='tight')
```

*   
  `'filename'` → The name of the file you want to save, including the extension (e.g., `'plot.png'`).

* `dpi` → Dots per inch (resolution). A higher DPI gives a higher-quality image (e.g., `dpi=300` for print quality).

* `format` → The format in which to save the file (e.g., `'png'`, `'jpg'`, `'pdf'`, `'svg'`). If not specified, it automatically detects the format based on the file extension.

* `bbox_inches` → Controls the bounding box (area to be saved). `'tight'` ensures that the figure fits tightly within the saved image, removing unnecessary margins.

## Example 1: Saving a Plot as PNG

```py
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('Sine Wave')

 Save the plot as a PNG file with a DPI of 300
plt.savefig('sine_wave.png', dpi=300)
plt.show()
```

## Example 2: Saving a Plot as a PDF

```py
plt.plot(x, y, color='red')
plt.title('Sine Wave')

 Save the plot as a PDF
plt.savefig('sine_wave.pdf')
plt.show()
```

*   
  PDF is often used for vector graphics, ensuring the plot is scalable without loss of quality.

## Example 3: Saving a Plot as SVG

```py
plt.plot(x, y, color='green')
plt.title('Sine Wave')

 Save the plot as an SVG file (scalable vector graphic)
plt.savefig('sine_wave.svg')
plt.show()
```

*   
  SVG files are often used for high-quality vector graphics, commonly used for web images and illustrations.

## Example 4: Saving a Plot as a JPEG

```py
plt.plot(x, y, color='purple')
plt.title('Sine Wave')

 Save the plot as a JPEG file with lower quality for web use
plt.savefig('sine_wave.jpg', dpi=100, quality=85)
plt.show()
```

*   
  The `quality` parameter in `JPEG` files controls the compression quality (1 to 100).

## Example 5: Saving a Plot with Tight Layout

```py
plt.plot(x, y, label='sin(x)')
plt.title('Sine Wave with Tight Layout')

 Use 'bbox_inches="tight"' to ensure tight bounding box around the plot
plt.savefig('tight_layout_plot.png', dpi=300, bbox_inches='tight')
plt.show()
```

*   
  This ensures the plot’s labels, title, and axes are included without any extra whitespace.

---

# Additional `savefig` Parameters

* **`transparent`**: If set to `True`, the background of the plot is transparent (useful for overlaying images).

```py
plt.savefig('plot_with_transparent_background.png', transparent=True)
```

*   
  **`pad_inches`**: Controls padding around the plot when saving with `bbox_inches='tight'`.

```py
plt.savefig('tight_with_padding.png', dpi=300, bbox_inches='tight', pad_inches=0.1)
```

*   
  **`facecolor` and `edgecolor`**: Set the background color of the figure and the edge color of the saved image.

```py
plt.savefig('custom_background.png', dpi=300, facecolor='white', edgecolor='black')
```

*   
  **`backend`**: Allows you to specify the backend used for rendering (rarely needed in basic use cases).

---

# Common Formats Supported by `savefig`

* **PNG**: Bitmap image format, good for web use (lossy compression).

* **JPEG**: Compressed image format (lossy), good for web and smaller files.

* **PDF**: Scalable vector graphics, high-quality for printing.

* **SVG**: Scalable vector graphics, good for web and illustrations.

* **EPS**: Encapsulated PostScript, used for printing and graphics.

* **TIFF**: Tagged Image File Format, typically used for high-quality raster graphics.

---

# Tips for Saving Plots

1. **Quality**: Always use a high DPI (e.g., `300`) when saving plots for print or publication.

2. **Format**: Use **vector formats** (like PDF or SVG) for illustrations that need to scale, and **raster formats** (like PNG or JPEG) for web or screen use.

3. **Layout**: Use `bbox_inches='tight'` to minimize excess white space and fit the plot tightly.

By using `plt.savefig()`, you can save high-quality plots in a variety of formats to meet your needs. Let me know if you need further help\!

