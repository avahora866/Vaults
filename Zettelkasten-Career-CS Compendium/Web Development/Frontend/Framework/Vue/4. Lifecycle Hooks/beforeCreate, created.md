---
tags:
  - Vue
---
These are part of the **Options API**.

---

# `beforeCreate`

* **When it runs**: Immediately after the component instance is initialized, **before** Vue sets up:

  * `data`

  * `props`

  * `computed`

  * `methods`

  * `watchers`

* **What you can do**: Very little — reactive state isn't available yet.

## Example:

```javascript
export default {
  beforeCreate() {
    console.log('beforeCreate:', this.message); // undefined
  },
  data() {
    return {
      message: 'Hello Vue'
    };
  }
};
```

## Use cases:

* Rarely used

* You might use it for debugging or low-level plugin setup

---

# `created`

* **When it runs**: After Vue sets up:

  * `data`, `props`, `methods`, `computed`, etc.

* **What you can do**: You can access `this`, use reactive state, call methods, and make API calls.

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Hello Vue'
    };
  },
  created() {
    console.log('created:', this.message); // 'Hello Vue'
  }
};
```

## Use cases:

* Initializing values

* Fetching data from an API

* Setting up watchers

* Starting timers

---

# Comparison

| Hook | Reactive Data Available? | Common Use |
| ----- | ----- | ----- |
| `beforeCreate` | ❌ No | Rarely needed |
| `created` | ✅ Yes | Setup logic, API calls |

---

# Summary

* `beforeCreate`: Very early, no access to data/methods yet.

* `created`: Good for setup work, you can access everything except the DOM.

