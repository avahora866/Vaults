---
tags:
  - Vue
---
**Debouncing** and **throttling** are both techniques used to control the rate at which a function is executed, often in response to user actions like scrolling, resizing, or typing. They help improve performance by limiting the number of times a function is called during rapid, continuous events. Lodash, a popular utility library, provides functions like `debounce` and `throttle` that make implementing these techniques easy.

---

# Debouncing

## What is Debouncing?

Debouncing is the practice of ensuring that a function is executed only after a specified delay, and only if the event has stopped firing for a certain amount of time. In other words, it **delays** the execution of the function until after a certain "pause" in the event.

## Use Case for Debouncing

* **Search input**: Imagine you're implementing an auto-suggest search feature. Without debouncing, the search function could be triggered every time the user types a character, making unnecessary API calls. Instead, debouncing delays the API call until the user stops typing for a specified duration.

## How Debouncing Works

* Each time the event is fired (e.g., keystroke, scroll), the debounced function is reset. The function will only execute after the event stops for a certain period of time.

## Debouncing with Lodash

Lodash's `debounce` function helps to delay the invocation of a function until after a certain amount of time has passed since the last call.

```javascript
import { debounce } from 'lodash';

// Function to be debounced
const search = debounce(function(query) {
  console.log('Searching for: ', query);
}, 300); // 300ms delay

// Simulate input event
document.getElementById('search-input').addEventListener('input', function(event) {
  search(event.target.value);
});
```

## Explanation:

* **`debounce`**: The function `search` will only execute after the user has stopped typing for 300 milliseconds.

* **Input Event**: Each time the user types in the search box, the event is debounced, and the search function is not called immediately.

---

# Throttling

## What is Throttling?

Throttling is the practice of ensuring that a function is called at most once every specified interval, regardless of how many times the event is triggered. Unlike debouncing, throttling does not wait for the event to stop firing; it simply limits how frequently the function can be executed.

## Use Case for Throttling

* **Scroll events**: If you're implementing a scroll-based feature, like lazy loading or a "back-to-top" button, you may want to limit the number of times the scroll handler is called while the user is scrolling, even if the user scrolls continuously.

* **Resizing**: When handling window resizing events, throttling ensures the resize handler is not called too frequently.

## How Throttling Works

* Throttling ensures that the function is invoked at most once in a specified period, even if the event fires continuously.

## Throttling with Lodash

Lodash's `throttle` function is used to make sure that a function is only called at most once in a defined time interval.

```javascript
import { throttle } from 'lodash';

// Function to be throttled
const handleScroll = throttle(function() {
  console.log('Scrolling...');
}, 200); // 200ms interval

// Simulate scroll event
window.addEventListener('scroll', handleScroll);
```

## Explanation:

* **`throttle`**: The `handleScroll` function will only be called once every 200 milliseconds while the user is scrolling, regardless of how frequently the scroll event is triggered.

* **Scroll Event**: This ensures that the scroll handler does not get invoked too many times, improving performance.

---

# Key Differences Between Debouncing and Throttling

| Feature | Debouncing | Throttling |
| ----- | ----- | ----- |
| **Behavior** | Waits for the event to stop before calling the function | Ensures the function is called at a consistent interval |
| **Typical Use Case** | Input fields (e.g., search suggestions) | Continuous events (e.g., scrolling, resizing) |
| **Invocation Pattern** | Executes after the event stops for a specified delay | Executes once every specified time interval, no matter how frequently the event fires |
| **Example** | API call after user stops typing | Scroll event handler every 200ms while scrolling |

---

# When to Use Debouncing vs Throttling

* **Use Debouncing** when you want to wait for a pause in a series of events, typically for actions like typing, resizing, or searching, where you don't need every event to trigger a response, just the final one.

* **Use Throttling** when you want to limit the number of times an event handler is called, even if the event is firing continuously (like scroll or resize events).

---

## Real-World Examples

## Debouncing Example: Search Input

Imagine a search bar where users type queries and get search suggestions. Without debouncing, every keystroke could trigger an API call. With debouncing, the API call will only be triggered once the user stops typing for a certain time.

```javascript
import { debounce } from 'lodash';

const search = debounce(async function(query) {
  const response = await fetch(`https://api.example.com/search?q=${query}`);
  const data = await response.json();
  console.log(data);
}, 500); // 500ms delay

document.getElementById('search-input').addEventListener('input', (event) => {
  search(event.target.value);
});
```

*   
  **Effect**: The API request will only be made after the user has stopped typing for 500ms.

## Throttling Example: Scroll Event

Consider an infinite scroll feature, where items load as the user scrolls down the page. To prevent the function from being called too frequently during scrolling, you can throttle the scroll handler.

```javascript
import { throttle } from 'lodash';

const loadMoreItems = throttle(function() {
  console.log('Loading more items...');
  // Fetch and append more items here
}, 1000); // Call this function at most once every 1 second

window.addEventListener('scroll', loadMoreItems);
```

*   
  **Effect**: The `loadMoreItems` function will only be called once every second while the user is scrolling, improving performance by not firing too many requests.

---

## Conclusion

* **Debouncing** ensures that a function is executed only after a pause in the event (ideal for search inputs and other interactions where you want to respond to the "final" event).

* **Throttling** ensures that a function is called at most once every specified interval (ideal for continuous events like scrolling or resizing).

Using **Lodash's `debounce` and `throttle`** functions can help you optimize performance in your Vue applications by controlling how often certain events trigger functions.

