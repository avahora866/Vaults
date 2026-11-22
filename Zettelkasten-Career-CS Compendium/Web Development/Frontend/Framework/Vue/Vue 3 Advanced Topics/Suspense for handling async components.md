---
tags:
  - Vue
---
**Vue Suspense** is a feature introduced in Vue 3 that helps manage **async components** and **delayed content** in your application. It allows you to handle asynchronous components gracefully by showing a fallback UI (like a loading spinner) while waiting for an async operation (e.g., fetching data, loading a component, etc.) to complete.

Suspense is especially useful in scenarios where you want to load a component or data asynchronously without blocking the rest of the page. It provides a more user-friendly experience by showing something (like a loading spinner) while waiting for async content.

---

# Key Concepts of Suspense

1. **Async Components**: These are components that are loaded lazily (e.g., through dynamic `import()`), which means they aren't loaded immediately but rather when needed.

2. **Fallback Content**: While waiting for an async operation to finish, you can display fallback content, such as a loading spinner, placeholder, or custom UI, to inform users that something is loading.

3. **Suspense Boundary**: The `Suspense` component wraps around your async content and allows you to specify what should be shown while the content is loading.

---

# How Suspense Works

* **Suspense** is a wrapper component that waits for one or more child components to finish loading before rendering them. If the child components are asynchronous, you can specify a fallback UI to be shown while waiting.

# Basic Syntax

```vue
<Suspense>
  <template #default>
    <!-- Async component goes here -->
    <AsyncComponent />
  </template>

  <template #fallback>
    <!-- Fallback content (e.g., a loading spinner) goes here -->
    <div>Loading...</div>
  </template>
</Suspense>
```

*   
  `#default`: This is the content that is displayed when the async component has finished loading.

 `#fallback`: This is the fallback content that is displayed while waiting for the async component to load.

---

# Example: Lazy-Loading an Async Component with Suspense

Let’s create a simple example where we lazily load a component (e.g., `AsyncComponent`) and show a loading indicator until it is fully loaded.

## AsyncComponent.vue

```vue
<template>
  <div>
    <h2>Async Component</h2>
    <p>This component is loaded asynchronously.</p>
  </div>
</template>

<script>
export default {
  name: 'AsyncComponent',
};
</script>
```

## App.vue (Using Suspense)

```vue
<template>
  <Suspense>
    <template #default>
      <!-- Async component will be loaded here -->
      <AsyncComponent />
    </template>
    <template #fallback>
      <!-- Fallback content to show while waiting for AsyncComponent -->
      <div>Loading component...</div>
    </template>
  </Suspense>
</template>

<script>
// Dynamically import AsyncComponent
import { defineAsyncComponent } from 'vue';

export default {
  components: {
    AsyncComponent: defineAsyncComponent(() => import('./components/AsyncComponent.vue')),
  },
};
</script>
```

## How It Works:

* **Suspense** is wrapping the `AsyncComponent`.

* **Fallback content** ("Loading component...") is shown until the `AsyncComponent` finishes loading.

* Once the `AsyncComponent` is loaded, it will be displayed instead of the fallback content.

---

# Handling Async Data Fetching with Suspense

Suspense can also be used for async data fetching (like API calls). You can use the `Suspense` feature with async `setup()` functions, which allow you to fetch data and wait for it to resolve before rendering the component.

## Example: Suspense with Async Data Fetching

```vue
<template>
  <Suspense>
    <template #default>
      <!-- Data-loaded component -->
      <UserProfile />
    </template>
    <template #fallback>
      <!-- Loading state while fetching data -->
      <div>Loading user profile...</div>
    </template>
  </Suspense>
</template>

<script>
// Dynamically import the UserProfile component
import { defineAsyncComponent } from 'vue';

export default {
  components: {
    UserProfile: defineAsyncComponent(() => import('./components/UserProfile.vue')),
  },
};
</script>
```

## UserProfile.vue

```vue
<template>
  <div>
    <h2>User Profile</h2>
    <p>{{ user.name }}</p>
    <p>{{ user.email }}</p>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'UserProfile',
  setup() {
    const user = ref(null);

    // Simulate an API call to fetch user data
    const fetchUserData = async () => {
      const response = await fetch('https://api.example.com/user');
      const data = await response.json();
      user.value = data;
    };

    onMounted(fetchUserData);

    return { user };
  },
};
</script>
```

# How It Works:

* **UserProfile.vue** fetches the user data asynchronously in the `setup()` function using the `onMounted()` lifecycle hook.

* **Suspense** waits for the `UserProfile` component to finish its async operation (in this case, the data fetching).

* While waiting for the data, the fallback UI (`<div>Loading user profile...</div>`) is displayed.

* Once the data is fetched and the component is ready, the main content (user profile) is rendered.

---

# Advanced Suspense: Using `Suspense` with Multiple Async Components

You can wrap multiple async components in a single `Suspense` component. Vue will wait for all async components inside the boundary to resolve before rendering them.

## Example: Multiple Async Components in Suspense

```vue
<template>
  <Suspense>
    <template #default>
      <div>
        <AsyncComponent1 />
        <AsyncComponent2 />
      </div>
    </template>
    <template #fallback>
      <div>Loading components...</div>
    </template>
  </Suspense>
</template>

<script>
import { defineAsyncComponent } from 'vue';

export default {
  components: {
    AsyncComponent1: defineAsyncComponent(() => import('./components/AsyncComponent1.vue')),
    AsyncComponent2: defineAsyncComponent(() => import('./components/AsyncComponent2.vue')),
  },
};
</script>
```

*   
  Here, `AsyncComponent1` and `AsyncComponent2` are loaded lazily.

* The fallback UI is shown until both components finish loading.

* Once both components are ready, they will be displayed.

---

# Why Use Suspense?

* **Improved User Experience**: Suspense ensures that users see a consistent and smooth experience, showing a loading indicator until async operations are complete.

* **Better Handling of Async Code**: It simplifies handling async components and data fetching by providing a declarative API for async content loading.

* **Optimized Rendering**: It avoids blocking rendering until all async operations are complete, allowing the rest of the page to load or interact independently.

---

# Conclusion

**Suspense** in Vue 3 is a powerful tool for handling async operations like component loading and data fetching. It allows you to:

* **Render fallback content** while waiting for async components or data to load.

* **Simplify the management of async components** and asynchronous data fetching.

* Provide a better user experience by handling delays gracefully without blocking the rendering of other parts of the page.

Suspense helps make your app more robust and user-friendly by enabling smooth transitions and loading states, particularly in complex asynchronous scenarios.

