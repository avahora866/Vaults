---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js applications, it's common to fetch data from APIs to populate components or pages. Two of the most popular ways to do this are:

* The native **`fetch`** API (built into modern browsers)

* The **`axios`** library (a powerful HTTP client)

Both approaches allow you to make asynchronous HTTP requests, but they have some differences in usage and features.

---

# Fetching Data with the `fetch` API

The `fetch` API is a native browser API used to make HTTP requests. It returns a **Promise**, which resolves with the `Response` object representing the response to the request.

## Example: Fetch Data on Component Mount

```vue
<template>
  <div>
    <h2>Posts</h2>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const posts = ref([])

onMounted(async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts')
  const data = await res.json()
  posts.value = data
})
</script>
```

## Key Notes:

* **`fetch()`** takes the URL as its first argument.

* You need to explicitly call `.json()` to parse the response body.

* You must handle errors manually using `try...catch`.

## Error Handling with Fetch

```javascript
try {
  const res = await fetch(url)
  if (!res.ok) throw new Error(`HTTP error! status: ${res.status}`)
  const data = await res.json()
} catch (error) {
  console.error('Fetch error:', error)
}
```

---

# Fetching Data with `axios`

**`axios`** is a popular third-party HTTP client that simplifies making requests and handling responses. It supports interceptors, automatic JSON parsing, and better error handling than `fetch`.

## Installation

```
npm install axios
```

## Basic Usage

```vue
<template>
  <div>
    <h2>Users</h2>
    <ul>
      <li v-for="user in users" :key="user.id">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const users = ref([])

onMounted(async () => {
  try {
    const res = await axios.get('https://jsonplaceholder.typicode.com/users')
    users.value = res.data
  } catch (err) {
    console.error('Axios error:', err)
  }
})
</script>
```

## Advantages of Axios over Fetch:

| Feature | `fetch` | `axios` |
| ----- | ----- | ----- |
| **Default JSON parsing** | No (must use `.json()`) | Yes (`res.data` is parsed) |
| **Error Handling** | Manual check `res.ok` | Automatically throws on error |
| **Request/Response Interceptors** | No | Yes |
| **Timeouts** | No built-in | Yes |
| **Wide browser support** | Native in modern browsers | Requires polyfill for old ones |

---

# Which Should You Use?

* **Use `fetch`** if:

  * You want a lightweight, native solution.

  * You’re only making basic requests and can manually handle parsing and errors.

* **Use `axios`** if:

  * You need easier syntax, built-in error handling, and JSON parsing.

  * You want advanced features like request/response interceptors, timeouts, or global configuration.

---

# Example: Posting Data with Fetch and Axios

## Using `fetch`

```javascript
await fetch('/api/posts', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ title: 'New Post' })
})
```

## Using `axios`

```javascript
await axios.post('/api/posts', { title: 'New Post' })
```

---

If you’re building a Vue project with lots of API interactions, `axios` is usually more convenient. But for lightweight or one-off data fetching, `fetch` is totally fine.

