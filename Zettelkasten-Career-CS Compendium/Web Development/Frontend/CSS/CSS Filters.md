---
tags:
  - FORMAT
  - CSS
---
CSS filters are a powerful tool that allows you to apply visual effects to elements, including images, text, and backgrounds, directly in your stylesheet. Filters let you manipulate the appearance of an element by applying various graphic effects such as blur, brightness, contrast, grayscale, and many others. These effects are processed on the element's rendering and do not alter the original content itself.

# CSS `filter` Property

The **`filter`** property applies graphical effects such as blur, brightness, contrast, etc., to an element. You can apply multiple filters at once by separating them with spaces.

* **Syntax**:

```
filter: filter-function(value);
```

Where `filter-function` can be one of the following functions, and `value` is the value associated with that function.

# Common CSS Filters

Let’s explore some of the most commonly used filters:

---

# `blur()`

The **`blur()`** filter applies a Gaussian blur to an element, making it appear out of focus. The higher the value, the more blurred the element will appear.

* **Syntax**:

```
filter: blur(5px); /* Apply 5px blur */
```

*   
  **Example**:

```
img {
  filter: blur(5px);
}
```

This will make the image blurry with a **5px** radius of blur.

---

# `brightness()`

The **`brightness()`** filter adjusts the brightness of an element. Values greater than `1` make the element brighter, and values less than `1` make it darker. A value of `1` keeps the brightness unchanged.

* **Syntax**:

```
filter: brightness(1.5); /* Increase brightness by 50% */
```

*   
  **Example**:

```
img {
  filter: brightness(1.2);
}
```

In this case, the image will appear **20% brighter**.

---

# `contrast()`

The **`contrast()`** filter changes the contrast of an element. Similar to brightness, values greater than `1` increase contrast, while values between `0` and `1` decrease contrast. A value of `1` means no change.

* **Syntax**:

```
filter: contrast(2); /* Double the contrast */
```

*   
  **Example**:

```
img {
  filter: contrast(0.5);
}
```

This will make the image have **half the contrast**.

---

# `grayscale()`

The **`grayscale()`** filter converts the element to grayscale (black and white). The value is a percentage, where `0%` means no effect and `100%` means a fully grayscale (black and white) image. Intermediate values apply partial grayscale.

* **Syntax**:

```
filter: grayscale(100%); /* Fully grayscale */
```

*   
  **Example**:

```
img {
  filter: grayscale(50%);
}
```

This will make the image **50% grayscale**, creating a desaturated effect.

---

# `sepia()`

The **`sepia()`** filter gives an image a warm, brownish tone, creating a vintage or old photo effect. The value is a percentage, where `0%` means no effect, and `100%` applies a full sepia tone.

* **Syntax**:

```
filter: sepia(100%); /* Apply full sepia tone */
```

*   
  **Example**:

```
img {
  filter: sepia(70%);
}
```

This will apply a **70% sepia** effect to the image.

---

# `saturate()`

The **`saturate()`** filter changes the saturation of an element. A value greater than `1` increases saturation (more vivid colors), while a value between `0` and `1` decreases saturation (less vibrant colors).

* **Syntax**:

```
filter: saturate(2); /* Double the saturation */
```

*   
  **Example**:

```
img {
  filter: saturate(0.5);
}
```

This will make the image **50% less saturated**, making the colors less vivid.

---

# `invert()`

The **`invert()`** filter inverts the colors of an element, effectively creating a negative of the element's colors. The value is a percentage where `0%` means no inversion and `100%` is a full color inversion.

* **Syntax**:

```
filter: invert(100%); /* Fully invert colors */
```

*   
  **Example**:

```
img {
  filter: invert(100%);
}
```

This will **invert the colors** of the image, making it look like a negative.

---

# `hue-rotate()`

The **`hue-rotate()`** filter rotates the hue of an element’s colors by a specified angle (in degrees). The angle is typically given in degrees from `0deg` to `360deg`, where `0deg` means no rotation.

* **Syntax**:

```
filter: hue-rotate(90deg); /* Rotate hue by 90 degrees */
```

*   
  **Example**:

```
img {
  filter: hue-rotate(180deg);
}
```

This will rotate the **hue by 180 degrees**, changing the colors in a significant way.

---

# `drop-shadow()`

The **`drop-shadow()`** filter applies a shadow to the element, similar to `box-shadow`, but it's a filter. You can specify the horizontal and vertical offsets, blur radius, and color of the shadow.

* **Syntax**:

```
filter: drop-shadow(10px 10px 15px rgba(0, 0, 0, 0.5)); /* Apply shadow */
```

*   
  **Example**:

```
img {
  filter: drop-shadow(5px 5px 10px rgba(0, 0, 0, 0.5));
}
```

This will apply a **shadow** to the image, offset by **5px horizontally** and **5px vertically**, with a **10px blur** and a **50% black color**.

---

# Combining Multiple Filters

You can combine multiple filters by chaining them together, separated by spaces. For example:

```
filter: grayscale(50%) blur(5px) brightness(1.2);
```

This would apply a **50% grayscale**, **5px blur**, and **20% brightness increase** to the element.

---

# Example: Applying Multiple Filters to an Image

Let’s apply some of these filters to an image for a creative effect:

```
img {
  filter: grayscale(100%) sepia(50%) contrast(1.5) brightness(0.8);
}
```

In this case:

* **Grayscale**: The image is completely converted to grayscale.

* **Sepia**: A sepia effect is added at **50%** intensity.

* **Contrast**: The contrast is **increased by 1.5 times**.

* **Brightness**: The brightness is **reduced to 80%** of its original value.

---

# Conclusion

CSS filters offer a simple and effective way to apply visual effects to elements, making it easier to enhance or manipulate the appearance of images and other content without using JavaScript or external image-editing tools. Filters like **`blur`**, **`brightness`**, **`contrast`**, **`grayscale`**, **`invert`**, and others allow for creative styling of web elements, and you can combine them for even more sophisticated effects.

