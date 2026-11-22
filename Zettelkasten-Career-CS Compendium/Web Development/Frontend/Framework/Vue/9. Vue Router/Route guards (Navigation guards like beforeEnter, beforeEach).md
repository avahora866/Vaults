---
tags:
  - Vue
---
# What Are Route Guards?

**Route guards** in Vue Router are functions that allow you to intercept and control navigation before a route is entered or after it’s left. You can use these guards to perform actions like:

* **Authentication**: Check if the user is logged in before allowing access to certain routes.

* **Authorization**: Check if the user has the right permissions to access a route.

* **Data fetching**: Trigger data fetching before a component is rendered.

Vue Router provides several types of **navigation guards** that can be used at different points in the navigation process.

---

# Types of Route Guards

## Global Guards

Global guards are used across the entire application and are applied to every route navigation. They can be defined globally by calling methods on the **router instance**.

* **`beforeEach`**: Called before every navigation, no matter which route you’re navigating to.

* **`afterEach`**: Called after the navigation has been confirmed and the route has been entered (but after the component has been rendered).

## Per-Route Guards

Per-route guards are used for specific routes. They allow you to define navigation guards on an individual route, which are only applied to that route.

* **`beforeEnter`**: Called before the navigation happens, just before the component is resolved, for a specific route.

## In-Component Guards

You can also define guards within individual components using lifecycle hooks. These are useful for cases where you want to guard a route only when it is associated with a particular component.

* **`beforeRouteEnter`**: Called before the route is confirmed and before the component is created.

* **`beforeRouteUpdate`**: Called when the route changes, but the component is reused (when the route is the same but the parameters change).

* **`beforeRouteLeave`**: Called before leaving the route, allowing you to perform checks or cleanup before navigating away.

---

# Global Guards

## `beforeEach`

This is a **global guard** that is called **before every route navigation**. It is useful for cases like checking authentication or permissions before entering any route.

### Example: Authentication Guard with `beforeEach`

```javascript
import { createRouter, createWebHistory } from 'vue-router';
import Home from './components/Home.vue';
import Login from './components/Login.vue';

const routes = [
  { path: '/', component: Home },
  { path: '/login', component: Login }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

// Global before guard
router.beforeEach((to, from, next) => {
  // If the user is not logged in and tries to access a protected route
  if (to.path !== '/login' && !isUserLoggedIn()) {
    next('/login'); // Redirect to the login page
  } else {
    next(); // Allow navigation
  }
});

function isUserLoggedIn() {
  // Example function to check if user is logged in (could check a token, etc.)
  return false; // Simulating a logged-out user
}

export default router;
```

In this example:

* **`beforeEach`** checks if the user is logged in before allowing navigation to routes other than `/login`.

* If the user is not logged in, the guard redirects them to the `/login` route.

## `afterEach`

This guard is called after navigation has been confirmed. It's useful for performing tasks after a route has changed, like analytics or logging.

```javascript
router.afterEach((to, from) => {
  console.log('Navigation completed from', from.fullPath, 'to', to.fullPath);
});
```

This example logs the navigation details after the route change.

---

# Per-Route Guards

Per-route guards are used to define navigation guards for individual routes.

## `beforeEnter`

This guard is defined directly in the route definition and is called **before entering a route**. It's only applied to the route it's attached to.

### Example: Protecting a Route with `beforeEnter`

```javascript
const routes = [
  {
    path: '/admin',
    component: AdminPage,
    beforeEnter: (to, from, next) => {
      if (!isAdmin()) {
        next('/login'); // Redirect to login if the user is not an admin
      } else {
        next(); // Allow navigation
      }
    }
  }
];
```

In this example:

* **`beforeEnter`** checks if the user is an admin before allowing access to the `/admin` route. If not, they are redirected to `/login`.

---

# In-Component Guards

Vue provides lifecycle hooks to define route guards **within individual components**. These are useful when you need route-specific guards for specific components.

## `beforeRouteEnter`

This guard is called **before the route is confirmed**, but before the component is created. You cannot access `this` in this guard because the component instance hasn't been created yet.

### Example: Using `beforeRouteEnter` for Data Fetching

```javascript
export default {
  beforeRouteEnter(to, from, next) {
    // Perform some action before entering the route, like fetching data
    next((vm) => {
      // The component instance is available here
      vm.fetchData();
    });
  },
  methods: {
    fetchData() {
      // Fetch some data for the component
    }
  }
};
```

*   
  **`beforeRouteEnter`** allows you to fetch data or perform actions before the route is confirmed. After the route is confirmed, `next()` is called to proceed to the next step.

## `beforeRouteUpdate`

This guard is called when the **route changes** but the component is **reused** (i.e., the same component is reused, but route parameters change). This is useful for handling updates when the route parameters change.

### Example: Handling Route Parameter Changes

```javascript
export default {
  beforeRouteUpdate(to, from, next) {
    if (to.params.id !== from.params.id) {
      this.fetchData(to.params.id);  // Fetch new data based on the new ID
    }
    next(); // Proceed with navigation
  },
  methods: {
    fetchData(id) {
      // Fetch data based on the new `id`
    }
  }
};
```

*   
  **`beforeRouteUpdate`** is useful for fetching new data when route parameters change but the component is reused.

## `beforeRouteLeave`

This guard is called **before leaving the route**. It’s useful for things like cleanup or asking the user to confirm leaving the page (for example, if they have unsaved changes).

### Example: Prompting the User Before Leaving the Route

```javascript
export default {
  beforeRouteLeave(to, from, next) {
    const answer = window.confirm('Do you really want to leave? You have unsaved changes!');
    if (answer) {
      next(); // Proceed with navigation
    } else {
      next(false); // Cancel navigation
    }
  }
};
```

In this example:

* **`beforeRouteLeave`** asks the user for confirmation before navigating away if they have unsaved changes.

---

# Summary

* **Global Guards (`beforeEach`, `afterEach`)**: These are applied globally to all routes. Use `beforeEach` to intercept navigation globally, for example, for authentication checks.

* **Per-Route Guards (`beforeEnter`)**: These are applied to specific routes and allow you to perform checks before entering the route.

* **In-Component Guards (`beforeRouteEnter`, `beforeRouteUpdate`, `beforeRouteLeave`)**: These are defined within components and give you more control over route changes specific to that component.

These route guards are essential for controlling navigation, performing checks before or after navigation, and managing things like authentication, data fetching, or user interactions.

