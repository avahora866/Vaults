---
tags:
  - Vue
---
**Pinia** is a state management library designed for **Vue.js**, and it is considered the **official replacement for Vuex** in Vue 3\. While **Vuex** is a powerful, well-established state management solution, **Pinia** is much **simpler**, **lighter**, and **more intuitive** to use, especially with Vue 3's Composition API.

Pinia follows the same principles as Vuex (centralized state management) but makes use of modern JavaScript features like **Composition API**, **ES Modules**, and **TypeScript** support, providing a more streamlined experience.

---

# Key Features of Pinia

* **Simpler API**: The API in Pinia is easier to understand and use compared to Vuex.

* **Composition API Support**: It works seamlessly with Vue 3’s Composition API, making it easier to integrate with the rest of the Vue ecosystem.

* **Modular**: Like Vuex, Pinia allows you to structure your store in a modular way.

* **Built for Vue 3**: Pinia takes full advantage of Vue 3's features, including **reactivity** and **TypeScript** integration.

---

# Basic Concepts of Pinia

1. **Store**: The central object that holds the state of your application.

2. **State**: The data that you want to manage and share between components.

3. **Getters**: Computed properties for the store that can be used to transform or compute values from the state.

4. **Actions**: Methods that can modify the store's state, often used for asynchronous operations.

---

# Installing Pinia

To use Pinia in your Vue 3 project, you'll need to install it first:

```
npm install pinia
```

Once installed, you need to set it up in your Vue app:

```javascript
// main.js (or main.ts)
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia'

const app = createApp(App)

app.use(createPinia())  // Add Pinia to the Vue app

app.mount('#app')
```

---

# Creating a Store with Pinia

In Pinia, a store is created using the `defineStore` function. This is the central object where you define your state, getters, and actions.

## Basic Example of a Store

```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  getters: {
    doubledCount: (state) => state.count * 2
  },
  actions: {
    increment() {
      this.count++  // Mutating the state directly
    },
    async fetchCountFromAPI() {
      const response = await fetch('https://api.example.com/count')
      const data = await response.json()
      this.count = data.count  // Directly modifying state
    }
  }
})
```

In this example:

* **`state`** holds the data (`count`).

* **`getters`** compute derived data (`doubledCount`).

* **`actions`** define methods that modify the state (e.g., `increment` and `fetchCountFromAPI`).

---

# Using the Store in Vue Components

In a Vue component, you can **use the store** via the `useStore` hook to access state, getters, and actions.

```vue
// Inside a Vue component
<template>
  <div>
    <p>Count: {{ counter.count }}</p>
    <p>Doubled Count: {{ counter.doubledCount }}</p>
    <button @click="incrementCount">Increment</button>
    <button @click="fetchCount">Fetch Count</button>
  </div>
</template>

<script setup>
import { useCounterStore } from './stores/counter'

// Access the store
const counter = useCounterStore()

// Call actions from the store
const incrementCount = () => {
  counter.increment()
}

const fetchCount = async () => {
  await counter.fetchCountFromAPI()
}
</script>
```

*   
  **`useCounterStore()`**: This is how you access your store inside the component.

* **`counter.count`**: Accessing the state.

* **`counter.doubledCount`**: Using a getter to derive data.

* **`counter.increment()`**: Calling an action to modify the state.

* **`counter.fetchCountFromAPI()`**: Dispatching an action that fetches data asynchronously and updates the state.

---

# Comparison of Pinia vs Vuex

| Feature | Pinia | Vuex |
| ----- | ----- | ----- |
| **Setup** | Simpler, uses Composition API | More verbose, uses Options API |
| **State** | Directly accessible inside the store | Requires mutations to update state |
| **Getters** | Computed properties directly inside store | Defined separately, uses getters |
| **Actions** | Can mutate state directly in actions | Actions commit mutations |
| **TypeScript Support** | Full, native TypeScript support | TypeScript integration via plugins |
| **Module Support** | Built-in support for modular stores | Modules available but more complex |
| **API Complexity** | Simple and intuitive | More complex and boilerplate-heavy |

---

# State Management Flow with Pinia

1. **State** holds your application's data.

2. **Getters** allow you to derive and compute values from that state.

3. **Actions** allow you to perform operations (like asynchronous API calls) and modify the state.

In Pinia, the state can be **mutated directly** within actions. There is no need to explicitly commit mutations as you would in Vuex, making the code simpler.

---

# Advanced Features in Pinia

* **Persisting State**: Pinia supports plugins like `pinia-plugin-persist` to persist the state across page reloads.

* **Modules**: Just like Vuex, Pinia supports splitting your store into multiple modules.

* **TypeScript**: Pinia has built-in TypeScript support, making it a great choice for TypeScript users.

---

# Example of Using Multiple Stores

You can define multiple stores using **`defineStore`** and organize them in separate files.

```javascript
// stores/user.js
import { defineStore } from 'pinia'

export const useUserStore = defineStore('user', {
  state: () => ({
    name: 'John Doe',
    email: 'john@example.com'
  }),
  actions: {
    setUser(user) {
      this.name = user.name
      this.email = user.email
    }
  }
})
```

```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++
    }
  }
})
```

In your Vue component, you can use **both stores**:

```vue
<template>
  <div>
    <p>{{ user.name }} - {{ user.email }}</p>
    <p>Count: {{ counter.count }}</p>
    <button @click="incrementCount">Increment</button>
  </div>
</template>

<script setup>
import { useUserStore } from './stores/user'
import { useCounterStore } from './stores/counter'

const user = useUserStore()
const counter = useCounterStore()

const incrementCount = () => {
  counter.increment()
}
</script>
```

In this setup:

* `useUserStore()` provides access to the user data.

* `useCounterStore()` provides access to the counter data.

* Both stores are used independently in the same component.

---

# Summary

Pinia provides a simpler and more modern approach to state management for Vue 3 applications. It is designed to work seamlessly with Vue’s Composition API, offers easier API usage, and provides full TypeScript support. Unlike Vuex, Pinia allows for **direct mutations** in actions and a much more **intuitive syntax** overall.

Pinia is a great choice for **smaller to medium-sized applications**, and if you’re already using Vue 3, it’s highly recommended for state management.

Let me know if you'd like a deeper dive into any specific feature of Pinia\!

