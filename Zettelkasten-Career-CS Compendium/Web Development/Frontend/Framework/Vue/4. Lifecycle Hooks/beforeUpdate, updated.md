---
tags:
  - Vue
---
These hooks are part of the **update phase**, which occurs whenever **reactive data** or **props** change. They allow you to hook into the process right before and after the DOM gets updated in response to data changes.

---

# `beforeUpdate`

* **When it runs**: This hook is called **before** the DOM is updated due to reactive data changes.

* **What you can do**:

  * You can still access the **old DOM** state, but the **DOM updates haven’t been applied yet**.

  * You can make changes to **data** before the update is actually applied to the DOM.

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Hello Vue'
    };
  },
  beforeUpdate() {
    console.log('beforeUpdate: The message is about to change');
  },
  methods: {
    updateMessage() {
      this.message = 'Updated message';
    }
  }
};
```

In this example:

* When you call `updateMessage()`, the `message` will change, but **before** Vue applies the change to the DOM, the `beforeUpdate` hook is triggered.

## Use Cases:

* **Optimizations**: If you want to do something before Vue re-renders the DOM, like adjusting values or making calculations.

* **Snapshot**: If you need to compare the current state to the updated state and want to act before DOM changes.

---

# `updated`

* **When it runs**: This hook is called **after** Vue updates the DOM in response to data changes.

* **What you can do**: You can now access the **updated DOM** and perform actions that rely on the new DOM state.

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Hello Vue'
    };
  },
  updated() {
    console.log('updated: The DOM has been updated');
  },
  methods: {
    updateMessage() {
      this.message = 'Updated message';
    }
  }
};
```

Here:

* After `message` changes and Vue has re-rendered the DOM, the `updated` hook is triggered.

## Use Cases:

* **DOM manipulation**: If you need to interact with the updated DOM, like applying a third-party library that works with the DOM.

* **Animations or transitions**: Trigger animations or transitions after data-driven DOM changes.

* **Post-update calculations**: React to changes in the DOM after Vue has updated it (e.g., recalculating layouts or positions).

---

# Comparison

| Hook | When it runs | DOM Updated? | Common Use Cases |
| ----- | ----- | ----- | ----- |
| `beforeUpdate` | Before DOM is updated | ❌ Not yet updated | Pre-update calculations, optimizations |
| `updated` | After DOM has been updated | ✅ Yes | Post-update DOM access, animations, or external libraries |

---

# Recap

* **`beforeUpdate`**: Called before the DOM is updated. You can still access the old DOM and make changes to data. It’s useful for pre-update optimizations.

* **`updated`**: Called after the DOM has been updated. You can interact with the updated DOM and perform post-update actions.

---

