---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
# What is Vuex?

**Vuex** is a state management library specifically designed for **Vue.js** applications. It helps you manage the application's state (data) in a **centralized store**, allowing you to access and update state across components in a predictable and organized manner. This is especially useful for larger applications where multiple components need to share and modify state.

---

# Core Concepts of Vuex

There are several core concepts in Vuex that work together to provide a predictable flow for managing application state. These include:

1. **State**

2. **Getters**

3. **Mutations**

4. **Actions**

5. **Modules**

---

# State: The Source of Truth

The **state** in Vuex is a **single object** that contains all the data for your application. All components access the state from the **store** rather than directly managing their own state. This makes it easier to share data between components and maintain consistency.

## Example of State

```javascript
// store.js
import { createStore } from 'vuex';

const store = createStore({
  state: {
    count: 0,
    user: {
      name: '',
      email: ''
    }
  }
});

export default store;
```

In the above example, the state contains two properties:

* `count`: A counter variable.

* `user`: An object with the user’s `name` and `email`.

Components can access this state to display or manipulate the data.

---

# Getters: State Derived Computed Properties

**Getters** are like computed properties for the Vuex store. They allow you to **derive** state values or **transform** the data before it's used in a component. Getters are cached, meaning they will only recalculate when their dependencies (state values) change.

## Example of Getters

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
    // Getter to get the count doubled
    doubledCount(state) {
      return state.count * 2;
    },
    // Getter to get the user's full name
    fullName(state) {
      return `${state.user.name} (${state.user.email})`;
    }
  }
});
```

You can use the getter in your components as:

```javascript
computed: {
  doubledCount() {
    return this.$store.getters.doubledCount;
  },
  fullName() {
    return this.$store.getters.fullName;
  }
}
```

---

# Mutations: Synchronous State Modifications

**Mutations** are the only way to change the state in Vuex. They are **synchronous** functions that directly modify the state. Mutations are triggered by **committing** them, and they ensure that the state changes are **traceable** and **predictable**.

## Example of Mutations

```javascript
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    },
    setCount(state, payload) {
      state.count = payload;
    }
  }
});
```

In your components, you can commit mutations to update the state:

```javascript
methods: {
  incrementCount() {
    this.$store.commit('increment');
  },
  setCountToValue(value) {
    this.$store.commit('setCount', value);
  }
}
```

---

# Actions: Asynchronous Operations

**Actions** are similar to mutations but can be **asynchronous**. They allow you to handle **side-effects**, like making API calls, and then **commit mutations** to modify the state after the operation is complete. Actions are triggered by **dispatching** them.

## Example of Actions

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
      const response = await fetch('https://api.example.com/getCount');
      const data = await response.json();
      commit('setCount', data.count);
    }
  }
});
```

In your components, you can dispatch actions:

```javascript
methods: {
  fetchCount() {
    this.$store.dispatch('fetchCountFromAPI');
  }
}
```

---

# Modules: Organizing State

For larger applications, you can split the store into **modules**, each with its own state, mutations, actions, and getters. This helps keep the store **modular** and **maintainable** as your app grows.

## Example of Modules

```javascript
const store = createStore({
  modules: {
    counter: {
      state: () => ({ count: 0 }),
      mutations: {
        increment(state) {
          state.count++;
        }
      },
      actions: {
        async fetchCountFromAPI({ commit }) {
          const response = await fetch('https://api.example.com/getCount');
          const data = await response.json();
          commit('increment');
        }
      },
      getters: {
        doubledCount(state) {
          return state.count * 2;
        }
      }
    },
    user: {
      state: () => ({
        name: 'John Doe',
        email: 'john@example.com'
      }),
      mutations: {
        setUser(state, user) {
          state.name = user.name;
          state.email = user.email;
        }
      }
    }
  }
});
```

In this example:

* The store is divided into two modules: `counter` and `user`.

* Each module has its own state, mutations, actions, and getters.

* You can access the state of a module with the module name, like `this.$store.state.counter.count` or `this.$store.getters['counter/doubledCount']`.

---

# Accessing Vuex State in Vue Components

In Vue components, you can access the Vuex state, getters, commit mutations, and dispatch actions.

## Accessing State

You can use **`$store.state`** to access the state directly.

```javascript
computed: {
  count() {
    return this.$store.state.counter.count;
  }
}
```

## Accessing Getters

You can use **`$store.getters`** to access the getters.

```javascript
computed: {
  doubledCount() {
    return this.$store.getters['counter/doubledCount'];
  }
}
```

## Committing Mutations

You can use **`$store.commit`** to commit mutations and change the state.

```javascript
methods: {
  incrementCount() {
    this.$store.commit('counter/increment');
  }
}
```

## Dispatching Actions

You can use **`$store.dispatch`** to dispatch actions, especially when performing asynchronous operations.

```javascript
methods: {
  fetchData() {
    this.$store.dispatch('counter/fetchCountFromAPI');
  }
}
```

---

# Summary

* **State**: The central data store, accessible throughout the app.

* **Getters**: Derived state, similar to computed properties.

* **Mutations**: Synchronous functions that directly change the state.

* **Actions**: Asynchronous operations that can dispatch mutations after completing a task (like an API call).

* **Modules**: Organize state, mutations, actions, and getters into separate modules for larger applications.

Vuex helps maintain consistency in your app by ensuring all components share a single source of truth and manage the state in a structured and predictable manner. It’s especially useful in large-scale applications where state needs to be shared across many components.

