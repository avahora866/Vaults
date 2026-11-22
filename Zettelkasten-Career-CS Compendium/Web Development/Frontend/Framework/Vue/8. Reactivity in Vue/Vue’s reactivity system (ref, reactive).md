---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
Vue’s **reactivity system** is what makes your UI stay in sync with your data — when data changes, the DOM updates automatically.

In **Vue 3**, the reactivity system is more powerful and flexible thanks to Composition API features like `ref` and `reactive`.

---

# What is Reactivity in Vue?

Reactivity means that when your **data changes**, Vue will automatically **update the DOM** where that data is used — no need to manually manipulate the DOM.

---

# `ref()`

`ref()` is used to create **reactive primitives** — single values like strings, numbers, booleans, etc.

## Example:

```vue
<script setup>
import { ref } from 'vue';

const count = ref(0);

function increment() {
  count.value++;
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

## How it works:

* `ref(0)` creates a reactive object with a `.value` property.

* You access or update the actual value with `.value`.

* When `count.value` changes, the DOM updates.

---

# `reactive()`

`reactive()` is used for **objects and arrays** — it creates a deeply reactive version of the object.

## Example:

```vue
<script setup>
import { reactive } from 'vue';

const user = reactive({
  name: 'Alice',
  age: 25
});

function growOlder() {
  user.age++;
}
</script>

<template>
  <div>
    <p>{{ user.name }} is {{ user.age }} years old</p>
    <button @click="growOlder">Grow Older</button>
  </div>
</template>
```

## How it works:

* `reactive()` wraps the entire object.

* Vue tracks all changes and updates anything bound to the object's properties.

---

# Key Differences

| Feature | `ref()` | `reactive()` |
| ----- | ----- | ----- |
| Used for | Primitives (number, string, etc) | Objects, arrays, nested data |
| Access syntax | `count.value` | `user.name`, `user.age` |
| Reactivity | Wraps value in `.value` | Makes all properties reactive |

---

# Behind the Scenes

Vue uses **Proxies** under the hood to intercept reads/writes to properties and trigger DOM updates when needed.

---

# You Can Combine Them

You can use `ref()` with objects too, but then you need `.value` for everything:

```javascript
const state = ref({ name: 'Alice' });
state.value.name = 'Bob';
```

Or use `reactive()` to avoid `.value`:

```javascript
const state = reactive({ name: 'Alice' });
state.name = 'Bob';
```

---

# Summary

| Term | Use For | Access | Example |
| ----- | ----- | ----- | ----- |
| `ref()` | Single values | `.value` | `const count = ref(0)` |
| `reactive()` | Objects/arrays | direct | `const user = reactive({...})` |

*   
  Use `ref()` for numbers, booleans, strings.

* Use `reactive()` for complex objects.

* Vue auto-tracks these so your UI always stays in sync.

