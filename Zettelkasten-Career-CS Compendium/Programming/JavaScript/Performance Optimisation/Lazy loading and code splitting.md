---
tags:
  - FORMAT
  - JavaScript
---
# Lazy Loading and Code Splitting in JavaScript

Both **lazy loading** and **code splitting** are performance optimization techniques used in modern JavaScript development to improve the **initial load time** of applications and **reduce the overall resource consumption** by loading resources only when they are needed.

These techniques are especially important for Single Page Applications (SPAs), where large JavaScript bundles can slow down the initial loading of the page.

---

# Lazy Loading

**Lazy loading** is a design pattern that postpones the loading of non-essential resources until they are actually needed. Instead of loading everything at once, resources (like scripts, images, or components) are loaded **on-demand**, which reduces the initial load time and improves the overall performance.

## How Lazy Loading Works

* Resources are only loaded when required (e.g., when the user scrolls to a certain section of a page, or when a specific route is accessed).

* The browser loads only the **critical code** for the initial page load, deferring the rest of the resources until they are needed.

## Use Cases for Lazy Loading:

* **Images**: Load images only when they enter the viewport (on-scroll).

* **JavaScript Modules**: Load JavaScript code only when a user navigates to a specific part of the app.

* **Components in SPAs**: Load parts of a user interface only when the user interacts with them.

## Lazy Loading with JavaScript Example (Dynamic Import)

In modern JavaScript, lazy loading can be implemented using **dynamic `import()`** syntax to load modules only when required.

**Example of Lazy Loading with Dynamic Imports**:

```javascript
// A button click triggers the lazy loading of the 'heavyComponent' module
const button = document.getElementById("loadComponentButton");
button.addEventListener("click", () => {
    import("./heavyComponent.js")
        .then(module => {
            const HeavyComponent = module.default;
            // Use the heavy component once it's loaded
            const element = new HeavyComponent();
            document.body.appendChild(element.render());
        })
        .catch(error => console.log("Error loading component:", error));
});
```

*   
  When the button is clicked, the `heavyComponent.js` file is loaded dynamically. Until then, the browser doesn't download or execute it.

* The module is **only loaded when required**, making the initial load faster.

## Lazy Loading Images Example:

You can also use lazy loading for images in modern web development:

```
<img src="placeholder.jpg" data-src="real-image.jpg" class="lazyload" alt="Lazy loaded image">
```

```javascript
// Lazy load images as they enter the viewport
const lazyImages = document.querySelectorAll('img.lazyload');

const loadImage = (image) => {
    const src = image.getAttribute('data-src');
    image.src = src;
    image.classList.remove('lazyload');
};

const imageObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            loadImage(entry.target);
            observer.unobserve(entry.target);
        }
    });
});

lazyImages.forEach(image => imageObserver.observe(image));
```

*   
  In this example, images are loaded only when they come into view (using the **IntersectionObserver API**), which reduces the amount of content that needs to be loaded initially.

---

# Code Splitting

**Code splitting** is the process of dividing your JavaScript code into smaller chunks, which are loaded only when needed, instead of loading the entire codebase in a single file. Code splitting helps **reduce the size of the initial bundle** and allows the browser to download only the necessary parts of the code.

## How Code Splitting Works

* The JavaScript code is broken up into smaller **chunks**.

* These chunks are only downloaded when specific routes or components are accessed, leading to **faster initial page loads**.

* The goal is to optimize performance by **decreasing the size of the initial JavaScript bundle**.

## Types of Code Splitting

1. **Entry Points**: Splitting the code into multiple entry points. Each entry point can load a different chunk of code, so only the necessary files are loaded for each entry.

    **Example**: A site might have a separate chunk for the homepage, the about page, and the contact page.

2. **Dynamic Imports**: With dynamic imports, code is split at runtime and only the code required for the current view or feature is loaded.

3. **Vendor Splitting**: Third-party libraries (like React or Lodash) can be placed into separate chunks, so they're loaded once and cached for later visits.

## Code Splitting with Webpack

Webpack is one of the most popular bundlers that provides built-in support for code splitting. It can automatically split your code into smaller chunks based on routes, dynamic imports, or vendor libraries.

**Example: Basic Code Splitting with Webpack**:

```javascript
// webpack.config.js
module.exports = {
    entry: './src/index.js', // Main entry point
    output: {
        filename: '[name].[contenthash].js', // This will generate hashed filenames
        path: path.resolve(__dirname, 'dist')
    },
    optimization: {
        splitChunks: {
            chunks: 'all', // Split the code into separate chunks
        }
    }
};
```

In this example, Webpack will automatically split your code into smaller chunks, including vendor libraries, to optimize the performance.

## Code Splitting with Dynamic Import and Webpack

Using dynamic `import()` with Webpack allows you to split your code based on user interaction. Webpack will automatically create separate chunks for these dynamic imports.

```javascript
// Example of dynamic import with Webpack for route-based code splitting
document.getElementById("loadAboutPage").addEventListener("click", () => {
    import('./aboutPage.js').then(module => {
        const AboutPage = module.default;
        const page = new AboutPage();
        document.body.appendChild(page.render());
    });
});
```

*   
  Webpack will automatically create a separate chunk for `aboutPage.js` and load it only when the user clicks the button.

## Code Splitting with React (React.lazy and Suspense)

In React, you can use `React.lazy()` and `Suspense` to implement code splitting for components.

```javascript
import React, { Suspense } from 'react';

// Lazy load the 'HeavyComponent' only when needed
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

function App() {
    return (
        <div>
            <h1>My App</h1>
            <Suspense fallback={<div>Loading...</div>}>
                <HeavyComponent />
            </Suspense>
        </div>
    );
}

export default App;
```

*   
  `React.lazy()` ensures that the `HeavyComponent` is only loaded when it's rendered for the first time.

* `Suspense` is used to display a loading state while the component is being loaded.

---

# Lazy Loading vs. Code Splitting

| Aspect | Lazy Loading | Code Splitting |
| ----- | ----- | ----- |
| **Concept** | Delays the loading of non-essential resources until they are needed (e.g., images, components). | Divides the code into smaller chunks that are loaded only when needed. |
| **Use Case** | Loading resources like images or JavaScript modules on-demand. | Splitting large bundles of JavaScript into smaller chunks for better performance. |
| **Focus** | Delays the execution of resources. | Splits the codebase into smaller files for faster initial loads. |
| **Example** | Lazy loading images, components, or APIs when they become visible or needed. | Splitting JavaScript files for different routes, vendor libraries, etc. |
| **Performance Benefit** | Reduces initial page load time. | Reduces the size of the initial JavaScript bundle, speeding up page load. |

---

# Key Takeaways

* **Lazy Loading**: Postpones the loading of resources until they are needed, improving initial load times and reducing resource consumption.

* **Code Splitting**: Breaks your JavaScript into smaller chunks, loading only the necessary chunks for the current user interaction (e.g., a route or a component), leading to faster initial load times.

* **Webpack** and **React** provide powerful tools for implementing both lazy loading and code splitting.

Would you like more detailed examples or guidance on how to set this up in a specific framework (e.g., React, Angular, or Vue)?

