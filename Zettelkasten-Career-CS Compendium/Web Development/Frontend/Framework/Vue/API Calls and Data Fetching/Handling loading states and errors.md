---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
When fetching data (especially from an API), it's important to give users feedback — like showing a loading spinner or an error message if something goes wrong.

Vue makes it simple to manage this using **reactive state** with either the Options API or Composition API. Here's how to do it step-by-step using the **Composition API**.

---

# Step 1: Define Reactive State

You'll usually want to track:

* `isLoading` – whether the request is in progress.

* `error` – any error message that occurs.

* `data` – the result of your fetch.

```javascript
import { ref, onMounted } from 'vue'
import axios from 'axios'

const isLoading = ref(false)
const error = ref(null)
const data = ref(null)
```

---

# Step 2: Use These States in an API Call

Wrap your fetch logic in a `try...catch` block and update the state accordingly.

```javascript
onMounted(async () => {
  isLoading.value = true
  error.value = null

  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
    data.value = response.data
  } catch (err) {
    error.value = err.message || 'Something went wrong.'
  } finally {
    isLoading.value = false
  }
})
```

---

# Step 3: Update the Template

Now you can conditionally render based on the loading and error state.

```vue
<template>
  <div>
    <h2>Posts</h2>

    <div v-if="isLoading">Loading...</div>

    <div v-else-if="error" class="error">
      Error: {{ error }}
    </div>

    <ul v-else>
      <li v-for="post in data" :key="post.id">
        {{ post.title }}
      </li>
    </ul>
  </div>
</template>
```

---

# Putting It All Together

Here’s a complete example using `<script setup>`:

```vue
<template>
  <div>
    <h2>Posts</h2>

    <div v-if="isLoading">Loading...</div>

    <div v-else-if="error" class="error">Error: {{ error }}</div>

    <ul v-else>
      <li v-for="post in data" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const isLoading = ref(false)
const error = ref(null)
const data = ref([])

onMounted(async () => {
  isLoading.value = true
  error.value = null

  try {
    const res = await axios.get('https://jsonplaceholder.typicode.com/posts')
    data.value = res.data
  } catch (err) {
    error.value = err.message || 'An unexpected error occurred.'
  } finally {
    isLoading.value = false
  }
})
</script>

<style>
.error {
  color: red;
}
</style>
```

---

# Extras (Optional but Useful)

## Debounce API calls (for search inputs):

To avoid sending requests too frequently, use `lodash.debounce` or `watchEffect` wisely.

## Retry on Error:

Let the user retry manually by showing a "Try Again" button if there's an error.

## Global Loading Spinner:

You can use a global `isLoading` state via **Pinia** or **provide/inject** if many components need to show loading.

---

