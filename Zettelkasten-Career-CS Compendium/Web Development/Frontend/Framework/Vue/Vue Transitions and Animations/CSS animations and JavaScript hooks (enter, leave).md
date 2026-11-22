---
tags:
  - Vue
---
Vue provides a flexible transition system that works with both **CSS animations/transitions** and **JavaScript hooks**. This is useful when you want fine-grained control over how elements enter or leave the DOM.

---

# CSS Animations in Vue Transitions

Vue automatically adds transition classes that you can target in your CSS to animate elements in and out.

## Lifecycle Classes Vue Applies:

| State | Class Name |
| ----- | ----- |
| Enter Start | `v-enter-from` |
| Enter Active | `v-enter-active` |
| Enter End | `v-enter-to` |
| Leave Start | `v-leave-from` |
| Leave Active | `v-leave-active` |
| Leave End | `v-leave-to` |

If you use a `name="fade"`, the prefix `fade-` is added to each class above.

## Example: CSS Transition

```vue
<template>
  <button @click="show = !show">Toggle</button>
  <transition name="fade">
    <p v-if="show">I fade in and out!</p>
  </transition>
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

---

# JavaScript Hooks for Custom Animations

Vue also lets you define **custom JS transition hooks** for more control over the animation timing, like using `requestAnimationFrame`, `setTimeout`, or animation libraries.

## Available JavaScript Hooks:

* `beforeEnter(el)`

* `enter(el, done)`

* `afterEnter(el)`

* `enterCancelled(el)`

* `beforeLeave(el)`

* `leave(el, done)`

* `afterLeave(el)`

* `leaveCancelled(el)`

You must call the `done()` callback to signal when the animation ends (unless using CSS).

---

## Example: JavaScript Hook for Enter/Leave

```vue
<template>
  <button @click="show = !show">Toggle</button>
  <transition
    @before-enter="beforeEnter"
    @enter="enter"
    @leave="leave"
  >
    <p v-if="show" ref="box">JS Hook Animation</p>
  </transition>
</template>

<script setup>
import { ref } from 'vue'
const show = ref(true)

function beforeEnter(el) {
  el.style.opacity = 0
  el.style.transform = 'translateY(-20px)'
}

function enter(el, done) {
  setTimeout(() => {
    el.style.transition = 'all 0.5s ease'
    el.style.opacity = 1
    el.style.transform = 'translateY(0)'
    setTimeout(done, 500)
  })
}

function leave(el, done) {
  el.style.transition = 'all 0.5s ease'
  el.style.opacity = 0
  el.style.transform = 'translateY(-20px)'
  setTimeout(done, 500)
}
</script>
```

---

# When to Use JavaScript Hooks

Use JS hooks when:

* You need to coordinate with third-party animation libraries (like GSAP).

* You want **dynamic animation durations**.

* You’re animating properties not easily handled by CSS.

---

# CSS vs JavaScript Transitions: Quick Compare

| Feature | CSS Animation | JS Hooks |
| ----- | ----- | ----- |
| Easy to use | ✅ | ⚠️ more setup |
| Uses classes | ✅ | ❌ |
| Supports animation libs | ❌ (manual integration) | ✅ |
| Control over timing | ⚠️ limited | ✅ total control |
| Must call `done()` | ❌ | ✅ (except with `css: false`) |

---

