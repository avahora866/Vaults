---
tags:
  - Vue
---
# Reactivity

* **What it is**: Vue's reactivity system allows your data and the view (UI) to stay in sync automatically. When you change the data in your application, the view is updated in real-time, without needing to manually manipulate the DOM.

* **How it works**: Vue observes changes in data and automatically updates the DOM when necessary. This is done using **reactive objects**, where Vue tracks dependencies (e.g., data properties) and efficiently re-renders the necessary parts of the UI. The reactivity system is based on getters and setters, making it very efficient.

**Example**:

```javascript
data() {
  return {
    message: 'Hello, Vue!'
  };
}
```

If `message` changes, the DOM that is bound to `message` will automatically update.

# Directives

* **What they are**: Directives are special tokens in Vue’s templates that tell the framework how to render or manipulate the DOM. They are prefixed with `v-` to distinguish them as special attributes.

* **Common directives**:

  * `v-bind`: Dynamically binds attributes or classes to elements.

  * `v-for`: Loops through an array or object and renders a template for each item.

  * `v-if`, `v-else`, `v-show`: Conditionals for rendering elements based on expressions.

  * `v-model`: Two-way binding of form inputs and data.

**Example**:

```vue
<div v-if="isVisible">This content is conditionally rendered</div>
<button v-on:click="toggleVisibility">Toggle</button>
```

# Components

* **What they are**: Components are reusable, self-contained units of code that define a part of the user interface. Vue uses a **component-based architecture**, meaning you break your application into components that manage their own data, logic, and view.

* **Features**:

  * Components can have their own state, methods, and lifecycle hooks.

  * They can be nested, allowing for complex UIs to be built from smaller, manageable pieces.

  * Components can communicate with each other using **props** (from parent to child) and **events** (from child to parent).

**Example**:

```javascript
Vue.component('my-component', {
  template: '<p>Hello, {{ name }}</p>',
  data() {
    return { name: 'Vue' };
  }
});
```

# Vue Router

* **What it is**: Vue Router is the official routing library for Vue.js that enables navigation between different views or pages in single-page applications (SPA). It allows you to map URLs to specific components or views and control the navigation of your application.

* **Key features**:

  * Dynamic routing: You can change routes dynamically based on user input or application state.

  * Nested routes: You can define routes within routes to create complex layouts.

  * Route guards: Handle logic like authentication checks before navigating to a route.

   History mode: Avoid hash (`#`) in the URL for cleaner, more natural-looking URLs.

**Example**:

```javascript
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About }
];
const router = new VueRouter({ routes });
```

# Vuex / Pinia (State Management)

* **Vuex**: Vuex is the state management library for Vue.js. It helps manage the state of your application in a centralized store, making it easier to share data across components, especially in large-scale apps.

  * **State**: Centralized data that all components can access.

  * **Getters**: Derived state based on the store’s state.

  * **Mutations**: Methods that change the state, following Vue's reactivity system.

  * **Actions**: Methods for performing asynchronous operations, like API calls.

* **Example (Vuex)**:

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    }
  }
});
```

*   
  **Pinia**: Pinia is the new state management solution for Vue 3, designed as the successor to Vuex. It offers a simpler and more modular approach to managing state with better TypeScript support. Unlike Vuex, Pinia is designed to be lightweight and reactive by default, making it a more flexible option for modern Vue apps.

   **Example (Pinia)**:

```javascript
import { defineStore } from 'pinia';
const useStore = defineStore('main', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});
```

---

# Summary of Key Features:

* **Reactivity**: Automatically updates the UI when data changes.

* **Directives**: Special syntax in templates that provide functionality like looping, conditional rendering, and binding data.

* **Components**: Reusable, self-contained units that manage their own state, methods, and templates.

* **Vue Router**: Manages navigation between views in SPAs.

* **Vuex/Pinia**: Centralized state management for sharing data across components in complex applications.

These features are what make Vue.js a powerful and flexible framework for building modern, dynamic web applications.

