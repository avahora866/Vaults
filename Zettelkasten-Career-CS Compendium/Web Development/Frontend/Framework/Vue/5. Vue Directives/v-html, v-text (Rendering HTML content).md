---
tags:
  - Vue
---
In Vue.js, `v-html` and `v-text` are two directives used to render content dynamically in the DOM. Both serve different purposes, and understanding how they work will help you effectively handle dynamic content in your templates.

# `v-html` Directive

The `v-html` directive allows you to render raw HTML content in an element. This is useful when you need to display HTML markup, like when rendering rich text, user-generated content, or content fetched from an API that contains HTML.

**Usage:**

```vue
<div v-html="rawHtmlContent"></div>
```

In this example, `rawHtmlContent` is a data property in your Vue instance, and it might contain HTML tags. For example:

```javascript
new Vue({
  el: '#app',
  data: {
    rawHtmlContent: '<h1>Hello, World!</h1><p>This is <strong>raw HTML</strong> content.</p>'
  }
});
```

Here, the `v-html` directive will render the HTML tags as actual HTML content, meaning the result will be:

```vue
<h1>Hello, World!</h1>
<p>This is <strong>raw HTML</strong> content.</p>
```

# Security Warning with `v-html`

While `v-html` is useful for rendering HTML, it can be **dangerous** if you're rendering content that could come from an untrusted source. This is because Vue will **not sanitize** the HTML, which opens the door for **cross-site scripting (XSS)** attacks.

If you're rendering content that could come from an external source (like user-generated content), you should **sanitize** the HTML to prevent malicious scripts from being executed.

# `v-text` Directive

The `v-text` directive, on the other hand, is used to insert **plain text** into an element, without rendering any HTML tags. It is useful when you want to escape any special characters or ensure the content is displayed exactly as it is, without interpreting HTML.

**Usage:**

```vue
<div v-text="plainTextContent"></div>
```

Here, `plainTextContent` is a data property in your Vue instance, and it contains plain text:

```javascript
new Vue({
  el: '#app',
  data: {
    plainTextContent: 'This is <strong>plain text</strong>'
  }
});
```

In this case, the output will be:

```vue
<div>This is <strong>plain text</strong></div>
```

However, **the HTML tags will not be rendered**. Instead, the browser will display the text as:

```vue
This is <strong>plain text</strong>
```

So, `v-text` safely displays the raw text, without interpreting any HTML tags inside it.

# Key Differences Between `v-html` and `v-text`

| Feature | `v-html` | `v-text` |
| ----- | ----- | ----- |
| **Purpose** | Renders raw HTML content. | Renders plain text content. |
| **HTML Rendering** | Renders HTML tags (e.g., `<p>`, `<strong>`) as HTML. | Does not render HTML tags; escapes them as text. |
| **Security Risk** | Risk of XSS attacks if content is not sanitized. | No security risk; always escapes content. |
| **Use Case** | Use when you need to display rich HTML (e.g., formatted text, user-submitted HTML). | Use when you want to display text without HTML rendering. |

# When to Use `v-html` and `v-text`

* **Use `v-html`** when you need to render dynamic HTML content (e.g., formatted text, or any content that includes HTML tags). Always sanitize untrusted content before using it.

* **Use `v-text`** when you just need to display plain text and don't want to render any HTML tags. This is a safer choice when working with content from untrusted sources.

# Example of Both in Action

```vue
<div id="app">
  <div v-html="htmlContent"></div>
  <div v-text="textContent"></div>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    htmlContent: '<p><em>This is HTML content with <strong>tags</strong></em>.</p>',
    textContent: 'This is plain text content with <strong>tags</strong>.'
  }
});
```

Here:

* The `v-html` directive will render the HTML tags, so you'll see the formatted content.

* The `v-text` directive will escape the HTML tags and show them as plain text.

# Summary

* **`v-html`**: Renders raw HTML content. Be cautious with untrusted data to avoid XSS attacks.

* **`v-text`**: Renders plain text, escaping any HTML tags. Safe to use when you don’t want HTML rendering.

