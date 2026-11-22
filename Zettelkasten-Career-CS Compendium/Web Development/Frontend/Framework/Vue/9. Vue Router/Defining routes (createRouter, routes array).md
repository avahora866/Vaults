---
tags:
  - Vue
---
# What Are Routes in Vue Router?

In Vue Router, **routes** are the mappings between URL paths and Vue components. When a user navigates to a specific URL, Vue Router matches the path to the corresponding component and renders it.

The main method for defining routes is using the **`routes` array** inside the **`createRouter`** function. Each route object in the array represents a mapping between a URL path and a component.

---

# Defining Routes with `createRouter`

To define routes in a Vue 3 application using Vue Router, you typically use the `createRouter` function. This function takes a **history mode** (which defines how URLs are handled) and a **routes array** (where the route paths are defined).

Here’s how it works:

## Example Setup: `router.js` (or `router/index.js`)

```javascript
import { createRouter, createWebHistory } from 'vue-router';

// Import your components (views)
import Home from './components/Home.vue';
import About from './components/About.vue';
import UserProfile from './components/UserProfile.vue';

// Define your routes
const routes = [
  {
    path: '/',  // URL path
    name: 'Home',  // Name of the route (optional, but useful)
    component: Home  // Component that will be rendered for this path
  },
  {
    path: '/about',
    name: 'About',
    component: About
  },
  {
    path: '/user/:id',  // Dynamic route (e.g., /user/1, /user/2)
    name: 'UserProfile',
    component: UserProfile,
    props: true  // Pass the route params (like 'id') as props to the component
  }
];

// Create the router instance
const router = createRouter({
  history: createWebHistory(),  // Use HTML5 history mode
  routes  // Array of route definitions
});

export default router;
```

# Breakdown of the code:

* **`createRouter`**: This function creates a router instance.

  * **`history`**: This defines the history mode of the router.

     `createWebHistory()` is used for HTML5 history mode, which uses clean URLs (without a hash `#`).

     You can also use `createWebHashHistory()` if you need to support hash-based URLs (e.g., `http://example.com/#/about`).

  * **`routes`**: An array of route objects. Each object defines a path and the corresponding component.

---

# Route Object Details

Each object in the `routes` array defines a **route**. Here’s a breakdown of common properties:

## Example Route Object

```javascript
{
  path: '/about',  // The URL path for this route
  name: 'About',    // A unique name for the route (optional)
  component: About, // The Vue component to render when this path is matched
  props: true,      // If true, route parameters will be passed as props to the component
}
```

*   
  **`path`**: This is the URL pattern (e.g., `/about`). It can also include **dynamic parameters** (e.g., `/user/:id`) to match dynamic segments of the URL.

* **`name`**: This is an optional name for the route. Using a named route allows you to refer to the route by its name in the application, which is useful for programmatic navigation (e.g., `router.push({ name: 'About' })`).

* **`component`**: This is the **Vue component** that will be rendered when the route is matched.

* **`props`**: When set to `true`, the route parameters (e.g., `:id`) will be passed as **props** to the component. This is a useful feature when working with dynamic routes.

---

# Dynamic Routes

You can define dynamic routes that accept parameters from the URL. For example, if you want to display a user profile based on their ID, you can use a **dynamic route** like `/user/:id`.

```javascript
{
  path: '/user/:id',  // Dynamic route with a parameter `id`
  name: 'UserProfile',
  component: UserProfile,
  props: true  // Automatically pass `id` as a prop to the component
}
```

*   
  **Dynamic Parameter**: In the path `/user/:id`, `:id` is a **dynamic parameter** that matches any value in that position. For example:

  * `/user/1` → `id = 1`

  * `/user/42` → `id = 42`

* **Accessing the Parameter**: In the `UserProfile` component, you can access this parameter as a prop:

```vue
<template>
  <div>
    <h1>User Profile</h1>
    <p>User ID: {{ id }}</p>
  </div>
</template>

<script>
export default {
  props: ['id']  // `id` is passed as a prop from the route
};
</script>
```

---

# Named Routes

Named routes allow you to refer to a route by its name instead of its path. This is useful for programmatic navigation or when using `<router-link>`.

## Example of Named Route

```javascript
{
  path: '/about',
  name: 'About',  // Named route
  component: About
}
```

You can navigate to the "About" route programmatically using its name:

```javascript
this.$router.push({ name: 'About' });
```

Or use `<router-link>` to navigate to the named route:

```vue
<router-link :to="{ name: 'About' }">Go to About</router-link>
```

---

# Nested Routes

Vue Router also supports **nested routes**, where you can define routes within other routes. This is useful for creating layouts with child views.

## Example of Nested Routes

```javascript
const routes = [
  {
    path: '/dashboard',
    component: DashboardLayout,
    children: [
      {
        path: '',  // Default child route
        component: DashboardHome
      },
      {
        path: 'settings',
        component: DashboardSettings
      }
    ]
  }
];
```

In this example:

* When the user navigates to `/dashboard`, it will render the `DashboardLayout` component.

* The `DashboardLayout` component can include a `<router-view>` to display nested routes like `DashboardHome` and `DashboardSettings` depending on the path.

---

# Final Setup

Once you've defined the routes and created the router instance, you need to use it in your **main application file** (`main.js` or `main.ts`).

```javascript
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';  // Import the router

const app = createApp(App);
app.use(router);  // Use the router instance
app.mount('#app');
```

---

# Summary

* **`createRouter`**: A function that creates a router instance for your Vue app.

* **`routes`**: An array that defines the routing paths and the components that should be rendered for each path.

* **`path`**: The URL path that the route will match.

* **`component`**: The Vue component that will be displayed when the route is matched.

* **`props`**: A way to pass route parameters as props to the components.

* **Named Routes**: Allow you to reference routes by name instead of their path.

