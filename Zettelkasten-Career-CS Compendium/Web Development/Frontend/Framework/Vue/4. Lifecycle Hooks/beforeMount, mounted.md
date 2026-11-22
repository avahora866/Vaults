---
tags:
  - Vue
---
These are called during the **mounting phase**, which is when Vue is about to insert your component into the DOM.

---

# `beforeMount`

* **When it runs**: After the template has been compiled, but **before the component is inserted into the DOM**.

* **What you can do**: Access reactive data and methods, but **DOM has not been rendered yet**.

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Hello Vue'
    };
  },
  beforeMount() {
    console.log('beforeMount:', document.getElementById('app').innerHTML);
  }
};
```

At this point, the DOM still contains the original template placeholder (`{{ message }}`), not the rendered result.

---

# `mounted`

* **When it runs**: After the component is **inserted into the DOM** and Vue has finished rendering.

* **What you can do**:

  * Access the DOM

  * Use `refs`

  * Start animations

  * Perform tasks that rely on the actual HTML being present

## Example:

```javascript
export default {
  data() {
    return {
      message: 'Hello Vue'
    };
  },
  mounted() {
    console.log('mounted:', document.getElementById('app').innerHTML);
  }
};
```

Now the DOM contains the rendered HTML, and you can safely interact with it.

---

# Comparison

| Hook | DOM Rendered? | Reactive Data Available? | Use Cases |
| ----- | ----- | ----- | ----- |
| `beforeMount` | ❌ No | ✅ Yes | Last chance before DOM renders |
| `mounted` | ✅ Yes | ✅ Yes | Accessing DOM, refs, API calls that need DOM |

---

# When to Use

* Use **`beforeMount`** if you need to inspect or prepare before Vue renders to the DOM.

* Use **`mounted`** when:

  * You need access to DOM elements

  * You're initializing third-party libraries

  * You're starting timers, listeners, or fetching data that depends on the DOM

