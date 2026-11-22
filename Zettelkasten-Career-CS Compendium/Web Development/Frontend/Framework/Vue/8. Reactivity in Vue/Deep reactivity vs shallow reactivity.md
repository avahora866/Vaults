---
tags:
  - Vue
---
# What is Reactivity?

Reactivity in Vue means Vue "tracks" changes to data so it can update the DOM when something changes. But when you’re working with **nested objects**, it's important to understand how **deeply** Vue tracks those changes.

---

# Deep Reactivity

Vue’s **`reactive()`** and **`ref()`** (with objects) are **deeply reactive** by default. That means Vue tracks **every nested property** inside the object.

## Example:

```vue
<script setup>
import { reactive } from 'vue';

const user = reactive({
  name: 'Alice',
  address: {
    city: 'Paris',
    zip: 75000
  }
});

function updateCity() {
  user.address.city = 'Lyon';  // Vue tracks this change
}
</script>
```

Here:

* Changing `user.address.city` will trigger reactivity.

* Vue watches **deep inside** the object.

---

# Shallow Reactivity

Vue provides **shallow** versions of reactivity helpers when you want **only the top-level properties** to be reactive — not the nested ones.

## `shallowReactive()` and `shallowRef()`

These only track reactivity **one level deep**.

## Example:

```vue
<script setup>
import { shallowReactive } from 'vue';

const settings = shallowReactive({
  theme: 'dark',
  options: {
    fontSize: 14
  }
});

function updateFont() {
  settings.options.fontSize = 16;  // Vue won't track this change
}
</script>
```

Here:

* `settings.theme` is reactive.

* But changes inside `settings.options` **won't trigger updates**.

---

# Why Use Shallow Reactivity?

Shallow reactivity is useful when:

* You want to optimize performance.

* You don't care about tracking nested changes.

* You’re managing deeply nested data yourself (like with external libraries, or immutability patterns).

---

# Summary Table

| Feature | Deep Reactivity (`reactive`, `ref`) | Shallow Reactivity (`shallowReactive`, `shallowRef`) |
| ----- | ----- | ----- |
| Tracks nested props | Yes | No |
| Re-renders on deep changes | Yes | Only on top-level changes |
| Use case | Most cases where full tracking is needed | Performance-focused or integrating with external state |

---

# Recap

* Use `reactive()` or `ref()` for **deep reactivity** (default behavior).

* Use `shallowReactive()` or `shallowRef()` to track only top-level properties.

* Choose based on your component’s needs: reactivity depth vs performance.

