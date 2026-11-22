---
tags:
  - FORMAT
  - Python
---
Matplotlib allows you to customize the appearance of your plots through **styles** and **themes**. This is useful when you want to change the default appearance of your plots to make them more aesthetically pleasing or aligned with certain visual guidelines. One way to do this is by using **Matplotlib's predefined styles**, or you can use external libraries like **Seaborn** to apply more sophisticated themes.

# Using `plt.style.use()`

The function `plt.style.use()` allows you to apply a style or theme to your Matplotlib plots. This affects various aspects of the plot, such as the color scheme, grid lines, background, fonts, and more. Matplotlib comes with several built-in styles, and you can also use external libraries like Seaborn to apply more advanced themes.

# Applying Matplotlib's Built-In Styles

Matplotlib comes with a set of predefined styles. You can easily apply these styles using the `plt.style.use()` function by passing the name of the style as a string.

## Example:

```py
import matplotlib.pyplot as plt
import numpy as np

 Use a built-in style
plt.style.use('ggplot')

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('Plot with ggplot Style')
plt.show()
```

*   
  **Explanation**:

  * The `plt.style.use('ggplot')` line applies the `ggplot` style to the plot, giving it a look similar to the **ggplot2** library in R (including grid lines, lighter background, etc.).

# Using Seaborn Styles

Seaborn, a statistical data visualization library built on top of Matplotlib, also provides themes and styles that can enhance the appearance of your plots. You can apply Seaborn's style using `plt.style.use('seaborn')`, which will automatically apply the default Seaborn theme.

## Example with Seaborn:

```py
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

 Apply Seaborn style
plt.style.use('seaborn-darkgrid')

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('Plot with Seaborn-darkgrid Style')
plt.show()
```

*   
  **Explanation**:

  * The `plt.style.use('seaborn-darkgrid')` line applies the Seaborn style `seaborn-darkgrid`, which gives the plot a dark background with grid lines (often used for clearer data presentation in statistical plots).

# Available Seaborn Styles

Seaborn provides several built-in styles that can be applied to plots. Some common Seaborn styles include:

* **seaborn-white**: A clean white background with no grid lines.

* **seaborn-darkgrid**: A darker background with grid lines.

* **seaborn-whitegrid**: A white background with grid lines.

* **seaborn-ticks**: White background with ticks (no grid lines).

* **seaborn-pastel**: Light pastel colors for elements.

* **seaborn-muted**: Muted color palette.

## Example:

```py
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

 Apply seaborn-pastel style
plt.style.use('seaborn-pastel')

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('Plot with Seaborn-pastel Style')
plt.show()
```

*   
  **Explanation**:

  * The `seaborn-pastel` style gives the plot a soft, pastel color palette for the lines and markers.

# Customizing Themes and Styles Further

While you can apply Seaborn or Matplotlib styles with `plt.style.use()`, you can also create your own custom styles or modify existing ones. Matplotlib allows for highly customizable plots where you can adjust properties like colors, grid lines, fonts, and axes appearance.

## Example: Customizing Theme with Seaborn

```py
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

 Apply a Seaborn style
sns.set(style="whitegrid", palette="pastel", font="Arial", font_scale=1.5)

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('Custom Seaborn Theme with Whitegrid', fontsize=18)
plt.show()
```

*   
  **Explanation**:

  * `sns.set(style="whitegrid", palette="pastel", font="Arial", font_scale=1.5)` applies a customized Seaborn style where:

    * `style="whitegrid"` sets the background with grid lines.

    * `palette="pastel"` applies pastel colors.

    * `font="Arial"` sets the font to Arial.

    * `font_scale=1.5` increases the font size of labels and titles.

---

 List of Commonly Used Styles

Here is a list of some of the commonly used Matplotlib and Seaborn styles:

# Matplotlib Built-in Styles:

* **'seaborn-darkgrid'**: Dark background with gridlines (commonly used).

* **'seaborn-whitegrid'**: White background with gridlines.

* **'seaborn-white'**: Clean, white background with no gridlines.

* **'seaborn-dark'**: Dark background without gridlines.

* **'ggplot'**: R ggplot2-inspired style.

* **'bmh'**: A style with a minimal, clean background.

* **'fivethirtyeight'**: Style inspired by FiveThirtyEight.com.

* **'classic'**: The default Matplotlib style (looks like older versions of Matplotlib).

# Seaborn Themes:

* **'seaborn-darkgrid'**: Dark background with gridlines.

* **'seaborn-whitegrid'**: White background with gridlines.

* **'seaborn-ticks'**: Clean background with ticks (no gridlines).

* **'seaborn-pastel'**: Soft pastel color palette for the plot.

* **'seaborn-muted'**: Muted color palette for plots.

---

 Applying and Combining Styles

You can also combine multiple styles or use specific elements of a style. For example, you can first apply a Seaborn style and then modify individual plot elements:

# Example: Combining Seaborn and Custom Elements

```py
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

 Apply a Seaborn style
sns.set(style="darkgrid", palette="Set2", font="Comic Sans MS")

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)

 Customize individual elements
plt.title('Combining Seaborn and Custom Elements', fontsize=18)
plt.xlabel('X-axis', fontsize=14)
plt.ylabel('Y-axis', fontsize=14)

plt.show()
```

*   
  **Explanation**:

  * The `sns.set(style="darkgrid", palette="Set2", font="Comic Sans MS")` line applies a custom Seaborn style, while we further customize the title and axis labels.

---

 Summary of Key Points

* **`plt.style.use()`**: Allows you to apply predefined styles to Matplotlib plots, making them visually more appealing.

* **Seaborn Styles**: Seaborn offers several built-in themes (like `seaborn-whitegrid`, `seaborn-darkgrid`, etc.) to enhance the aesthetic of your plots.

* **Customization**: You can further customize these styles by modifying individual plot elements like the title, axes, colors, and fonts.

By applying styles and themes, you can make your plots more professional, readable, and visually aligned with your preferences. Let me know if you need any more examples or clarification\!

