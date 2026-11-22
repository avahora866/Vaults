---
tags:
  - Vue
---
# Installing Vue Router

If you're using **Vue 3** and need to add routing to your project, here's how you can install Vue Router:

## Install with npm (Node.js package manager)

Run the following command in your terminal:

```
npm install vue-router@4
```

This installs **Vue Router 4.x**, which is compatible with **Vue 3**.

## Install with yarn (if you're using Yarn as a package manager)

```
yarn add vue-router@4
```

This will add Vue Router to your project and ensure it works with Vue 3\.

---

# Setting Up Vue Router

After installation, you'll need to configure Vue Router in your project. Here's a step-by-step guide to get you started.

## Create a `router.js` file

Typically, you create a `router.js` or `router/index.js` file where you'll define the routing logic.

Example (`router.js`):

```javascript
import { createRouter, createWebHistory } from 'vue-router';

// Import components (pages)
import Home from './components/Home.vue';
import About from './components/About.vue';

// Define routes
const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  }
];

// Create the router instance and pass the `routes` option
const router = createRouter({
  history: createWebHistory(),  // Use the HTML5 History API (recommended for modern browsers)
  routes  // Array of route objects
});

export default router;
```

In this example:

* We're using `createRouter` to create a new router instance.

 `createWebHistory()` enables HTML5 history mode (clean URLs without hash `#`).

* We define an array of **routes**, where each route maps a **path** (URL) to a Vue component (e.g., `Home` and `About`).

## Integrate Vue Router with the Vue Application

In your `main.js` (or `main.ts` if using TypeScript), you'll need to tell Vue to use the Vue Router instance.

Example (`main.js`):

```javascript
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';  // Import the router

const app = createApp(App);

// Use the router
app.use(router);

app.mount('#app');
```

Here, we call `app.use(router)` to install the router instance into the Vue application, making it available to all components.

---

# Using `<router-view>` for Rendering Views

In the main layout or `App.vue`, you need to include a `<router-view>` element. This is where the matched components for the current route will be displayed.

Example (`App.vue`):

```vue
<template>
  <div>
    <h1>Welcome to My Vue App</h1>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </nav>

    <!-- This is where the route component will be displayed -->
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

*   
  **`<router-view>`**: This is a placeholder where the component associated with the current route will be injected and rendered.

* **`<router-link>`**: These are used for navigation. They automatically handle the routing without triggering a page reload.

---

# Run Your Application

Once everything is set up, run your application using:

```
npm run serve
```

Your Vue app should now be set up with Vue Router. You can navigate between different routes (`/` and `/about`) without a full page reload, which is the power of a Single-Page Application (SPA).

---

# Optional: Adding Dynamic Routes and Route Parameters

You can define routes that accept parameters, for example, to display a user profile based on an ID.

Example with dynamic routes:

```javascript
import UserProfile from './components/UserProfile.vue';

const routes = [
  {
    path: '/user/:id',  // `:id` is a dynamic route parameter
    name: 'UserProfile',
    component: UserProfile
  }
];
```

Now, when you visit `/user/1`, Vue Router will pass `1` as the `id` parameter to the `UserProfile` component, where you can access it using `$route.params.id`.

---

# Summary of the Steps

1. **Install Vue Router** with `npm install vue-router@4`.

2. **Set up a router file** to define routes and create a router instance.

3. **Integrate Vue Router** in your app by calling `app.use(router)` in `main.js`.

4. **Use `<router-view>`** in `App.vue` to display the component that matches the current route.

5. **Navigate using `<router-link>`** to trigger route changes.

---

