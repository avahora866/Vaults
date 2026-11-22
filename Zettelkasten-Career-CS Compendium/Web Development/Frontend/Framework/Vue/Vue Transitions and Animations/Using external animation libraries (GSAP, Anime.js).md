---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
Vue works really well with animation libraries like **GSAP** and **Anime.js** when you need **advanced animations** beyond what CSS and Vue transitions can offer.

You can hook these libraries into Vue's `<transition>` or lifecycle hooks like `mounted()`, `watchEffect()`, or directly inside a component method.

---

# Popular Libraries

## GSAP (GreenSock Animation Platform)

* Powerful, fast, and widely used in professional animation workflows.

* Works great with Vue for timeline-based and coordinated animations.

## Anime.js

* Lightweight, flexible library that works well for simple UI effects and SVG animations.

---

# Example 1: Animating with GSAP \+ `<transition>`

```vue
<template>
  <button @click="show = !show">Toggle</button>
  <transition @enter="onEnter" @leave="onLeave">
    <div v-if="show" class="box">Hello GSAP</div>
  </transition>
</template>

<script setup>
import { ref } from 'vue'
import gsap from 'gsap'

const show = ref(true)

function onEnter(el, done) {
  gsap.fromTo(el, { opacity: 0, y: -20 }, { opacity: 1, y: 0, duration: 0.5, onComplete: done })
}

function onLeave(el, done) {
  gsap.to(el, { opacity: 0, y: -20, duration: 0.5, onComplete: done })
}
</script>

<style>
.box {
  width: 200px;
  background: #42b983;
  color: white;
  padding: 20px;
  margin-top: 10px;
}
</style>
```

---

# Example 2: Anime.js in Vue Lifecycle

```vue
<template>
  <div ref="circle" class="circle"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import anime from 'animejs/lib/anime.es.js'

const circle = ref(null)

onMounted(() => {
  anime({
    targets: circle.value,
    translateX: 250,
    rotate: '1turn',
    backgroundColor: '#FF6F61',
    duration: 2000
  })
})
</script>

<style>
.circle {
  width: 100px;
  height: 100px;
  background: teal;
  border-radius: 50%;
}
</style>
```

---

# Where to Use These Libraries in Vue

| Vue Feature | Ideal Use Case |
| ----- | ----- |
| `@enter`, `@leave` on `<transition>` | Smooth enter/exit animations |
| `onMounted()` | Animating elements on load |
| `watch()` / `watchEffect()` | Reactively animate changes |
| Template refs (`ref="box"`) | Target DOM elements directly |

---

# Tips

* **Use `ref`** to target elements in Vue 3 (`<div ref="el" />`).

* **Call `done()`** in transition hooks to let Vue know animation is complete.

* Both libraries work great with SVG, canvas, and more complex scenes.

* For global animations (page transitions, route changes), use Vue Router’s hooks \+ GSAP.

---

# Want to Try?

I can show you how to:

* Animate route transitions with GSAP

* Build an animated list (like a to-do app)

* Use Anime.js with a scroll-triggered animation

