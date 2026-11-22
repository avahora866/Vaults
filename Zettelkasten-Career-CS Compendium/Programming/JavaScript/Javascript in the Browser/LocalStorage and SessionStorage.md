---
tags:
  - FORMAT
  - JavaScript
---
# LocalStorage and SessionStorage in JavaScript

`LocalStorage` and `SessionStorage` are both web storage solutions provided by modern browsers for storing data on the client side. They allow you to save data in the browser that persists across page reloads or even browser sessions, making them useful for storing user preferences, application state, or temporary data.

Both of these storage mechanisms are part of the **Web Storage API** and are relatively easy to use, but they have different lifetimes and scopes of data persistence.

---

## LocalStorage

`LocalStorage` allows you to store data persistently in the browser. The data saved in `LocalStorage` does **not expire** and remains even after the browser is closed and reopened.

### Key Features of `LocalStorage`

* **Persistent**: Data is stored until explicitly deleted by the user or the code (it remains even after closing the browser).

* **Storage Capacity**: Typically, browsers provide around **5MB of storage per domain**.

* **Same-origin Policy**: Data is accessible only within the same domain, so one domain cannot access the `LocalStorage` data of another domain.

### Methods of `LocalStorage`

* **`setItem(key, value)`**: Saves data to `LocalStorage`.

* **`getItem(key)`**: Retrieves the data associated with the given key.

* **`removeItem(key)`**: Removes the data for the specified key.

* **`clear()`**: Clears all data in `LocalStorage`.

* **`key(index)`**: Returns the key name of the item at the specified index.

* **`length`**: Returns the number of key/value pairs stored in `LocalStorage`.

### Example: Using `LocalStorage`

```javascript
// Save data to LocalStorage
localStorage.setItem('username', 'john_doe');

// Retrieve data from LocalStorage
const username = localStorage.getItem('username');
console.log(username);  // Outputs: john_doe

// Remove a specific item from LocalStorage
localStorage.removeItem('username');

// Clear all items from LocalStorage
localStorage.clear();
```

### When to Use `LocalStorage`

* Storing user preferences (e.g., theme settings).

* Storing non-sensitive, persistent data across sessions (e.g., shopping cart items, form inputs).

* Caching data locally to improve performance.

---

## SessionStorage

`SessionStorage` is similar to `LocalStorage`, but it is designed for storing data for the duration of a **single browser session**. Data stored in `SessionStorage` is **only available during the current page session** (i.e., until the browser tab or window is closed).

### Key Features of `SessionStorage`

* **Temporary**: Data is automatically cleared when the tab or browser is closed. This means that once the user closes the browser tab, the data is lost.

* **Storage Capacity**: Also provides around **5MB of storage per domain**, similar to `LocalStorage`.

* **Same-origin Policy**: Like `LocalStorage`, the data is specific to the same domain and cannot be accessed by other domains.

### Methods of `SessionStorage`

The methods for `SessionStorage` are nearly identical to those for `LocalStorage`:

* **`setItem(key, value)`**: Saves data to `SessionStorage`.

* **`getItem(key)`**: Retrieves data associated with the given key.

* **`removeItem(key)`**: Removes the data for the specified key.

* **`clear()`**: Clears all data in `SessionStorage`.

* **`key(index)`**: Returns the key name at the specified index.

* **`length`**: Returns the number of key/value pairs stored in `SessionStorage`.

### Example: Using `SessionStorage`

```javascript
// Save data to SessionStorage
sessionStorage.setItem('userSession', 'active');

// Retrieve data from SessionStorage
const userSession = sessionStorage.getItem('userSession');
console.log(userSession);  // Outputs: active

// Remove a specific item from SessionStorage
sessionStorage.removeItem('userSession');

// Clear all items from SessionStorage
sessionStorage.clear();
```

### When to Use `SessionStorage`

* Storing data that should only persist for the current browser session (e.g., user authentication during a session).

* Storing temporary data like form inputs or session-related information that shouldn't be saved after the browser window/tab is closed.

---

## Comparison: LocalStorage vs. SessionStorage

| Feature | `LocalStorage` | `SessionStorage` |
| ----- | ----- | ----- |
| **Persistence** | Data persists across browser sessions | Data persists only during the current session (until the tab/window is closed) |
| **Scope** | Available across all tabs/windows of the same origin | Available only in the tab/window where data was stored |
| **Storage Capacity** | Typically around **5MB per domain** | Typically around **5MB per domain** |
| **API Methods** | Same as `SessionStorage` (`setItem()`, `getItem()`, `removeItem()`, `clear()`) | Same as `LocalStorage` (`setItem()`, `getItem()`, `removeItem()`, `clear()`) |
| **Use Cases** | Storing long-term preferences or settings | Storing temporary session data |

---

## Key Takeaways

* **`LocalStorage`** is useful for storing data that should persist across browser sessions and even after the browser is closed.

* **`SessionStorage`** is suitable for temporary data that only needs to persist during a single session (i.e., until the tab or browser window is closed).

* Both `LocalStorage` and `SessionStorage` have a storage capacity of approximately 5MB per domain, and both use a key-value pair system for storing data.

* Data in **`LocalStorage`** remains even after the browser is closed, whereas **`SessionStorage`** data is automatically cleared when the browser tab or window is closed.

By choosing between `LocalStorage` and `SessionStorage`, you can decide the best storage solution based on the persistence requirements of the data you're working with.

