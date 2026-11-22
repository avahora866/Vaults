---
tags:
  - Vue
---
The `<keep-alive>` wrapper in Vue is used to **cache** components, which means that when a component is removed from the DOM (e.g., when it’s no longer visible), Vue will **retain its state** and **DOM structure** in memory. This allows you to **reuse** the component when it’s shown again, instead of having to re-render the component from scratch.

This is particularly useful when you have components that don’t need to be re-rendered every time they are toggled, as it can lead to performance improvements by reducing unnecessary re-renders.

---

# How `<keep-alive>` Works

When a component is wrapped with `<keep-alive>`, Vue keeps a cached version of the component in memory, so it doesn't have to destroy and recreate the component when it is toggled. When the component is shown again, Vue simply "revives" the cached component, preserving its internal state.

# Basic Usage of `<keep-alive>`

Here’s an example of how to use `<keep-alive>`:

```vue
<template>
  <div>
    <button @click="currentView = 'home'">Home</button>
    <button @click="currentView = 'about'">About</button>

    <keep-alive>
      <component :is="currentView" />
    </keep-alive>
  </div>
</template>

<script>
import Home from './Home.vue'
import About from './About.vue'

export default {
  components: {
    Home,
    About
  },
  data() {
    return {
      currentView: 'home' // Controls which component to show
    }
  }
}
</script>
```

# Explanation:

* **`<keep-alive>`**: This wrapper caches the components inside it. When the `Home` or `About` component is removed from the DOM, Vue will not destroy them. Instead, it will retain the components in memory.

* **`:is="currentView"`**: The `component` element dynamically switches between the `Home` and `About` components based on the value of `currentView`.

# How It Helps:

When switching between views, the state of each component (like form inputs, scroll positions, or internal data) will persist because Vue keeps the components in memory.

---

# When to Use `<keep-alive>`

`<keep-alive>` is useful in scenarios where:

* **Switching Between Views or Components Frequently**: You want to avoid the overhead of re-rendering components every time they become active.

* **Maintaining Component State**: You want to preserve the state of a component (e.g., form data, scroll position, etc.) when switching between views.

* **Improving Performance**: If a component takes a long time to mount or initialize, caching it with `<keep-alive>` avoids unnecessary re-renders and improves performance.

# Example: Caching Form Data

Imagine you have a form where users fill in data, and you want to preserve the form data when switching between different sections:

```vue
<template>
  <div>
    <button @click="currentView = 'form'">Go to Form</button>
    <button @click="currentView = 'summary'">Go to Summary</button>

    <keep-alive>
      <component :is="currentView" />
    </keep-alive>
  </div>
</template>

<script>
import FormComponent from './FormComponent.vue'
import SummaryComponent from './SummaryComponent.vue'

export default {
  components: {
    FormComponent,
    SummaryComponent
  },
  data() {
    return {
      currentView: 'form' // Initially show the form
    }
  }
}
</script>
```

*   
  **FormComponent**: The form is shown initially. When switching to the summary view and then back to the form, the form's state (e.g., filled data) will be preserved because it’s cached by `<keep-alive>`.

---

# `keep-alive` Lifecycle Hooks

When using `<keep-alive>`, Vue provides two special lifecycle hooks that are only available to cached components:

1. **`activated`**: This hook is called when a cached component is inserted into the DOM.

2. **`deactivated`**: This hook is called when a cached component is removed from the DOM.

You can use these hooks to handle any specific actions when a component is reactivated or deactivated.

# Example of `activated` and `deactivated` Hooks

```vue
<template>
  <div>
    <keep-alive>
      <MyComponent />
    </keep-alive>
  </div>
</template>

<script>
export default {
  components: {
    MyComponent
  }
}
</script>

<script>
// Inside MyComponent.vue
export default {
  activated() {
    console.log('Component is now active!')
  },
  deactivated() {
    console.log('Component has been cached and removed from DOM.')
  }
}
</script>
```

# Explanation:

* **`activated()`**: This hook is called whenever the component is re-inserted into the DOM after being cached.

* **`deactivated()`**: This hook is triggered when the component is removed from the DOM and cached.

---

# Controlling Which Components to Cache with `include` and `exclude`

You can control which components are cached by `<keep-alive>` using the `include` and `exclude` props. These props allow you to specify which components should be cached or excluded from caching based on their names.

# `include` Prop

The `include` prop allows you to define which components should be cached based on a regular expression or a comma-separated string of component names.

```vue
<keep-alive :include="['Home', 'About']">
  <component :is="currentView" />
</keep-alive>
```

# `exclude` Prop

The `exclude` prop allows you to define which components should be excluded from caching based on a regular expression or a comma-separated string of component names.

```vue
<keep-alive :exclude="['Summary']">
  <component :is="currentView" />
</keep-alive>
```

# Combined Example with `include` and `exclude`

```vue
<keep-alive :include="['Home']" :exclude="['Summary']">
  <component :is="currentView" />
</keep-alive>
```

*   
  In this example, the `Home` component will be cached, but the `Summary` component will be excluded from caching.

---

# Performance Considerations

While `<keep-alive>` can provide performance benefits, it’s important to use it wisely:

* **Memory Usage**: Cached components are kept in memory, so excessive use of `<keep-alive>` can increase memory usage, especially with many large components.

* **Component Lifecycle**: Since `<keep-alive>` doesn’t destroy the components, you must be aware of the fact that data or events in those components may not be reset when the component is reactivated. You may need to manage cleanup manually in the `deactivated` lifecycle hook if necessary.

---

# Summary of `<keep-alive>`

* **Caching Components**: The `<keep-alive>` tag helps cache components and preserve their state when they are no longer active in the DOM.

* **`activated` and `deactivated` Lifecycle Hooks**: Use these hooks to handle component-specific behavior when a component is activated (shown) or deactivated (hidden).

* **`include` and `exclude` Props**: Control which components should be cached or excluded from caching using these props.

* **Improved Performance**: Caching can improve performance by reducing unnecessary re-renders, especially for components with expensive computations or animations.

* **Consider Memory Usage**: Be mindful of memory usage when caching multiple large components.

`<keep-alive>` is an essential tool for optimizing performance in Vue, especially when working with dynamic components that frequently enter and exit the view.

