---
tags:
  - Vue
---
# What Are Dynamic Routes?

A **dynamic route** allows you to define URL paths with **parameters**. These parameters act as placeholders that can match any value in that segment of the URL. In Vue Router, you define dynamic routes by using a colon (`:`) in the path to signify a dynamic parameter.

For example:

* `/user/:id` is a dynamic route where `:id` is a placeholder for a dynamic parameter, such as `1`, `42`, or any other value.

When a user navigates to a route with a dynamic segment, Vue Router will extract the dynamic part of the URL (e.g., `1` in `/user/1`) and make it available in your component, typically via the `$route.params` object.

---

# Example of Defining a Dynamic Route

## Step 1: Define the Route in `router.js`

You can define dynamic routes in the `routes` array by using a colon (`:`) followed by the parameter name. Here's an example:

```javascript
import { createRouter, createWebHistory } from 'vue-router';
import UserProfile from './components/UserProfile.vue';

const routes = [
  {
    path: '/user/:id',   // Dynamic route with a parameter
    name: 'UserProfile',
    component: UserProfile,
    props: true           // Pass the dynamic parameter as a prop to the component
  }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;
```

*   
  **Dynamic Parameter (`:id`)**: In the path `/user/:id`, `:id` is a dynamic segment that matches any value in that position.

* **Component (`UserProfile`)**: When a user navigates to `/user/:id`, the `UserProfile` component is rendered.

* **`props: true`**: This ensures that the dynamic parameter (e.g., `id`) is passed as a prop to the `UserProfile` component.

---

## Step 2: Accessing the Parameter in the Component

Now, in your component (e.g., `UserProfile.vue`), you can access the dynamic parameter through `props`, because we set `props: true` in the router definition.

Example (`UserProfile.vue`):

```vue
<template>
  <div>
    <h1>User Profile</h1>
    <p>User ID: {{ id }}</p>  <!-- Display the dynamic parameter -->
  </div>
</template>

<script>
export default {
  props: ['id'],  // `id` is passed as a prop from the route
};
</script>
```

*   
  **Accessing `id`**: The `id` parameter from the URL (`/user/1`, `/user/42`, etc.) is passed as a prop to the `UserProfile` component.

* **Display `id`**: You can now use `{{ id }}` to display the dynamic parameter in your template.

For example, if the user visits `/user/1`, `id` will be `1`, and the component will display:

```
User Profile
User ID: 1
```

---

# Step 3: Navigating to Dynamic Routes

You can programmatically navigate to a dynamic route using **`$router.push`** and pass parameters.

Example:

```javascript
this.$router.push({ name: 'UserProfile', params: { id: 42 } });
```

This will navigate to `/user/42` and render the `UserProfile` component with the `id` prop set to `42`.

Alternatively, you can use a **`<router-link>`** to create a clickable link that navigates to a dynamic route:

```vue
<router-link :to="{ name: 'UserProfile', params: { id: 42 } }">Go to User 42</router-link>
```

---

# Dynamic Routes with Multiple Parameters

You can also define routes with multiple dynamic parameters. For example, a route that matches `/post/:postId/comment/:commentId`:

```javascript
const routes = [
  {
    path: '/post/:postId/comment/:commentId',
    name: 'CommentDetail',
    component: CommentDetail,
    props: true
  }
];
```

In the `CommentDetail` component, you would access both `postId` and `commentId` as props:

```vue
<template>
  <div>
    <h1>Post {{ postId }} - Comment {{ commentId }}</h1>
  </div>
</template>

<script>
export default {
  props: ['postId', 'commentId'],
};
</script>
```

When navigating to `/post/1/comment/5`, the component would render:

```
Post 1 - Comment 5
```

---

# Dynamic Route Matching (Optional: Regex and Validation)

You can use regular expressions in the path to apply custom matching rules. For instance, if you want the `id` to only accept numerical values, you can define the route like this:

```javascript
{
  path: '/user/:id(\\d+)',  // Only match numerical `id` values
  name: 'UserProfile',
  component: UserProfile
}
```

Here, `:id(\\d+)` ensures that only numeric values are matched for `id` (e.g., `/user/1`, but not `/user/abc`).

---

# Using Query Parameters (Optional)

In addition to path parameters (like `:id`), you can also pass **query parameters** in Vue Router, like `/user?id=1`. These can be accessed via `$route.query`.

Example:

```javascript
{
  path: '/user',
  name: 'UserProfile',
  component: UserProfile
}
```

You can navigate with query parameters like this:

```javascript
this.$router.push({ name: 'UserProfile', query: { id: 42 } });
```

In the component, you can access the `id` query parameter as follows:

```javascript
console.log(this.$route.query.id);  // Will log '42'
```

---

# Summary

1. **Dynamic Route (`:id`)**: A dynamic route uses a colon (`:`) in the path to represent a variable, such as `/user/:id`.

2. **Accessing Parameters**: You can access the dynamic parameter (`id`) in the component via **props** (with `props: true` in the route definition).

3. **Programmatic Navigation**: Use `this.$router.push({ name: 'routeName', params: { id: value } })` to navigate to dynamic routes.

4. **Multiple Parameters**: You can have multiple dynamic parameters (e.g., `/post/:postId/comment/:commentId`).

5. **Validation**: Use regular expressions for additional validation of dynamic parameters (e.g., only allow numeric `id`).

Dynamic routes are essential for building applications where the content is determined by parameters in the URL, such as user profiles, product pages, and blog posts.

