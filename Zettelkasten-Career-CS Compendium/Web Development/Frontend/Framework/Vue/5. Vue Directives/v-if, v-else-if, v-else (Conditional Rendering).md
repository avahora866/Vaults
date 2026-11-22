---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js, **conditional rendering** allows you to control which elements or components are displayed in the DOM based on certain conditions. The `v-if`, `v-else-if`, and `v-else` directives are used to render content conditionally, and they provide powerful ways to control the flow of your templates.

# `v-if` (Conditional Rendering)

The **`v-if`** directive is used to conditionally render an element or a block of elements based on a **Boolean expression**. If the expression evaluates to `true`, the element is rendered in the DOM; if it evaluates to `false`, the element is not rendered at all.

## Basic Syntax:

```vue
<div v-if="isVisible">This is conditionally rendered</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    isVisible: true
  }
});
```

In this example:

* If `isVisible` is `true`, the `<div>` element will be displayed.

* If `isVisible` is `false`, the `<div>` will not appear in the DOM at all.

## Important Notes:

* The element is **completely removed** from the DOM when the condition is `false` (it's not just hidden).

* This is a **reactive** binding, meaning the DOM updates whenever the condition changes.

# `v-else-if` (Alternative Conditions)

The **`v-else-if`** directive allows you to chain multiple conditional checks. It is used to specify a **secondary condition** that will be checked only if the previous condition (`v-if`) evaluates to `false`.

## Basic Syntax:

```vue
<div v-if="type === 'A'">Type A content</div>
<div v-else-if="type === 'B'">Type B content</div>
<div v-else>If neither condition is true, this content will be displayed</div>
```

In this example:

* If `type` is `'A'`, the first `<div>` will be shown.

* If `type` is not `'A'` but is `'B'`, the second `<div>` will be displayed.

* If neither condition is met, the third `<div>` will be rendered.

## When to Use `v-else-if`:

Use `v-else-if` to check for **alternative conditions** after the `v-if` condition is false. This allows you to test for multiple possible conditions and render different elements accordingly.

# `v-else` (Fallback Condition)

The **`v-else`** directive is used to render an element when all previous conditions fail (i.e., if `v-if` and `v-else-if` conditions are `false`). It's the **fallback** option when none of the conditions match.

## Basic Syntax:

```vue
<div v-if="isLoggedIn">Welcome back, user!</div>
<div v-else>Please log in to continue</div>
```

In this example:

* If `isLoggedIn` is `true`, the first `<div>` is rendered.

* If `isLoggedIn` is `false`, the second `<div>` (with `v-else`) is rendered.

## Important Notes:

* The `v-else` directive must be placed immediately after a `v-if` or `v-else-if` element without any other elements in between.

---

# Example of `v-if`, `v-else-if`, and `v-else` in Action

Here’s a more complete example to show how all three directives work together:

```vue
<div id="app">
  <p v-if="status === 'loading'">Loading...</p>
  <p v-else-if="status === 'error'">An error occurred. Please try again later.</p>
  <p v-else>Content has been successfully loaded!</p>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    status: 'loading' // Try changing this value to 'error' or 'loaded'
  }
});
```

In this example:

* If `status` is `'loading'`, the first message ("Loading...") is displayed.

* If `status` is `'error'`, the second message ("An error occurred...") is shown.

* If `status` is neither `'loading'` nor `'error'` (e.g., `'loaded'`), the fallback message ("Content has been successfully loaded\!") is shown.

# Performance Considerations

1. **Conditional Rendering and DOM Manipulation**: Vue uses **virtual DOM** to efficiently manage the changes. When an element is conditionally rendered with `v-if`, it is removed or added to the DOM based on the condition, which involves creating and destroying elements. This is fine for most cases, but if the condition changes frequently and the element is expensive to render, consider using **`v-show`** instead, which only toggles the element's visibility.

2. **`v-if` vs. `v-show`**:

   * **`v-if`**: Conditionally renders the element by adding/removing it from the DOM. It is **more expensive** in terms of performance if the condition changes frequently.

   * **`v-show`**: Always renders the element but toggles the **visibility** (via `display: none`). It is **more efficient** if you need to show/hide an element frequently.

3. Choose `v-if` for conditions that don’t change often and `v-show` for conditions that toggle frequently.

# Summary of `v-if`, `v-else-if`, and `v-else`:

* **`v-if`**: Used to conditionally render an element based on a Boolean expression. The element is only added to the DOM if the condition evaluates to `true`.

* **`v-else-if`**: Used to specify additional conditions to check if the previous `v-if` condition was `false`.

* **`v-else`**: Used to render a fallback element when none of the `v-if` or `v-else-if` conditions are met. It must immediately follow a `v-if` or `v-else-if` block.

By using these directives, you can control the visibility and rendering of elements in Vue based on dynamic data or conditions, providing more flexibility in your application’s UI.

