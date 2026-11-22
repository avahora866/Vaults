---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
`watchEffect` is part of Vue's Composition API and is very useful for running **side effects** reactively. When using it with API calls, it helps you automatically re-fetch data **whenever the dependencies used in the effect change**, without explicitly declaring them.

---

# What is `watchEffect`?

* `watchEffect()` immediately runs a function and re-runs it **automatically** when any reactive dependency it uses changes.

* It’s **auto-tracking**, meaning you don’t need to declare which variables to watch—it figures it out from usage.

---

# Basic Use Case: Reactive API Call

Here’s an example of how `watchEffect` can be used to make an API call whenever a reactive variable like a `searchQuery` changes.

```vue
<template>
  <div>
    <input v-model="searchQuery" placeholder="Search users..." />
    <ul>
      <li v-for="user in users" :key="user.id">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import axios from 'axios'

const searchQuery = ref('')
const users = ref([])

watchEffect(async () => {
  if (!searchQuery.value) {
    users.value = []
    return
  }

  try {
    const res = await axios.get(`https://jsonplaceholder.typicode.com/users?name_like=${searchQuery.value}`)
    users.value = res.data
  } catch (error) {
    console.error('API error:', error)
  }
})
</script>
```

---

# How It Works

* The `watchEffect` block runs **immediately** when the component mounts.

* It then re-runs **any time `searchQuery.value` changes**, because it's used inside the effect.

* You don’t need to explicitly declare what to watch—Vue tracks it automatically.

---

# When to Use `watchEffect`

Good for:

* Auto-running API calls when **reactive data changes**.

* Cases where you're watching **multiple sources** (no need to specify them manually).

* Quick prototyping.

Not ideal for:

* Watching something **only when a specific variable changes** (use `watch()` for that).

* Expensive operations that don’t need to re-run on every tiny change.

---

# Example: Compare with `watch`

If you wanted to do the same thing with `watch()`, you’d write:

```javascript
watch(searchQuery, async (newQuery) => {
  // fetch data based on newQuery
})
```

With `watchEffect`, it’s more concise and automatic.

---

# Optional Cleanup

You can return a cleanup function inside `watchEffect`, for example, to cancel a previous request if needed.

```javascript
watchEffect((onCleanup) => {
  const controller = new AbortController()

  axios.get('/api/data', { signal: controller.signal })
    .then(res => { /* handle res */ })
    .catch(err => { if (err.name !== 'AbortError') console.error(err) })

  onCleanup(() => {
    controller.abort()
  })
})
```

---

# Conclusion

`watchEffect` is a powerful tool in Vue’s Composition API for running **automatic reactive logic** like API calls. It's ideal when:

* You want to **auto-track** dependencies

* You want to **fetch data reactively** based on inputs or parameters

