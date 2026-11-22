---
tags:
  - Vue
---
The **Vue Lifecycle** refers to the sequence of stages a Vue component goes through — from the moment it's created until it's destroyed. Vue gives you **lifecycle hooks**, which are special functions you can use to **run your own code at specific points** during that process.

---

# Why Does the Lifecycle Matter?

Understanding the lifecycle helps you:

* Know **when** to fetch data, access DOM elements, or set up timers

* Handle **setup and cleanup** tasks efficiently

* Avoid bugs by using the right hook for the right job

---

# The Vue Component Lifecycle — Overview

Here’s a simplified version of the lifecycle phases:

1. **Creation Phase**

   * `beforeCreate`

   * `created`

2. **Mounting Phase**

   * `beforeMount`

   * `mounted`

3. **Update Phase** (when reactive data changes)

   * `beforeUpdate`

   * `updated`

4. **Unmounting Phase**

   * `beforeUnmount`

   * `unmounted`

---

# Breakdown of Each Phase

## Creation Phase

* **`beforeCreate`**:  
   Runs before Vue sets up data, props, methods, etc.  
   You don’t have access to `this.data` or `this.props` yet.

* **`created`**:  
   Data and props are now available. Good place for:

  * API calls

  * Initializing variables

  * Setting up watchers or event listeners

---

## Mounting Phase

* **`beforeMount`**:  
   Called right before the component is added to the DOM.

* **`mounted`**:  
   Called after the component is added to the DOM.  
   Perfect for:

  * Accessing DOM elements

  * Using `refs`

  * Starting animations

  * Making requests that depend on the DOM

---

## Update Phase

Triggered when **reactive data or props change**.

* **`beforeUpdate`**:  
   Runs before the DOM is patched.

* **`updated`**:  
   Runs after DOM has been updated.

---

## Unmounting Phase

When a component is being removed from the DOM.

* **`beforeUnmount`**:  
   Great for cleanup:

  * Clearing intervals

  * Removing event listeners

* **`unmounted`**:  
   Called after the component is removed.

---

# Visual Flow (Simplified)

```
beforeCreate ➝ created ➝ beforeMount ➝ mounted
      ⬇
 (updates via reactive data)
      ⬇
beforeUpdate ➝ updated
      ⬇
 (component removed)
      ⬇
beforeUnmount ➝ unmounted
```

---

# Where Do You Use These?

In the **Options API** (Vue 2 or Vue 3):

```javascript
export default {
  data() { return { count: 0 }; },
  created() {
    console.log('Component created');
  },
  mounted() {
    console.log('Component mounted');
  }
}
```

In the **Composition API** (Vue 3):

```javascript
import { onMounted, onBeforeUnmount } from 'vue';

onMounted(() => {
  console.log('Mounted!');
});

onBeforeUnmount(() => {
  console.log('Cleaning up!');
});
```

---

# Summary

| Hook | When it runs | Common use cases |
| ----- | ----- | ----- |
| `beforeCreate` | Before anything is set up | Rarely used |
| `created` | After data/props/methods are available | API calls, setup |
| `beforeMount` | Before DOM insertion | Prep before render |
| `mounted` | After DOM is rendered | DOM access, animations, refs |
| `beforeUpdate` | Before DOM updates due to data change | Snapshot DOM state |
| `updated` | After DOM has updated | React to post-DOM change |
| `beforeUnmount` | Before the component is destroyed | Cleanup logic |
| `unmounted` | After the component is removed | Final cleanup |


