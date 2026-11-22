---
tags:
  - FORMAT
  - JavaScript
---
# Debouncing and Throttling in JavaScript

Both **debouncing** and **throttling** are techniques used to **control the frequency** of function executions, especially when handling **frequent events** like scrolling, resizing, or typing. These techniques help in **optimizing performance** by limiting the number of times a function is called in response to these events, and they can be used to avoid **overloading the system** with too many operations.

Here's an overview of both concepts:

---

# Debouncing

**Debouncing** ensures that a function is executed only **after a certain delay** when the event stops firing. It’s used when you want to **wait for the user to stop performing an action** (e.g., typing or resizing) before executing a function.

* **How it works**: The function will only be executed after the user has stopped triggering the event for a specified amount of time.

* **Use cases**:

  * Form validation after the user finishes typing.

  * Search input (trigger search only after typing has stopped).

  * Window resizing (perform an action only after resizing is done).

## Example:

You’re implementing an **auto-suggest search** feature. You don’t want to fire an API request for every keystroke, but instead after the user stops typing.

```javascript
function fetchSuggestions(query) {
    console.log("Fetching suggestions for:", query);
}

// Debounce function implementation
function debounce(func, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer); // Clear the previous timer
        timer = setTimeout(() => {
            func(...args);
        }, delay);
    };
}

// Create a debounced version of fetchSuggestions
const debouncedFetchSuggestions = debounce(fetchSuggestions, 500);

// Event listener for search input
document.getElementById("searchInput").addEventListener("input", (event) => {
    debouncedFetchSuggestions(event.target.value);
});
```

*   
  **How it works**: If the user stops typing for **500ms**, the `fetchSuggestions()` function is triggered. Otherwise, the previous call is canceled, and the new call is scheduled.

## Key Points:

* Debouncing delays the execution until after a pause in events.

* Useful for preventing unnecessary function calls (e.g., preventing an API request on every keypress).

---

# Throttling

**Throttling** ensures that a function is executed at **most once every specified interval**, regardless of how many times an event is triggered. It’s used when you want to **limit the frequency** of executions over time.

* **How it works**: The function will only be executed once in a specified time period (e.g., every 200ms). Even if the event is triggered multiple times in quick succession, the function will only run once during that interval.

* **Use cases**:

  * Scroll events (e.g., infinite scrolling).

  * Window resizing (e.g., adjusting layout at intervals).

  * Mouse movement (e.g., handling mousemove events without overwhelming the browser).

## Example:

Imagine you have a **scroll event** listener that fetches new data when the user reaches the bottom of the page. You don’t want to send multiple requests while the user is scrolling, so throttling is applied.

```javascript
function fetchMoreData() {
    console.log("Fetching more data...");
}

// Throttle function implementation
function throttle(func, limit) {
    let inThrottle = false;
    return function(...args) {
        if (!inThrottle) {
            func(...args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// Create a throttled version of fetchMoreData
const throttledFetchMoreData = throttle(fetchMoreData, 200);

// Event listener for scroll event
window.addEventListener("scroll", () => {
    const scrollPosition = window.scrollY + window.innerHeight;
    const documentHeight = document.documentElement.scrollHeight;

    if (scrollPosition >= documentHeight) {
        throttledFetchMoreData();
    }
});
```

*   
  **How it works**: `fetchMoreData()` will only be called once every **200ms**, no matter how quickly the user scrolls.

## Key Points:

* Throttling limits how frequently a function is called over time.

* Useful for events that can trigger rapidly (e.g., scroll, resize) to prevent excessive function calls.

---

# Debouncing vs Throttling

| Aspect | Debouncing | Throttling |
| ----- | ----- | ----- |
| **Behavior** | Delays execution until after a pause in events. | Executes at regular intervals (e.g., every X ms). |
| **Use case** | Ideal for actions that should happen once after the user has stopped (e.g., typing). | Ideal for actions that need to happen at regular intervals (e.g., scrolling). |
| **Execution Frequency** | Executes once after the event has stopped. | Executes at fixed intervals regardless of event frequency. |
| **Example** | Auto-suggest search, form validation. | Infinite scrolling, window resizing. |

---

# When to Use Which?

* **Use Debouncing** when:

  * You want to execute a function after a user **finishes interacting** (e.g., typing, form submissions).

  * You want to **prevent unnecessary executions** when events happen rapidly.

  * Example: Search suggestion API calls after a user stops typing.

* **Use Throttling** when:

  * You want to **limit the frequency** of executions during continuous or recurring events (e.g., scroll, resize).

  * You want to ensure a function is called at **consistent intervals**, but not too often.

  * Example: Scroll event listener in infinite scroll to load new data.

---

# Key Takeaways:

* **Debouncing**: Delays the execution until there is a pause in the event. Useful for cases where you want to trigger an action after the user stops interacting.

* **Throttling**: Limits the execution to once every specified time interval. Useful when you need to reduce the frequency of function calls over time, especially for events that occur frequently.

Would you like more examples or a deeper dive into implementing these techniques?

