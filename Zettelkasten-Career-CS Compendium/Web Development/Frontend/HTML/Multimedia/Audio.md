---
tags:
  - FORMAT
  - HTML
---
The `<audio>` tag in HTML is used to **embed audio content** directly into a webpage. It allows you to play sound files, such as `.mp3`, `.ogg`, or `.wav`, without relying on third-party plugins or players. The `<audio>` tag provides a simple way to add sound effects, music, podcasts, or any other audio to your site.

# Basic Structure of the `<audio>` Tag

The `<audio>` tag works similarly to the `<video>` tag but is used for audio content. It can have various attributes like `controls`, `autoplay`, `loop`, etc., to customize the behavior of the audio player.

Here’s the basic syntax:

```
<audio src="audio.mp3" controls>
  Your browser does not support the audio element.
</audio>
```

In this example:

* `src="audio.mp3"` specifies the audio file you want to embed.

* The `controls` attribute adds audio player controls like play, pause, volume, etc.

* The text inside the `<audio>` tag ("Your browser does not support the audio element") is a fallback message that appears if the browser doesn't support the `<audio>` tag.

# Attributes of the `<audio>` Tag

1. **`src`**:

   * Specifies the audio file to play.

   * Example: `<audio src="audio.mp3" controls>`.

2. **`controls`**:

   * Adds default audio controls like play, pause, volume, and seeking.

   * Example: `<audio src="audio.mp3" controls>`.

3. **`autoplay`**:

   * Makes the audio play automatically when the page loads.

   * Example: `<audio src="audio.mp3" autoplay controls>`.

4. **`loop`**:

   * Loops the audio when it reaches the end.

   * Example: `<audio src="audio.mp3" loop controls>`.

5. **`muted`**:

   * Starts the audio with the sound turned off.

   * Example: `<audio src="audio.mp3" muted controls>`.

6. **`preload`**:

   * Specifies if the audio should be loaded when the page loads. You can set it to `auto`, `metadata`, or `none`.

     * `auto`: Loads the audio completely when the page loads.

     * `metadata`: Loads only metadata (e.g., duration) when the page loads.

     * `none`: Doesn’t preload the audio.

   * Example: `<audio src="audio.mp3" preload="auto" controls>`.

7. **`volume`**:

   * While not directly set in the `<audio>` tag itself, the audio player will respect the user's system or browser volume settings. However, you can use JavaScript to control the volume level.

# Using Multiple Sources (for Browser Compatibility)

Since different browsers support different audio formats, it's best practice to provide multiple formats of the same audio to ensure compatibility.

Here’s an example with multiple `<source>` elements:

```
<audio controls>
  <source src="audio.mp3" type="audio/mp3">
  <source src="audio.ogg" type="audio/ogg">
  <source src="audio.wav" type="audio/wav">
  Your browser does not support the audio element.
</audio>
```

In this example:

* The browser will first try to play the `.mp3` file.

* If it doesn't support `.mp3`, it will try `.ogg`, and then `.wav` if neither of the first two formats are supported.

* If none of the formats are supported, the text "Your browser does not support the audio element" will be shown.

# Fallback Content

If a browser doesn't support the `<audio>` tag, you can provide fallback content, such as a link to download the audio file. This is placed between the opening `<audio>` and closing `</audio>` tags.

Example:

```
<audio controls>
  <source src="audio.mp3" type="audio/mp3">
  <source src="audio.ogg" type="audio/ogg">
  Your browser does not support the audio element. You can download the audio 
  <a href="audio.mp3">here</a>.
</audio>
```

# Example of Full Audio Embed

Here’s a practical example of embedding an audio file with various attributes:

```
<audio controls autoplay loop muted preload="auto">
  <source src="audio.mp3" type="audio/mp3">
  <source src="audio.ogg" type="audio/ogg">
  <source src="audio.wav" type="audio/wav">
  Your browser does not support the audio element. You can download the audio 
  <a href="audio.mp3">here</a>.
</audio>
```

In this example:

* The audio will start playing automatically (`autoplay`).

* It will loop after reaching the end (`loop`).

* The audio is muted at the beginning (`muted`).

* The `preload="auto"` attribute ensures the audio is loaded when the page loads.

* Multiple audio formats are provided to ensure compatibility across browsers.

# Supported Audio Formats

Different browsers support different formats, so it’s important to provide multiple audio formats for the best compatibility:

* **MP3** (`audio/mp3`) \- This is the most widely supported format.

* **OGG** (`audio/ogg`) \- Supported by many modern browsers.

* **WAV** (`audio/wav`) \- Supported by most modern browsers, but the file sizes are typically larger than MP3 or OGG.

By offering multiple formats, you ensure that your audio will play across all modern browsers.

# Summary

* The `<audio>` tag is used to embed audio content directly into a webpage.

* It supports attributes like `controls`, `autoplay`, `loop`, `muted`, and `preload` to customize the audio player’s behavior.

* Multiple `<source>` elements can be used to offer different audio formats, ensuring compatibility across browsers.

* You can include fallback content for browsers that don’t support the `<audio>` tag, such as a download link.

The `<audio>` tag is a simple and effective way to include audio files in your webpage, providing a seamless experience for your users without the need for third-party plugins or players.

