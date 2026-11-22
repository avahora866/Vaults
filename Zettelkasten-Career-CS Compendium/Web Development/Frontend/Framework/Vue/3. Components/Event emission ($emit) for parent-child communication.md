---
tags:
  - Vue
---
In Vue.js, `$emit` is used by **child components** to send **custom events** to their **parent components**. This is the standard way for children to communicate *upward* in the component hierarchy.

---

# Parent-Child Communication Recap

* **Parent → Child**: Use **props** to pass data *down*.

* **Child → Parent**: Use **`$emit`** to send events *up*.

---

# What is `$emit`?

The `$emit` method in a child component **triggers a custom event**. The parent can then **listen** for that event using `v-on` or `@` shorthand.

```javascript
this.$emit('event-name', payload)
```

*   
  `"event-name"`: The name of the event to emit.

* `payload`: Optional data you want to pass along with the event.

---

# Basic Usage of `$emit`

## ChildComponent.vue

```vue
<template>
  <button @click="notifyParent">Click Me</button>
</template>

<script>
export default {
  methods: {
    notifyParent() {
      this.$emit('childClicked'); // Emit event without data
    }
  }
}
</script>
```

## ParentComponent.vue

```vue
<template>
  <ChildComponent @childClicked="handleClick" />
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent },
  methods: {
    handleClick() {
      console.log('Child button clicked!');
    }
  }
}
</script>
```

---

# Passing Data with `$emit`

You can pass a payload (like a string, object, or number) when emitting an event.

## ChildComponent.vue

```vue
<template>
  <button @click="sendMessage">Send Message</button>
</template>

<script>
export default {
  methods: {
    sendMessage() {
      this.$emit('send-message', 'Hello from child!');
    }
  }
}
</script>
```

## ParentComponent.vue

```vue
<ChildComponent @send-message="receiveMessage" />

<script>
export default {
  methods: {
    receiveMessage(msg) {
      console.log(msg); // Output: Hello from child!
    }
  }
}
</script>
```

---

# Using `$event` in Template

When you use `$emit`, the payload becomes available in the parent via `$event`.

```vue
<ChildComponent @send-message="receiveMessage($event)" />
```

Or just:

```vue
<ChildComponent @send-message="receiveMessage" />
```

Both are valid if the method expects the event data.

---

# Emitting Complex Data (Objects, Arrays, etc.)

You can emit entire objects or arrays, too:

```javascript
this.$emit('update-user', { name: 'Jane', age: 30 });
```

Parent receives it just like any other function parameter.

---

# Custom Event Names

Event names are just strings—you can define your own:

```javascript
this.$emit('form-submitted')
```

Then in the parent:

```vue
<ChildComponent @form-submitted="onSubmit" />
```

Conventionally, event names are kebab-case (`form-submitted`), but you can use camelCase in JS if you prefer consistency.

---

# Best Practices

* Use clear, descriptive event names like `update-item`, `form-saved`, or `user-selected`.

* Keep emitted data minimal—only send what the parent needs.

* Use `defineEmits()` in Vue 3 composition API (optional but helpful).

---

# Vue 3 Composition API Example

In Vue 3, with the Composition API:

```vue
<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['send-message'])

function send() {
  emit('send-message', 'Hi from setup!');
}
</script>

<template>
  <button @click="send">Emit</button>
</template>
```

---

# Summary

| Direction | Method | Use Case |
| ----- | ----- | ----- |
| Parent → Child | `props` | Pass data down |
| Child → Parent | `$emit` | Notify parent about events |
| Child → Parent | `$emit('event', payload)` | Send event with optional data |


