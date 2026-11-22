---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
These are the building blocks of the **Vuex store** and help in managing your application's state in a centralized and predictable manner.

---

# State

In Vuex, **state** is where you store the **data** or **application state** that needs to be shared across multiple components. The **state** object holds the data that represents the application's current status.

The **state** in Vuex is **reactive**, meaning that when the state changes, the Vue components using that state will automatically re-render to reflect the new data.

## Example of State:

```javascript
// store.js
import { createStore } from 'vuex';

const store = createStore({
  state: {
    count: 0,          // A simple counter
    user: {            // User information
      name: 'John Doe',
      email: 'john@example.com'
    }
  }
});

export default store;
```

*   
  In this example, the **state** contains:

  * `count`: A simple integer value.

  * `user`: An object that contains information about the user.

State is accessed via the store, and components can get the data from the store using **`$store.state`**.

---

# Getters

**Getters** are like **computed properties** for the Vuex store. They allow you to **derive** values from the state. Getters are used to calculate or transform state data before it is returned, which is useful when you need to return modified or formatted state.

* **Getters** are **cached**, meaning they will only re-evaluate when the state they depend on changes.

## Example of Getters:

```javascript
const store = createStore({
  state: {
    count: 10,
    user: {
      name: 'John Doe',
      email: 'john@example.com'
    }
  },
  getters: {
    // Getter to return the doubled count
    doubledCount(state) {
      return state.count * 2;
    },
    // Getter to return a user's full name
    fullName(state) {
      return `${state.user.name} (${state.user.email})`;
    }
  }
});
```

*   
  **Accessing getters**:

  * **In a component**: You can access getters using `this.$store.getters`.

  * Example: `this.$store.getters.doubledCount` will give you the doubled value of `count`.

---

# Mutations

**Mutations** are **synchronous** functions that **modify** the state in Vuex. Mutations are the **only way** to change the state, and they are **centralized** so you can track when and how the state changes.

In Vuex, state modifications must be done through **mutations** for **predictability** and **debugging** purposes. This ensures that every state change is explicit and traceable, and can be easily logged or debugged.

* You **commit** a mutation to trigger it.

## Example of Mutations:

```javascript
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;   // Increases the count by 1
    },
    setCount(state, payload) {
      state.count = payload;  // Sets count to a specific value passed in the payload
    }
  }
});
```

*   
  **Committing mutations**: You commit a mutation in your components using `this.$store.commit()`.

Example of committing mutations:

```javascript
methods: {
  increment() {
    this.$store.commit('increment');
  },
  setCount(value) {
    this.$store.commit('setCount', value);
  }
}
```

In this example:

* `increment` is a mutation that increases the `count` by 1\.

* `setCount` is a mutation that sets the `count` to a specified value.

---

# Actions

**Actions** are similar to mutations, but they are **asynchronous**. They are used for handling **side effects** like making API calls, fetching data, or performing asynchronous tasks. After performing the asynchronous task, actions usually **commit mutations** to update the state.

Actions are triggered by **dispatching** them, unlike mutations, which are triggered by **committing**.

## Example of Actions:

```javascript
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    setCount(state, payload) {
      state.count = payload;
    }
  },
  actions: {
    async fetchCountFromAPI({ commit }) {
      const response = await fetch('https://api.example.com/count');
      const data = await response.json();
      commit('setCount', data.count);  // Commit the mutation to update the state
    }
  }
});
```

*   
  In this example, the action `fetchCountFromAPI` makes an asynchronous API request to fetch a `count` value and then commits the `setCount` mutation to update the state.

## Dispatching Actions:

In your components, you dispatch actions using `this.$store.dispatch()`:

```javascript
methods: {
  async fetchCount() {
    await this.$store.dispatch('fetchCountFromAPI');
  }
}
```

---

# Summary of Vuex Concepts

1. **State**:

   * Centralized data storage for the application.

   * Holds the current state of the app, which can be accessed by components.

2. **Getters**:

   * Used to access and transform state in a reactive manner.

   * Computed-like properties that depend on the state and return derived values.

3. **Mutations**:

   * Synchronous functions that directly modify the state.

   * The only way to change the state in Vuex.

   * Mutations are committed using `this.$store.commit()`.

4. **Actions**:

   * Asynchronous functions that can perform tasks like API calls.

   * Actions can dispatch mutations to modify the state.

   * Actions are dispatched using `this.$store.dispatch()`.

---

# Example of Putting It All Together

Here’s a simple example showing how Vuex concepts work together:

```javascript
import { createStore } from 'vuex';

const store = createStore({
  state: {
    count: 0
  },
  getters: {
    doubledCount: (state) => state.count * 2
  },
  mutations: {
    increment(state) {
      state.count++;
    },
    setCount(state, payload) {
      state.count = payload;
    }
  },
  actions: {
    async fetchCountFromAPI({ commit }) {
      const response = await fetch('https://api.example.com/count');
      const data = await response.json();
      commit('setCount', data.count);
    }
  }
});

export default store;
```

# In the Component:

```javascript
export default {
  computed: {
    doubledCount() {
      return this.$store.getters.doubledCount;  // Accessing getter
    }
  },
  methods: {
    incrementCount() {
      this.$store.commit('increment');  // Committing mutation
    },
    async fetchData() {
      await this.$store.dispatch('fetchCountFromAPI');  // Dispatching action
    }
  }
};
```

This example shows how:

* The component accesses state through getters (`doubledCount`).

* The component commits a mutation to modify state (`increment`).

* The component dispatches an action to fetch data and then update the state (`fetchCountFromAPI`).

