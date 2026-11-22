---
tags:
  - Vue
---
**Static Site Generation (SSG)** is the process of pre-rendering a website at build time into static HTML files, rather than rendering content dynamically at runtime (like with Server-Side Rendering). With SSG, the content of the pages is generated once during the build process, and these pre-rendered HTML files are served to the users when they request the page.

This approach is great for websites where the content doesn't change often, such as blogs, marketing sites, documentation, or portfolios. Since the content is already rendered and stored as static files, these sites tend to have very fast load times, which improves user experience and SEO.

# How Static Site Generation (SSG) Works

1. **Build Time Rendering**: During the build process, the content for each page is fetched (e.g., from APIs, databases, or markdown files), and static HTML is generated for each route.

2. **Serve Pre-rendered Pages**: The HTML files are stored in a `dist/` (or equivalent) directory, which can be served by any static file hosting service, like Netlify, Vercel, GitHub Pages, or AWS S3.

3. **No Dynamic Content at Runtime**: Since the content is generated at build time, there is no need for a backend server to render the page on each request. This reduces server load, increases performance, and simplifies deployment.

# Benefits of Static Site Generation (SSG)

1. **Fast Performance**: Static pages load very quickly because there is no need to wait for server-side rendering on each request. The browser just serves static files.

2. **Improved SEO**: Since all content is available in the pre-rendered HTML, search engines can easily crawl and index the content. No JavaScript execution is needed to access the content.

3. **Security**: Static sites don’t require a server running at all times, so there’s less surface area for attacks. The absence of dynamic backends also reduces potential vulnerabilities.

4. **Lower Hosting Costs**: Static sites can be hosted on inexpensive platforms like GitHub Pages, Netlify, and Vercel, which provide free or cheap hosting options.

5. **Scalability**: Static sites can scale effortlessly, as the hosting provider simply serves static files from a CDN (Content Delivery Network). There are no concerns about handling sudden traffic spikes or maintaining server infrastructure.

---

# How to Implement SSG in Nuxt.js

Nuxt.js, a popular framework for building Vue.js applications, provides excellent support for Static Site Generation out of the box. Here’s how you can use Nuxt.js to generate a static site.

## Set Up Nuxt.js Project

You can create a Nuxt.js project using the `create-nuxt-app` command.

```
npx create-nuxt-app my-ssg-app
```

Follow the setup prompts to choose your preferences (e.g., TypeScript, CSS framework, etc.).

## Set Static Target in `nuxt.config.js`

In Nuxt.js, you can specify the **target** mode in the `nuxt.config.js` file to indicate that you want to generate a static site.

```javascript
export default {
  target: 'static',  // This enables Static Site Generation
}
```

By setting `target: 'static'`, Nuxt will configure the application to generate static HTML files during the build process. This mode enables **SSG** and disables **SSR**.

## Use `asyncData` for Data Fetching

If you need to fetch data during the build process (e.g., pulling data from an API or a CMS), you can use the `asyncData` hook. This hook allows Nuxt.js to fetch the data for each page before generating the static HTML.

Here’s an example of using `asyncData` to fetch data for a blog page:

```javascript
export default {
  async asyncData({ params }) {
    const response = await fetch(`https://api.example.com/posts/${params.id}`);
    const post = await response.json();
    return { post };
  }
}
```

In this example:

* `asyncData` will be called during build time.

* It fetches data about a specific blog post using the `id` from the URL parameters.

* Nuxt will generate static HTML files for each blog post with the fetched data.

## Generate Static Pages

To generate the static files for your application, you use the `nuxt generate` command. This command builds your site and outputs the static files to a `dist/` directory.

Run the following command:

```
npm run generate
```

After running this command, the static files (HTML, CSS, JS) are generated in the `dist/` folder. You can now deploy these files to any static hosting service like **Netlify**, **Vercel**, or **GitHub Pages**.

## Deploy Your Static Site

Once the static site is generated, you can deploy it to any static file hosting service. Some common services are:

* **Netlify**: Drag and drop the `dist/` folder or connect your GitHub repository.

* **Vercel**: Automatically deploy your site from GitHub or GitLab.

* **GitHub Pages**: Serve the contents of the `dist/` folder from a GitHub repository.

---

# Dynamic Routes and Static Site Generation

For dynamic routes (e.g., blog posts with different URLs based on the content), Nuxt.js allows you to generate static pages for these routes using the `generate` method.

You can specify dynamic routes in the `nuxt.config.js` file like so:

```javascript
export default {
  generate: {
    routes: async () => {
      const res = await fetch('https://api.example.com/posts');
      const posts = await res.json();
      return posts.map(post => `/posts/${post.id}`);
    }
  }
}
```

In this example:

* The `generate.routes` function fetches a list of posts from an API.

* It generates static routes for each post using their IDs (`/posts/1`, `/posts/2`, etc.).

* Nuxt will pre-render each of these routes as static HTML files during the build process.

---

# SSG vs SSR (Server-Side Rendering)

While **SSG** and **SSR** both improve SEO and performance by rendering content on the server, they differ in how and when the content is rendered.

* **SSG (Static Site Generation)**:

  * Renders the site at **build time**.

  * The content is static and doesn’t change unless you rebuild the site.

  * Suitable for sites with content that doesn’t change frequently (e.g., blogs, marketing sites, documentation).

* **SSR (Server-Side Rendering)**:

  * Renders the site at **request time** (when a user accesses the page).

  * Dynamic content can be rendered on each request.

  * Suitable for sites with frequently changing content (e.g., e-commerce sites, user dashboards).

---

# SSG vs Client-Side Rendering (CSR)

* **SSG (Static Site Generation)**:

  * Pre-renders content at build time and serves static HTML to the client.

  * Faster initial load time and better SEO because content is available immediately.

* **CSR (Client-Side Rendering)**:

  * Renders content in the browser using JavaScript.

  * The browser downloads JavaScript, executes it, and renders content dynamically.

  * Slower initial page load compared to SSG and worse SEO since the content is only available after JavaScript is executed.

---

# Conclusion

**Static Site Generation (SSG)** with Nuxt.js is an excellent choice for creating fast, SEO-friendly websites with content that doesn't change frequently. By generating static HTML files during the build process, you eliminate the need for a server to render pages on each request, resulting in faster performance and reduced hosting costs.

Nuxt.js simplifies SSG by providing a straightforward configuration and data-fetching mechanism with `asyncData` and `generate`. The generated static files can be deployed to various static hosting platforms, making the process seamless and scalable.

SSG is particularly suited for marketing pages, blogs, documentation sites, and other content-driven websites where content updates are infrequent.

