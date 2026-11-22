---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
The **`provide`** and **`inject`** APIs in Vue.js are used to share state or values across components without having to pass them through every level of the component tree as props. This is particularly useful for **global state sharing** or for passing down values to deeply nested components.

* **`provide`** is used in a parent component to make a value available to all its descendants.

* **`inject`** is used in a child component to access the value provided by its ancestor.

These APIs are part of Vue's **Composition API** and are a way to create **dependency injection** for your components.

---

# How It Works

1. **Provide**: The parent component defines a value or state using the `provide` option. This state can be anything: a simple value, a reactive object, or even a function.

2. **Inject**: The child component retrieves that value using the `inject` option. Vue will look up the ancestor chain to find the provided value.

---

# Basic Example of `provide` and `inject`

## Providing State in a Parent Component

In the **parent** component, you use `provide` to make a value available to all child components:

```vue
<template>
  <div>
    <ChildComponent />
  </div>
</template>

<script setup>
import { provide, reactive } from 'vue'
import ChildComponent from './ChildComponent.vue'

// A reactive object to share the state
const state = reactive({
  count: 0
})

// Providing the state to all child components
provide('globalState', state)
</script>
```

*   
  Here, the `parent` component is **providing** the `state` object, which contains a `count` property, using the `provide` API.

* The `state` is **reactive**, so any changes made to it in the child components will automatically reflect in the parent and all other descendants.

## Injecting the Provided Value in a Child Component

In a **child** component, you use `inject` to access the provided state:

```vue
<template>
  <div>
    <p>Count from parent: {{ globalState.count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
import { inject } from 'vue'

// Injecting the provided value from the parent component
const globalState = inject('globalState')

// Method to increment the count
const increment = () => {
  globalState.count++
}
</script>
```

*   
  Here, the child component **injects** the `globalState` object provided by the parent.

* It can access `globalState.count` and modify it (like incrementing the count). Since the state is **reactive**, any changes made will be reflected in the parent and other components that use it.

---

# Important Points to Remember

1. **Reactivity**:

   * When you use `reactive` or `ref` for the value in `provide`, the injected value will remain reactive. That means changes in the provided state will automatically update any component that injects it.

2. **Scope of `provide`**:

   * The provided value is **available to all descendants** of the component where `provide` is used. This means that even if a component is deeply nested, it can access the value provided by its ancestor.

3. **Use Cases**:

   * `provide`/`inject` is often used for **global state management**, like theme settings, localization, or any data that needs to be shared across many components without the need to pass it through props.

   * This is **not a replacement for Vuex** (or Pinia), but for scenarios where you want to share state without involving a full state management solution.

4. **No Default Injection**:

   * Unlike props, **injected values do not have a default**. If you try to inject a value that hasn't been provided, it will return `undefined`. You can add a fallback value if needed.

```javascript
const globalState = inject('globalState', { count: 0 }) // Default value if not provided
```

---

# Advanced Example with a Function

You can also provide **functions** that modify the state or trigger certain behaviors. This is useful for scenarios like handling global events or actions.

## Parent Component (Providing a Function)

```vue
<template>
  <div>
    <ChildComponent />
  </div>
</template>

<script setup>
import { provide } from 'vue'
import ChildComponent from './ChildComponent.vue'

// Provide a function that increments the count
const incrementCount = () => {
  state.count++
}

// A reactive state object
const state = reactive({
  count: 0
})

// Providing both the state and the increment function
provide('globalState', state)
provide('increment', incrementCount)
</script>
```

## Child Component (Injecting and Using the Function)

```vue
<template>
  <div>
    <p>Current Count: {{ globalState.count }}</p>
    <button @click="increment">Increment Count</button>
  </div>
</template>

<script setup>
import { inject } from 'vue'

// Injecting the provided state and the increment function
const globalState = inject('globalState')
const increment = inject('increment')
</script>
```

*   
  In this case, the child component **injects** both the `globalState` and the `increment` function.

* The child component can now call the `increment` function to update the `count` value in the state.

---

# When to Use `provide`/`inject`

* **Global State Sharing**: You want to share data or state across many components without passing props through each layer.

* **Avoid Prop Drilling**: If you need to pass data deeply down the component tree and don't want to manually pass props through every intermediate level.

* **Shared Behavior**: When you want to share certain behaviors (like a function) across components.

**Note**: While `provide`/`inject` is convenient for simple use cases, it’s not ideal for more complex state management needs, like large applications that require **centralized state management**. In such cases, you may want to use **Pinia** or **Vuex**.

---

# Summary

* **`provide`**: Used in a parent component to make a value available to its descendants.

* **`inject`**: Used in a child component to access a provided value.

* Both APIs are useful for **global state sharing** or **passing data** without props.

* It's a good solution for simpler state-sharing needs but not a replacement for more complex state management tools like **Pinia** or **Vuex**.

Let me know if you'd like further clarification or examples\!

