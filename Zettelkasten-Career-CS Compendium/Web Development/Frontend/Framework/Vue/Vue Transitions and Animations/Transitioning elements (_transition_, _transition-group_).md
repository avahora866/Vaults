---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
Vue makes it super easy to add animations and transitions to elements entering, leaving, or changing in the DOM using two special components:

* `<transition>` – for **single elements or components**

* `<transition-group>` – for **lists of elements** (like `v-for`)

---

# `<transition>` — Animating a Single Element

The `<transition>` wrapper lets you apply classes at different stages of entering or leaving the DOM. Vue adds and removes these classes automatically.

## Example: Fade In/Out

```vue
<template>
  <div>
    <button @click="show = !show">Toggle</button>

    <transition name="fade">
      <p v-if="show">Hello, Vue Transition!</p>
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const show = ref(true)
</script>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
```

## How it works:

Vue applies these classes during transitions:

* `fade-enter-from` → starting state

* `fade-enter-active` → during transition

* `fade-enter-to` → end state

* `fade-leave-from`, `fade-leave-active`, `fade-leave-to` for leaving

---

# `<transition-group>` — Animating Lists

When you use `v-for` to render lists and want to animate their insertion/removal/movement, use `<transition-group>` instead of `<div>`.

## Example: Animating a List

```vue
<template>
  <div>
    <button @click="addItem">Add</button>

    <transition-group name="list" tag="ul">
      <li v-for="item in items" :key="item.id">{{ item.text }}</li>
    </transition-group>
  </div>
</template>

<script setup>
import { ref } from 'vue'

let id = 1
const items = ref([{ id: id++, text: 'Item 1' }])

function addItem() {
  items.value.push({ id: id++, text: `Item ${id}` })
}
</script>

<style>
.list-enter-active, .list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>
```

## Key Differences:

* `<transition>` wraps one element.

* `<transition-group>` wraps **multiple items** and needs a `key` for each one.

* You must specify a `tag` (like `ul`, `div`, etc.) for `<transition-group>` to render correctly.

---

# Custom JavaScript Hooks (Advanced)

You can use JS-based transitions instead of CSS by defining lifecycle hooks like:

```vue
<transition
  @before-enter="beforeEnter"
  @enter="enter"
  @leave="leave"
/>
```

---

# Summary

| Feature | `<transition>` | `<transition-group>` |
| ----- | ----- | ----- |
| Animates | Single element/component | Lists (`v-for`) |
| Needs `key`? | No | Yes |
| Tag required? | No | Yes (`<ul>`, `<div>`, etc.) |
| CSS classes applied? | Yes | Yes |
| Works with `v-if` | Yes | Not ideal (use `v-show` or control array) |

---

