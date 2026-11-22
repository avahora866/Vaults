---
tags:
  - Vue
---
The `setup()` function is the **entry point** for the **Composition API** in Vue It replaces most of the Options API features (`data`, `methods`, `computed`, `created`, etc.) and gives you more flexibility to organize logic.

---

# What Is `setup()`?

* It runs **before** the component is created.

* It’s where you define **reactive state**, **functions**, **computed values**, and **lifecycle hooks**.

* Everything returned from `setup()` is available in the component's template.

---

# Basic Example

```vue
<template>
  <p>Count: {{ count }}</p>
  <button @click="increment">Increment</button>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)
const increment = () => count.value++
</script>
```

## What's happening here:

* `ref(0)` creates a reactive value.

* `increment()` is a regular function.

* No need for `return` when using `<script setup>` (automatically exposed).

---

# Using `reactive`

Use `reactive()` when you want to create a reactive object instead of a single value.

```vue
<script setup>
import { reactive } from 'vue'

const user = reactive({
  name: 'Alice',
  age: 30
})

function updateName(newName) {
  user.name = newName
}
</script>
```

---

# Computed Properties

```vue
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed(() => `${firstName.value} ${lastName.value}`)
</script>
```

---

# Lifecycle Hooks Inside `setup()`

Vue provides Composition API lifecycle hooks like:

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log('Component is mounted')
})
</script>
```

---

# Returning from `setup()` (Without `<script setup>`)

If you're **not using** `<script setup>`, you return the values you want accessible in the template:

```javascript
export default {
  setup() {
    const count = ref(0)
    const increment = () => count.value++

    return { count, increment }
  }
}
```

---

# Summary

| Feature | Inside `setup()` |
| ----- | ----- |
| State | `ref()`, `reactive()` |
| Computed values | `computed()` |
| Functions | Define normally |
| Lifecycle hooks | `onMounted()`, `onUpdated()` etc. |
| Return to template | Automatically in `<script setup>`, or manually return in normal `setup()` |

---

