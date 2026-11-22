---
tags:
  - FORMAT
  - HTML
---
Semantic HTML5 tags are elements that clearly describe their meaning in the context of a webpage, making the content more accessible to both developers and users, especially those using assistive technologies. They also help improve SEO (Search Engine Optimization) by providing better structure and meaning to the content on a webpage.

Here’s a breakdown of some common **semantic HTML5 tags** and their uses:

# `<header>`

The `<header>` tag is used to define a **header section** for a page or a specific section of content. Typically, it contains introductory content, navigation links, or logos.

## Example:

```
<header>
  <h1>My Website</h1>
  <nav>
    <ul>
    <li><a href="#home">Home</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>
</header>
```

*   
  The `<header>` can contain an **H1 tag** for the title, and a **navigation menu** (`<nav>`), logos, or other introductory content.

* **Purpose**: It introduces the page or section and is usually placed at the top of the document.

# `<footer>`

The `<footer>` tag defines a **footer section** of the page or section. It typically contains information like copyright notices, contact information, or links to privacy policies or terms of use.

## Example:

```
<footer>
  <p>&copy; 2025 My Website</p>
<p><a href="#privacy">Privacy Policy</a> | <a href="#terms">Terms of Service</a></p>
</footer>
```

*   
  The `<footer>` is typically placed at the bottom of the page or section.

* **Purpose**: It provides meta-information about the document, such as author details, legal disclaimers, or additional links.

# `<article>`

The `<article>` tag is used to define an independent **piece of content** that could be distributed and reused on its own. Examples of articles include blog posts, news articles, or forum posts.

## Example:

```
<article>
  <h2>Understanding HTML5 Semantic Tags</h2>
  <p>HTML5 introduces a number of new semantic tags to improve the structure and meaning of web documents...</p>
</article>
```

*   
  The `<article>` element is often used for content that stands alone, like individual blog posts or product descriptions.

* **Purpose**: It’s used for self-contained content that can be syndicated or distributed independently.

# `<section>`

The `<section>` tag is used to group content that is thematically related, and it should be used when you want to break up a document into sections that could have their own heading. Each section typically has a related topic or theme.

## Example:

```
<section>
  <h2>HTML5 Tags</h2>
  <p>HTML5 introduces several new semantic elements to better structure content...</p>
</section>
```

*   
  The `<section>` element is a way to group related content under a single heading.

* **Purpose**: It's typically used to divide content into logical sections, each of which could have its own heading.

# `<nav>`

The `<nav>` tag is used to define a **navigation** section that contains links to other parts of the website or other websites. It is commonly used for the main menu of the site.

## Example:

```
<nav>
  <ul>
  <li><a href="#home">Home</a></li>
  <li><a href="#about">About</a></li>
  <li><a href="#services">Services</a></li>
  <li><a href="#contact">Contact</a></li>
  </ul>
</nav>
```

*   
  The `<nav>` tag helps identify the navigation area of the page.

* **Purpose**: It is used to wrap links to help users and search engines understand where the primary navigation of the site resides.

# `<aside>`

The `<aside>` tag is used for content that is **related to** the main content but could be considered separate or tangential. This could include sidebars, pull quotes, or additional information like advertisements or links to related content.

## Example:

```
<aside>
  <h3>Related Articles</h3>
  <ul>
  <li><a href="#article1">Understanding Semantic HTML5</a></li>
  <li><a href="#article2">The Evolution of Web Design</a></li>
  </ul>
</aside>
```

*   
  The `<aside>` tag is typically used for content that supports the main content but isn't part of the central narrative.

* **Purpose**: It provides auxiliary information that is not essential but related, such as sidebars or related links.

# `<main>`

The `<main>` tag is used to wrap the **primary content** of a document, excluding headers, footers, and sidebars. It helps search engines and assistive technologies identify the central content of the page.

## Example:

```
<main>
  <article>
    <h2>How to Build a Website</h2>
    <p>Building a website can seem like a daunting task, but with the right tools and knowledge...</p>
  </article>
</main>
```

*   
  The `<main>` element is used to contain the main content of the page, which is distinct from header, footer, and sidebar content.

* **Purpose**: It identifies the dominant content on the page, which can improve accessibility.

# `<mark>`

The `<mark>` tag is used to highlight or **mark** text. It is useful for highlighting portions of text that are relevant or have been emphasized, such as search results or important parts of the document.

## Example:

```
<p>Use the <mark>search</mark> function to find the information you need.</p>
```

*   
  The text inside the `<mark>` tag is typically displayed with a yellow background.

* **Purpose**: To highlight text for emphasis or relevance, making it easier for users to find important parts of content.

# `<figure>` and `<figcaption>`

The `<figure>` tag is used to group multimedia content, such as images, illustrations, videos, etc., along with a **caption** describing the content. The `<figcaption>` tag is used to provide a caption for the content inside the `<figure>`.

## Example:

```
<figure>
  <img src="image.jpg" alt="An example image">
  <figcaption>This is an example image caption.</figcaption>
</figure>
```

*   
  The `<figure>` element is used for images, illustrations, diagrams, and other media.

* The `<figcaption>` provides a caption or explanation for the content inside the `<figure>`.

# Benefits of Semantic HTML5 Tags

1. **Improved Accessibility**: Screen readers and assistive technologies can better interpret the structure of the document.

2. **SEO Benefits**: Search engines can better understand the content and context of a webpage, improving indexing and ranking.

3. **Better Readability**: Semantic tags improve the readability of the HTML code, making it easier to understand and maintain.

4. **Clearer Document Structure**: Semantic tags define a clear structure, which helps organize content logically.

# Example of a Complete Layout Using Semantic Tags:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sample Page</title>
</head>
<body>
  <header>
    <h1>Welcome to My Website</h1>
    <nav>
      <ul>
      <li><a href="#home">Home</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <h2>What is Semantic HTML?</h2>
      <p>Semantic HTML helps to create a more accessible, readable, and maintainable webpage...</p>
    </article>

    <section>
      <h2>How Semantic Tags Improve SEO</h2>
      <p>Using semantic HTML5 tags makes it easier for search engines to understand the structure and relevance of the content...</p>
    </section>
  </main>

  <aside>
    <h3>Related Topics</h3>
    <ul>
    <li><a href="#seo">SEO Best Practices</a></li>
    <li><a href="#accessibility">Web Accessibility</a></li>
    </ul>
  </aside>

  <footer>
    <p>&copy; 2025 My Website</p>
  </footer>
</body>
</html>
```

# Summary of Common Semantic HTML5 Tags:

| Tag | Description |
| ----- | ----- |
| **`<header>`** | Defines the header content of a page or section. |
| **`<footer>`** | Defines the footer content of a page or section. |
| **`<article>`** | Represents a self-contained piece of content (e.g., blog posts). |
| **`<section>`** | Represents a section of content, typically with a heading. |
| **`<nav>`** | Defines a navigation block for links to other parts of the site. |
| **`<aside>`** | Defines content related to the main content but separate (e.g., sidebar). |
| **`<main>`** | Defines the main content area of a webpage. |
| **`<mark>`** | Highlights text for emphasis. |
| **`<figure>`** | Used for images and other media, with an optional caption. |
| **`<figcaption>`** | Provides a caption for the content inside a `<figure>`. |

Using semantic HTML tags helps to create a clearer structure, better accessibility, and improved SEO, making your websites more efficient and user-friendly.

