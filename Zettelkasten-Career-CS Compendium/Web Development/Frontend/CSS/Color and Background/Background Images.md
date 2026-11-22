---
tags:
  - FORMAT
  - CSS
---
Adding background images to elements and controlling their positioning is a common task in CSS. This can be done using the `background-image` and `background-position` properties, along with other properties to fine-tune the display. Let's break down how to use these properties effectively.

# `background-image` Property

The `background-image` property allows you to set an image as the background for an HTML element.

## Syntax:

```
element {
    background-image: url('path/to/image.jpg');
}
```

## Example:

```
body {
    background-image: url('background.jpg');
}
```

This will apply the image `background.jpg` as the background of the entire webpage (the `body` element).

## Notes:

* The `url()` function is used to specify the path to the image.

* You can use either relative paths (e.g., `images/photo.jpg`) or absolute URLs (e.g., `https://www.example.com/photo.jpg`).

* The image will repeat by default across the background unless specified otherwise.

---

# `background-position` Property

The `background-position` property controls the position of the background image within the element. It allows you to adjust the image's starting point relative to the element's edges.

## Syntax:

```
element {
    background-position: x-position y-position;
}
```

*   
  `x-position` defines the horizontal starting point (e.g., `left`, `center`, `right`, or a specific pixel/percentage value).

* `y-position` defines the vertical starting point (e.g., `top`, `center`, `bottom`, or a specific pixel/percentage value).

## Example:

```
div {
    background-image: url('image.jpg');
    background-position: center center;
}
```

This will place the background image at the center of the `div` both horizontally and vertically.

## Values for `background-position`:

* **Keywords**:

  * `top`, `center`, `bottom` (for vertical position)

  * `left`, `center`, `right` (for horizontal position)

* **Length values**: You can also use pixel values or percentage values.

  * Example: `background-position: 50px 30px;` (50px from the left, 30px from the top).

  * Example: `background-position: 50% 50%;` (50% horizontally and vertically).

---

# Combining `background-image` and `background-position`

You can use both properties together to add a background image and position it exactly where you want it.

## Example:

```
div {
    background-image: url('image.jpg');
    background-position: top right;
    background-repeat: no-repeat;
}
```

In this example:

* The image `image.jpg` will be placed at the **top-right** corner of the `div`.

* The `background-repeat: no-repeat;` rule ensures that the background image will not repeat.

# Using Multiple Background Images

You can also use multiple background images in a single element. This can be useful for layering images, such as adding a texture over a photo.

## Syntax:

```
element {
    background-image: url('image1.jpg'), url('image2.png');
    background-position: left top, right bottom;
    background-repeat: no-repeat, repeat;
}
```

In this example:

* The first image (`image1.jpg`) is positioned at `left top`, and the second image (`image2.png`) is positioned at `right bottom`.

* The first image does not repeat, while the second image repeats.

## Notes:

* The images are layered in the order you list them (first listed image will be on top).

* You can set different positions, repeat values, and other properties for each background image.

---

# Other Helpful Background Properties

* **`background-repeat`**: Controls whether the background image repeats.

  * `repeat`: The image repeats both horizontally and vertically.

  * `no-repeat`: The image will not repeat.

  * `repeat-x`: The image repeats horizontally only.

  * `repeat-y`: The image repeats vertically only.

* Example:

```
div {
    background-image: url('image.jpg');
    background-repeat: no-repeat;
}
```

*   
  **`background-size`**: Allows you to control the size of the background image. This is useful for scaling images.

  * `cover`: Scales the image to cover the entire background, possibly cropping it.

  * `contain`: Scales the image to fit within the background area without cropping.

  * Specific dimensions (e.g., `100px 50px`): You can define custom width and height for the background image.

* Example:

```
div {
    background-image: url('image.jpg');
    background-size: cover;
}
```

*   
  **`background-attachment`**: Defines whether the background image scrolls with the page or stays fixed.

  * `scroll`: The image scrolls with the content.

  * `fixed`: The image stays fixed while the content scrolls.

* Example:

```
body {
    background-image: url('image.jpg');
    background-attachment: fixed;
}
```

---

# Complete Example with Background Image and Positioning:

```
body {
    background-image: url('background.jpg');
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    background-attachment: fixed;
}
```

In this example:

* The `background.jpg` image will be centered both horizontally and vertically.

* The image will not repeat, and it will cover the entire background area (it may be cropped).

* The background image will stay fixed while the page content scrolls.

---

# Summary of Key Properties:

* **`background-image`**: Adds an image as the background.

* **`background-position`**: Positions the background image within the element (using keywords, percentages, or pixel values).

* **`background-repeat`**: Determines whether the background image repeats.

* **`background-size`**: Adjusts the size of the background image (e.g., `cover`, `contain`, or specific dimensions).

* **`background-attachment`**: Controls whether the background image scrolls or stays fixed.

These properties are essential for creating visually appealing backgrounds and can be used to create various effects, such as parallax scrolling, full-screen backgrounds, and layered images.

