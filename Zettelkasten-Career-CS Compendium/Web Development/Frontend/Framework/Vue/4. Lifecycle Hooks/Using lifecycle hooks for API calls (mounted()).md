---
tags:
  - Vue
---
Using Vue lifecycle hooks for API calls is a common pattern in Vue applications. The `mounted()` hook is especially useful because it's called after the component is mounted to the DOM, making it a good place to trigger API requests that need to run once the component is fully rendered.

---

# Why Use the `mounted()` Hook for API Calls?

* **DOM is available**: By the time `mounted()` is called, the component has been added to the DOM. This means you can safely interact with the DOM if needed (e.g., to display loading indicators).

* **One-time requests**: Typically, you'll want to fetch data when the component is first displayed. The `mounted()` hook ensures this by triggering only once when the component is mounted.

---

# Example: Using `mounted()` for an API Call

## Example with Fetch API

```javascript
export default {
  data() {
    return {
      users: [],     // Data to store fetched results
      isLoading: true, // Flag to show loading indicator
      error: null     // For any errors from the API call
    };
  },
  mounted() {
    // Making the API call in the mounted hook
    this.fetchUsers();
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        const data = await response.json();
        this.users = data;  // Store the fetched data in the component's state
        this.isLoading = false; // Hide the loading indicator
      } catch (error) {
        this.error = 'Error fetching data';  // Handle the error
        this.isLoading = false; // Hide the loading indicator even if there's an error
      }
    }
  }
};
```

## Breakdown of the Code:

* **`data`**: Holds the state for the component:

  * `users`: An array to hold the API data.

  * `isLoading`: A flag to indicate whether the data is still loading (useful for showing a loading spinner).

  * `error`: To handle any error that might occur during the API call.

* **`mounted()`**: This lifecycle hook is where the `fetchUsers()` method is called. Once the component is mounted, the API call is triggered.

* **`fetchUsers()`**: A method that makes the API call:

  * It uses the `fetch` API to get data from a remote server.

  * When the response is received, it's parsed as JSON, and the data is assigned to the `users` state.

  * If an error occurs, the `error` state is set, and the loading indicator is hidden.

* **Error Handling**: By using a `try-catch` block, you ensure that if something goes wrong (e.g., network issues), the component can gracefully handle the error and update the UI accordingly.

---

# Example: Using `mounted()` with Axios (Common in Vue Projects)

If you're using a popular library like **Axios** for making API requests, the process is very similar.

## Example with Axios

```javascript
import axios from 'axios';

export default {
  data() {
    return {
      users: [],
      isLoading: true,
      error: null
    };
  },
  mounted() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      axios.get('https://jsonplaceholder.typicode.com/users')
        .then(response => {
          this.users = response.data;  // Store the response data
          this.isLoading = false;  // Stop loading
        })
        .catch(error => {
          this.error = 'Error fetching data'; // Handle any error
          this.isLoading = false;
        });
    }
  }
};
```

In this example, **Axios** is used instead of `fetch`:

* Axios automatically handles JSON parsing, so you can directly use `response.data`.

* The flow is similar: initiate the request in `mounted()`, and handle both success and failure.

---

# Advantages of Using `mounted()` for API Calls

1. **Initial data load**: Perfect for fetching data when the component first loads. For example, loading user profiles, product details, or any initial data.

2. **No DOM blocking**: Since API calls are asynchronous, using `mounted()` won't block the rendering of the DOM. The user can see the component while the data is being fetched in the background.

3. **Conditional rendering**: You can show a **loading spinner** or a **"no data" message** while waiting for the data to be available. The `isLoading` flag in the example above handles that.

---

# When Not to Use `mounted()` for API Calls

While `mounted()` is great for many scenarios, there are cases where another lifecycle hook may be more appropriate:

* **`created()`**: If you need the data **before** the component is rendered (for example, to pre-populate form fields), you might use the `created()` hook, though this means the DOM won't be available yet.

* **`beforeRouteEnter` or `beforeRouteUpdate`**: If you need to fetch data when a route changes, these hooks can be useful for fetching data **before** rendering the component.

---

# Conclusion

* The **`mounted()`** hook is perfect for making **API calls** in Vue, as it ensures the component is fully mounted to the DOM before the request is made.

* You can **show loading states**, **handle errors**, and **populate data** in your component once the API request is completed.

* Consider using **Axios** or **Fetch** for making the actual API call, depending on your preferences.


