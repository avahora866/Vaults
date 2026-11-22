---
tags:
  - Vue
---
Absolutely\! Vue’s **dynamic components** and **async components** are powerful tools that allow you to render components conditionally and load them only when needed — which can improve flexibility and performance in your app.

---

# Dynamic Components: `<component :is="compName" />`

Dynamic components allow you to switch between components *at runtime* using the built-in `<component>` element and the `:is` attribute.

## Basic Example

```vue
<template>
  <div>
    <button @click="currentTab = 'TabA'">Tab A</button>
    <button @click="currentTab = 'TabB'">Tab B</button>

    <component :is="currentTab" />
  </div>
</template>

<script>
import TabA from './TabA.vue';
import TabB from './TabB.vue';

export default {
  components: {
    TabA,
    TabB
  },
  data() {
    return {
      currentTab: 'TabA' // This will be used as the component name
    };
  }
};
</script>
```

## How It Works

* `<component :is="currentTab" />` will dynamically render `<TabA />` or `<TabB />` based on the `currentTab` value.

* The `:is` attribute accepts:

  * A **registered component name**

  * A **component object**

  * An **async component**

---

# Async Components

Async components are components that are **loaded only when needed** — useful for **code splitting** and optimizing performance.

## Simple Example

```javascript
const AsyncComponent = () => import('./MyHeavyComponent.vue');
```

This loads the component **lazily**, only when it’s rendered.

## Usage with Dynamic `<component :is>`:

```vue
<template>
  <component :is="currentView" />
</template>

<script>
export default {
  data() {
    return {
      currentView: () => import('./MyHeavyComponent.vue')
    };
  }
};
</script>
```

Or register it like this:

```javascript
export default {
  components: {
    AsyncView: () => import('./MyHeavyComponent.vue')
  },
  data() {
    return {
      currentView: 'AsyncView'
    };
  }
};
```

---

# When to Use Each

| Feature | Use Case |
| ----- | ----- |
| `:is` Dynamic Components | Switch between multiple components in the same template area |
| Async Components | Defer loading large components until they're needed |
| Combine Both | Dynamically switch between components that are **lazy-loaded** |

---

# Gotchas

* The component name used in `:is` must be **registered** or a **valid async function**.

* Use `keep-alive` if you want to cache dynamic components (like tabs):

```vue
<keep-alive>
  <component :is="currentTab" />
</keep-alive>
```

---

# Summary

* `<component :is="..."/>` lets you **dynamically render** a component based on a variable.

* You can pass:

  * A component name string

  * A component object

  * An **async import function**

* Async components improve performance by **lazy-loading** components.

