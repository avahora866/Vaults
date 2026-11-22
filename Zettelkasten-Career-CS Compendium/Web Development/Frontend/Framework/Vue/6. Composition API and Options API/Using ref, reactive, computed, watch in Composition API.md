---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
These four functions are the core tools of Vue's **Composition API**. They allow you to manage reactivity, derive values, and react to changes in state.

---

# `ref()` — Reactive Primitive Values

* Use `ref()` to create **reactive primitives** (numbers, strings, booleans).

* It returns an object with a `.value` property.

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <p>{{ count }}</p>
  <button @click="increment">Increment</button>
</template>
```

---

# `reactive()` — Reactive Objects

* Use `reactive()` for **objects and arrays**.

* Unlike `ref`, you **don't use `.value`** to access properties.

```vue
<script setup>
import { reactive } from 'vue'

const user = reactive({
  name: 'Alice',
  age: 25
})

function updateName(newName) {
  user.name = newName
}
</script>

<template>
  <p>{{ user.name }}</p>
  <button @click="updateName('Bob')">Change Name</button>
</template>
```

---

# `computed()` — Derived Reactive Values

* `computed()` creates **read-only reactive values** based on other reactive data.

* It automatically updates when dependencies change.

```vue
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('Jane')
const lastName = ref('Doe')

const fullName = computed(() => `${firstName.value} ${lastName.value}`)
</script>

<template>
  <p>{{ fullName }}</p>
</template>
```

You can also make a **writable computed** value by defining `get` and `set`.

---

# `watch()` — React to Changes

* `watch()` is like a listener. It runs a callback **when a reactive value changes**.

* Useful for side effects like API calls.

```vue
<script setup>
import { ref, watch } from 'vue'

const search = ref('')

watch(search, (newVal, oldVal) => {
  console.log('Search changed from', oldVal, 'to', newVal)
})
</script>

<template>
  <input v-model="search" placeholder="Type to watch..." />
</template>
```

## Options for `watch()`:

* You can watch multiple values using an array

* You can set `immediate: true` to run on first load

* You can set `deep: true` for nested objects

---

# When to Use Each

| Tool | Use When... |
| ----- | ----- |
| `ref()` | You need a reactive primitive (number, string, boolean) |
| `reactive()` | You want a reactive object or array |
| `computed()` | You need to derive a value from other reactive data |
| `watch()` | You want to **do something** when a reactive value changes |


