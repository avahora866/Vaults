---
tags:
  - Vue
---
**`beforeUnmount`** and **`unmounted`**. These hooks are part of the **unmounting phase** of the lifecycle and are called when a component is about to be destroyed or removed from the DOM.

---

# `beforeUnmount`

* **When it runs**: This hook is called **just before** the component is removed from the DOM.

* **What you can do**:

  * It's the last chance to **clean up** or **perform final actions** before Vue destroys the component.

  * You can stop timers, remove event listeners, or cancel any ongoing requests.

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Goodbye Vue!'
    };
  },
  beforeUnmount() {
    console.log('beforeUnmount: The component is about to be destroyed');
    // You can clean up timers, remove listeners, or cancel async requests
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => {
        console.log('Timer is running');
      }, 1000);
    }
  },
  mounted() {
    this.startTimer();
  }
};
```

In this example:

* `beforeUnmount` will be called before the component is actually destroyed. It's a great place to stop any ongoing actions, like cleaning up timers or removing event listeners.

---

# `unmounted`

* **When it runs**: This hook is called **after** the component has been completely removed from the DOM.

* **What you can do**:

  * You can do any final **cleanup** work that needs to happen after the DOM is removed.

  * This is generally used to clean up resources that might still be held by the component (e.g., external libraries, subscriptions).

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Goodbye Vue!'
    };
  },
  unmounted() {
    console.log('unmounted: The component has been destroyed');
    // Clean up any resources like removing event listeners, or canceling ongoing subscriptions
    clearInterval(this.timer);
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => {
        console.log('Timer is running');
      }, 1000);
    }
  },
  mounted() {
    this.startTimer();
  }
};
```

In this example:

* After the component is removed from the DOM, `unmounted` is triggered. Here, you would stop ongoing actions or clean up resources like timers, event listeners, or external libraries.

---

# Comparison

| Hook | When it runs | DOM Updated? | Common Use Cases |
| ----- | ----- | ----- | ----- |
| `beforeUnmount` | Before the component is destroyed | ❌ Not in DOM anymore | Cleanup logic (e.g., stop timers, remove listeners) |
| `unmounted` | After the component is destroyed | ✅ Removed from DOM | Final cleanup (e.g., clearing resources, external libraries) |

---

# Recap

* **`beforeUnmount`**: Runs right before the component is removed from the DOM. It's the last chance to clean up or perform final actions (e.g., stop timers, remove event listeners).

* **`unmounted`**: Runs after the component is completely removed from the DOM. It’s for final cleanup like clearing resources or canceling subscriptions.

---

These hooks are essential for ensuring that your components don’t leave **memory leaks** or **unnecessary background processes** running after they are no longer needed.

