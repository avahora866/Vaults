---
tags:
  - Vue
---
**Vue's `Teleport`** is a built-in feature that allows you to **render elements outside of the current component's DOM hierarchy**. It provides a way to move a part of your component's template to another location in the DOM, which can be especially useful for elements like **modals**, **tooltips**, **dropdowns**, and **pop-ups** that need to be rendered at a higher level in the DOM (outside of their parent component).

This can help in scenarios where:

* You want to **avoid overflow issues** in a container (e.g., a modal appearing outside a scrollable section).

* You need to render an element **outside the constraints** of the current component's scope (for accessibility, styling, or other reasons).

* You want to **separate concerns** by placing certain UI elements in a specific place in the DOM while keeping the logic and functionality tied to the current component.

---

# How Teleport Works

**`<Teleport>`** works by defining a target DOM element to which the content of the component should be rendered. This means you can decide where in the DOM the component's elements will appear, regardless of where the component itself is located in the component hierarchy.

# Basic Syntax

```vue
<template>
  <teleport to="body">
    <!-- Content to be rendered outside -->
    <div class="modal">
      <h2>Modal Content</h2>
      <p>This is a modal displayed outside the root component!</p>
    </div>
  </teleport>
</template>
```

*   
  **`<teleport>`** is the wrapper tag for the content you want to move.

 `to="body"` specifies that the content should be rendered inside the `<body>` tag of the page. You can specify any valid CSS selector, like `#app`, `#modal-container`, or even `body`.

---

# Key Features of Teleport

1. **Move Content to Any Location**:

   * The `to` attribute allows you to specify where the content should be rendered. This can be any valid CSS selector, such as a specific `div`, `body`, or even an element with an `id`.

    Example: `<teleport to="#modal-container">` will render the content into the element with the `id="modal-container"`.

2. **No Need to Move Data or Logic**:

   * The component's **data** and **methods** remain bound to the original component, even though the rendered DOM is moved to a new location. The logic and state are still tied to the original component.

3. **Works with Complex DOM Structures**:

   * Teleport works well with more complex UI elements like modals, tooltips, and menus that are often rendered outside the regular flow of their parent components but need to share state or functionality.

4. **Compatibility with Transition Effects**:

   * You can use **Vue’s transition system** with `<Teleport>`. This allows you to animate the content when it enters or leaves the DOM.

---

# Example: Modal Component with Teleport

Let's say we want to create a modal that is rendered outside of its parent component’s DOM hierarchy. The modal is triggered by a button, and its content will be teleported to a top-level `#modal-root` element.

## Modal Component

```vue
<template>
  <teleport to="#modal-root">
    <div v-if="isVisible" class="modal">
      <div class="modal-content">
        <h2>Modal Window</h2>
        <p>This content is rendered outside the parent component.</p>
        <button @click="closeModal">Close</button>
      </div>
    </div>
  </teleport>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true,
    };
  },
  methods: {
    closeModal() {
      this.isVisible = false;
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
}
</style>
```

## App Component

```vue
<template>
  <div id="app">
    <button @click="showModal = !showModal">Toggle Modal</button>
    <Modal v-if="showModal" />
  </div>
</template>

<script>
import Modal from './components/Modal.vue';

export default {
  components: {
    Modal,
  },
  data() {
    return {
      showModal: false,
    };
  },
};
</script>
```

## HTML (Public/index.html)

```vue
<body>
  <div id="app"></div>
  <div id="modal-root"></div>  <!-- Modal will be rendered here -->
</body>
```

---

# How It Works:

1. **Rendering Outside of Parent Component**:

    The modal content is inside the `<teleport>` tag, and the `to="#modal-root"` tells Vue to render the modal’s HTML inside the `#modal-root` element in the HTML body, even though it's part of the `Modal` component inside the Vue component tree.

2. **State and Logic Remain Intact**:

   * The modal’s **visibility** (`isVisible`) and logic (`closeModal` method) remain part of the `Modal` component, even though the HTML is moved outside the root component’s DOM.

3. **No Need for Prop Passing**:

   * The component does not need to pass data or emit events just for displaying the modal content in a different part of the DOM. All the state and methods remain localized to the `Modal` component.

---

# Use Cases for Teleport

* **Modals**: Modals often need to be rendered outside of the app’s main structure to avoid layout issues like scrolling or clipping within overflow containers.

* **Tooltips and Popovers**: Tooltips or popovers that may need to appear above other UI elements can benefit from being rendered outside the component’s normal flow.

* **Dropdowns and Menus**: Dropdowns or flyout menus often need to break free from a parent container’s overflow or boundaries.

---

# Teleport with Transitions

Vue's `<teleport>` works well with **transitions**. You can animate the entry and exit of the teleported content using Vue’s transition system:

```vue
<template>
  <teleport to="#modal-root">
    <transition name="fade">
      <div v-if="isVisible" class="modal">
        <div class="modal-content">
          <h2>Modal Window</h2>
          <button @click="closeModal">Close</button>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<style scoped>
.modal {
  /* modal styles */
}
.modal-content {
  /* content styles */
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
```

In this case, when the modal enters or leaves, the **fade transition** is applied to its visibility.

---

# Summary

* **`<teleport>`** allows you to render content outside the root component’s DOM hierarchy while keeping the logic and state inside the component.

* The `to` attribute defines where in the DOM the content will be rendered.

* It’s commonly used for **modals**, **tooltips**, **dropdowns**, and other UI elements that need to be positioned outside the regular component flow.

* It works seamlessly with Vue’s **transition system**, enabling smooth animations when entering or leaving the DOM.

