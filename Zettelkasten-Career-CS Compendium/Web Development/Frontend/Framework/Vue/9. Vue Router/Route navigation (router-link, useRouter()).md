---
tags:
  - Vue
---
# Route Navigation Using `<router-link>`

The **`<router-link>`** component is the **declarative way** of navigating between routes in Vue. It automatically handles the navigation without causing a page reload. This is the most common way of linking to different routes in Vue applications.

## Basic Usage of `<router-link>`

```vue
<template>
  <div>
    <!-- Declarative Navigation -->
    <router-link to="/home">Go to Home</router-link>
  </div>
</template>
```

In this example:

* **`<router-link>`** is used to create a clickable link.

* The **`to`** attribute specifies the path to navigate to (in this case, `/home`).

When the user clicks "Go to Home," Vue Router will navigate to the `/home` route without reloading the page.

## Key Attributes of `<router-link>`

* **`to`**: This is the destination URL or path. It can also be a route name (if you defined a named route).

```vue
<router-link :to="{ name: 'About' }">Go to About</router-link>
```

*   
  **`exact`**: If set to `true`, it makes sure the link is active only when the exact route is matched.

```vue
<router-link to="/about" exact>Exact match</router-link>
```

*   
  **`active-class`**: By default, the active class (`router-link-active`) is applied to the link when the route it points to is matched. You can customize the active class name using this prop:

```vue
<router-link to="/about" active-class="my-custom-active-class">Go to About</router-link>
```

*   
  **`replace`**: When set to `true`, it will **replace** the current history entry instead of pushing a new one. This is useful for scenarios like login redirection.

```vue
<router-link to="/home" replace>Go to Home (no history)</router-link>
```

## Conditional Rendering Based on Route

You can use the `active` class or programmatically check which route is active to apply different styles or behaviors. Vue Router automatically applies the `router-link-active` class when the linked route is active.

```vue
<router-link to="/home" active-class="active">Home</router-link>
<router-link to="/about" active-class="active">About</router-link>
```

---

# Programmatic Navigation with `useRouter()`

In addition to **declarative navigation** via `<router-link>`, Vue Router also supports **programmatic navigation**, where you can navigate to different routes through JavaScript. This is especially useful in event handlers, methods, or lifecycle hooks.

## `useRouter()` in Vue 3

In **Vue 3**, you can use the **`useRouter()`** hook to access the **router instance** within a **composition API** component. The router instance has methods like **`push()`**, **`replace()`**, and **`go()`** to perform navigation.

```javascript
import { useRouter } from 'vue-router';

export default {
  setup() {
    const router = useRouter(); // Get the router instance

    const goToHome = () => {
      router.push('/home'); // Navigate programmatically to the /home route
    };

    return { goToHome };
  }
}
```

In the above example:

* We use the `useRouter()` hook to get access to the router instance inside the **`setup()`** function (part of Vue 3’s Composition API).

* The `goToHome` method uses **`router.push()`** to navigate to the `/home` route when invoked.

## Other Programmatic Navigation Methods

* **`router.push()`**: Navigates to a new route by **adding a new entry** to the history stack (like clicking a link).

```javascript
router.push('/about');  // Navigate to /about
router.push({ name: 'UserProfile', params: { id: 42 } });  // Navigate using named route with parameters
```

*   
  **`router.replace()`**: Similar to `push()`, but it **replaces the current history entry** instead of adding a new one. This is useful when you don't want the user to be able to go back to the previous page (for example, after a successful login).

```javascript
router.replace('/home');  // Replace the current page with /home
```

*   
  **`router.go()`**: This is like the browser’s back and forward buttons. You can use it to go back (`-1`) or forward (`1`) in the history.

```javascript
router.go(-1);  // Go back one page in the history stack (like the browser's back button)
router.go(1);   // Go forward one page in the history stack (like the browser's forward button)
```

---

# Example: Programmatically Navigating After an Action

Here's an example where we navigate to a new route after performing some action, such as submitting a form:

```vue
<template>
  <div>
    <button @click="submitForm">Submit</button>
  </div>
</template>

<script>
import { useRouter } from 'vue-router';

export default {
  setup() {
    const router = useRouter();

    const submitForm = () => {
      // Perform form submission logic (e.g., API call)
      
      // Navigate to another page after form submission
      router.push('/thank-you');
    };

    return { submitForm };
  }
};
</script>
```

In this example:

* **`submitForm()`** is called when the user clicks the "Submit" button.

* After submitting the form, the method uses **`router.push('/thank-you')`** to navigate to the `/thank-you` page programmatically.

---

# Using Named Routes in Programmatic Navigation

You can also use **named routes** in programmatic navigation, which makes your code more readable and maintainable. Named routes allow you to refer to routes by their name instead of their path.

## Defining a Named Route in `router.js`:

```javascript
const routes = [
  {
    path: '/user/:id',
    name: 'UserProfile', // Name the route
    component: UserProfile
  }
];
```

## Navigating Using the Named Route:

```javascript
router.push({ name: 'UserProfile', params: { id: 42 } }); // Navigate to /user/42 using the named route
```

In this example:

* We defined the route `/user/:id` with the name `UserProfile`.

* Using `router.push()`, we navigate to the route by specifying its **name** (`UserProfile`) and passing the required route parameters (`id: 42`).

---

# Programmatic Navigation with Query Parameters

You can also include **query parameters** in programmatic navigation.

```javascript
router.push({ path: '/search', query: { q: 'Vue.js' } });
```

In this example:

* The URL will change to `/search?q=Vue.js`.

* You can access the query parameter in the component with `this.$route.query.q` or `useRoute().query.q` in the composition API.

---

# Summary

* **`<router-link>`** is used for **declarative navigation** in templates, which automatically handles route navigation without reloading the page.

  * Use the `to` prop to specify the target route.

  * Can be used with named routes, dynamic paths, query parameters, and more.

* **`useRouter()`** is used for **programmatic navigation** in Vue 3's Composition API.

  * You can call `router.push()`, `router.replace()`, and `router.go()` for navigation.

  * You can use it in response to events, like button clicks, or after actions like form submissions.

* **Named Routes** make navigation easier by allowing you to refer to routes by their name instead of their path.

