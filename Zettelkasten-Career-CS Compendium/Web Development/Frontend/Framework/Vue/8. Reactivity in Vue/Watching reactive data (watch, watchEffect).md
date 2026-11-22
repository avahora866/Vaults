---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue 3, `watch` and `watchEffect` are part of the **Composition API**, and they let you **respond to changes** in reactive data.

---

# Why Use `watch` and `watchEffect`?

Sometimes you need to do something **when data changes**, like:

* Fetch data when a prop or variable changes

* Trigger animations

* Log values

* Debounce inputs

---

# `watch`: Watch a specific source

Use `watch` when you want to track **specific variables** or **reactive properties**.

## Basic Syntax

```javascript
watch(source, callback)
```

*   
  `source`: the reactive variable, computed, or getter function

* `callback`: a function called when `source` changes

## Example

```vue
<script setup>
import { ref, watch } from 'vue';

const name = ref('Alice');

watch(name, (newVal, oldVal) => {
  console.log(`Name changed from ${oldVal} to ${newVal}`);
});
</script>
```

---

## Watching a Function (Getter)

This is useful when watching part of an object or a computed-like value:

```javascript
watch(() => user.name, (newVal) => {
  console.log('User name changed to:', newVal);
});
```

---

## Watching Multiple Sources

```javascript
watch([foo, bar], ([newFoo, newBar], [oldFoo, oldBar]) => {
  console.log('Changes:', newFoo, newBar);
});
```

---

# `watchEffect`: Automatically track dependencies

`watchEffect` is **lazy and auto-tracking** — it runs once immediately, then again whenever **any reactive data inside it** changes.

## Syntax

```javascript
watchEffect(() => {
  // any reactive data used in here will be tracked
});
```

## Example

```vue
<script setup>
import { ref, watchEffect } from 'vue';

const count = ref(0);

watchEffect(() => {
  console.log(`The count is now: ${count.value}`);
});
</script>
```

This will run once initially, and again any time `count.value` changes.

---

# Key Differences: `watch` vs `watchEffect`

| Feature | `watch` | `watchEffect` |
| ----- | ----- | ----- |
| Usage | Track specific source | Auto tracks anything used inside block |
| Runs immediately | No (by default) | Yes |
| Control | More precise (you define source) | Less control, but very convenient |
| Good for | Async calls, conditional logic | Logging, side-effects tied to state |

---

# Options

## `immediate: true`

Make `watch` run once immediately:

```vue
watch(name, callback, { immediate: true });
```

## `deep: true`

Track nested properties in objects/arrays:

```javascript
watch(user, callback, { deep: true });
```

But prefer watching specific properties or computed values when possible for better performance.

---

# Summary

* Use `watch` to **target specific reactive data**

* Use `watchEffect` when you want **automatic tracking and simpler syntax**

* Both are **side-effect utilities**, not replacements for reactive state

