---
tags:
  - FORMAT
  - CSS
---
# Web-Safe Fonts

Web-safe fonts are fonts that are widely supported across different devices and browsers, ensuring that text is displayed consistently for most users. These fonts are installed on most operating systems by default, meaning they will render correctly without requiring additional files to be downloaded.

# Common Web-Safe Fonts

Here are some common web-safe fonts and their classifications:

1. **Serif Fonts**:

   * `Times New Roman` (Windows/Mac)

   * `Georgia` (Windows/Mac)

2. **Sans-Serif Fonts**:

   * `Arial` (Windows/Mac)

   * `Helvetica` (Mac)

   * `Verdana` (Windows/Mac)

3. **Monospace Fonts**:

   * `Courier New` (Windows/Mac)

   * `Lucida Console` (Windows)

4. **Cursive Fonts**:

   * `Comic Sans MS` (Windows)

   * `Brush Script MT` (Windows/Mac)

5. **Fantasy Fonts**:

   * `Impact` (Windows)

   * `Trebuchet MS` (Windows/Mac)

## Syntax Example:

```
body {
    font-family: "Arial", sans-serif;
}
```

In this example:

* If the `Arial` font is available, it will be used. If not, the `sans-serif` font (which is a generic fallback) will be used.

## Fallback Font Stack:

Always specify a fallback font stack to ensure your text is readable, even if the preferred font is unavailable. For example:

```
body {
    font-family: "Arial", "Helvetica", sans-serif;
}
```

If `Arial` isn't available, the browser will try `Helvetica`. If that isn't available either, it will fall back to the generic `sans-serif` font.

---

# Google Fonts

Google Fonts is a free service that allows web designers to include a large library of fonts on their websites. Unlike web-safe fonts, Google Fonts must be loaded from an external source (Google's servers), but they provide access to hundreds of beautifully designed fonts.

## How to Use Google Fonts:

1. **Choose a Google Font**:

   * Visit [Google Fonts](https://fonts.google.com/) to browse available fonts.

   * Select the font you want to use.

2. **Embed the Font**: After selecting a font, Google Fonts provides a link that you can add to the `<head>` of your HTML document to import the font. For example, if you want to use the "Roboto" font:

```
<link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
```

3.   
   This link imports the `Roboto` font to your webpage.

4. **Apply the Font with CSS**: After linking to the font, you can use it in your CSS by referencing its name in the `font-family` property:

```
body {
    font-family: 'Roboto', sans-serif;
}
```

## Example:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Google Font Example</title>
    <!-- Link to Google Font -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
        }
    </style>
</head>
<body>
    <h1>Hello, this is Roboto font!</h1>
    <p>This text is styled with the Google Font Roboto.</p>
</body>
</html>
```

In this example:

* The `Roboto` font is loaded from Google Fonts, and then applied to the `body` of the page.

* If for some reason the font cannot be loaded, the `sans-serif` fallback will be used.

---

# Benefits of Using Google Fonts

1. **Variety**: Google Fonts offers hundreds of fonts in various styles, making it easy to find a font that suits your design.

2. **Performance**:

   * Google Fonts hosts the fonts on their servers, which means faster load times and better performance.

   * The fonts are optimized for web use.

3. **Cross-Browser Compatibility**: Google Fonts are optimized for use across different browsers and devices, ensuring consistent rendering of fonts.

4. **Free**: All Google Fonts are free to use, which makes them an excellent choice for both personal and commercial projects.

5. **Customizable**:

   * You can customize the font to load specific weights, styles, and character sets (e.g., Latin, Cyrillic).

   * Only load the specific font weights and styles you need, which reduces the amount of data being loaded.

---

# Loading Multiple Google Fonts

If you want to use more than one font from Google Fonts, you can link multiple fonts in the `<head>` section of your HTML:

## Example:

```
<link href="https://fonts.googleapis.com/css2?family=Roboto&family=Open+Sans&display=swap" rel="stylesheet">
```

In your CSS, you can now apply both fonts:

```
body {
    font-family: 'Roboto', sans-serif;
}

h1 {
    font-family: 'Open Sans', sans-serif;
}
```

---

# Summary

* **Web-Safe Fonts**: These fonts are universally available on most devices and browsers, ensuring consistent display. They include fonts like `Arial`, `Times New Roman`, and `Courier New`.

* **Google Fonts**: Provides access to a large collection of fonts that are free to use, hosted by Google. You can embed the fonts by including a `<link>` in your HTML and applying them with CSS.

For most modern web design, **Google Fonts** is often preferred due to the variety and flexibility it offers, but **web-safe fonts** are still useful for projects where font loading and compatibility across all devices are crucial.

