---
tags:
  - FORMAT
  - HTML
---
The `<video>` tag in HTML is used to **embed video content** directly on a webpage. It allows you to play multimedia video files, such as `.mp4`, `.webm`, or `.ogg` formats, without needing third-party plugins or players.

# Basic Structure of the `<video>` Tag

The `<video>` tag is used to embed a video file in the webpage. You can specify multiple video formats to ensure compatibility with different browsers. The `controls` attribute is commonly added to provide playback controls (play, pause, volume, etc.) to users.

Here’s the basic syntax:

```
<video src="video.mp4" controls>
  Your browser does not support the video tag.
</video>
```

In this example:

* `src="video.mp4"` specifies the video file you want to embed.

* The `controls` attribute adds the video player controls like play, pause, volume, and fullscreen.

* The text inside the `<video>` tag ("Your browser does not support the video tag") is a fallback message that appears if the browser doesn't support the `<video>` tag.

# Attributes of the `<video>` Tag

1. **`src`**:

   * Specifies the video file's path (relative or absolute).

   * Example: `<video src="movie.mp4">`.

2. **`controls`**:

   * Adds basic video controls (play, pause, volume, etc.) to the player.

   * Example: `<video src="movie.mp4" controls>`.

3. **`autoplay`**:

   * The video starts automatically as soon as it's loaded.

   * Example: `<video src="movie.mp4" autoplay controls>`.

4. **`loop`**:

   * Causes the video to automatically restart when it reaches the end.

   * Example: `<video src="movie.mp4" loop controls>`.

5. **`muted`**:

   * Starts the video with no sound.

   * Example: `<video src="movie.mp4" muted controls>`.

6. **`poster`**:

   * Specifies an image to be shown before the video starts playing. This image is displayed as a thumbnail or a static preview.

   * Example: `<video src="movie.mp4" poster="thumbnail.jpg" controls>`.

7. **`width` and `height`**:

   * Specifies the dimensions of the video player (in pixels).

   * Example: `<video src="movie.mp4" width="640" height="360" controls>`.

# Using Multiple Sources (for Browser Compatibility)

Different browsers support different video formats, so it’s a good idea to provide multiple video sources with the `<source>` element. The browser will try to play the first supported format.

Example:

```
<video controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.webm" type="video/webm">
  Your browser does not support the video tag.
</video>
```

In this example:

* The browser will first try to play the `.mp4` file.

* If it doesn’t support `.mp4`, it will attempt to play the `.ogg` file, and then the `.webm` file if the others are unsupported.

* The text "Your browser does not support the video tag" is displayed if none of the provided formats are supported.

# Fallback Content

If a browser doesn't support the `<video>` tag, you can include fallback content, such as a link to download the video or alternative instructions. This is placed between the `<video>` and `</video>` tags.

Example:

```
<video controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  Your browser does not support the video tag. You can download the video 
  <a href="movie.mp4">here</a>.
</video>
```

# Example of Full Video Embed

Here’s a practical example with a video embedded using the `<video>` tag with various attributes:

```
<video controls autoplay loop muted width="640" height="360" poster="preview.jpg">
  <source src="video.mp4" type="video/mp4">
  <source src="video.ogg" type="video/ogg">
  <source src="video.webm" type="video/webm">
  Your browser does not support the video tag. You can download the video 
  <a href="video.mp4">here</a>.
</video>
```

In this example:

* The video has playback controls (`controls`).

* The video starts automatically (`autoplay`), loops (`loop`), and is muted by default (`muted`).

* It has a specified width and height (640px by 360px).

* If the video cannot be played, users are offered a download link.

# Supported Video Formats

Different browsers support different formats. Here are the most commonly used video formats:

* **MP4** (preferred for most browsers): `video/mp4`

* **WebM**: `video/webm`

* **OGG**: `video/ogg`

By including multiple video formats, you ensure that your video can be played in a wider variety of browsers.

# Summary

* The `<video>` tag is used to embed videos in a webpage.

* It supports multiple attributes like `controls`, `autoplay`, `loop`, `muted`, and `poster` to customize the video player’s behavior.

* You can include multiple `<source>` elements for different video formats to ensure compatibility across browsers.

* Always provide fallback content for browsers that don’t support the `<video>` tag.

Embedding videos using the `<video>` tag is a powerful way to integrate multimedia directly into your webpages, providing a better user experience with full control over the video playback.

