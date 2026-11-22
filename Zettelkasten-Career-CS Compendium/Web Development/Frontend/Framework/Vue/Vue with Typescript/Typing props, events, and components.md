---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
When using TypeScript in Vue 3, you can **type props**, **events**, and even **the component itself** to enhance type safety, provide better IntelliSense support, and catch potential errors at compile-time. Let's dive into how to type these elements in your Vue components.

---

# Typing Props

Props are the primary way for a component to receive data from its parent. In TypeScript, you can **type props** to ensure the correct data is passed and to take advantage of IntelliSense and type checking.

## Basic Syntax for Typing Props

```vue
<script setup lang="ts">
import { defineProps } from 'vue'

interface Props {
  message: string
  count: number
}

const props = defineProps<Props>()
</script>

<template>
  <p>{{ props.message }}</p>
  <p>{{ props.count }}</p>
</template>
```

## Explanation:

* **`interface Props`**: This defines the types for the props the component expects.

  * `message` should be a `string`.

  * `count` should be a `number`.

* **`defineProps<Props>()`**: TypeScript uses this function to define and type-check the props in your component.

* This way, TypeScript will infer the correct types and validate that only a `string` and `number` are passed to the component.

---

# Typing Events (`defineEmits`)

In Vue 3, you can define custom events that the component will emit. With TypeScript, you can **type these events** to ensure the correct event names and payload types are used.

## Basic Syntax for Typing Events

```vue
<script setup lang="ts">
import { defineEmits } from 'vue'

interface EmitEvents {
  (event: 'increment', payload: number): void
  (event: 'decrement', payload: number): void
}

const emit = defineEmits<EmitEvents>()

const increment = () => {
  emit('increment', 1)
}

const decrement = () => {
  emit('decrement', 1)
}
</script>

<template>
  <button @click="increment">Increment</button>
  <button @click="decrement">Decrement</button>
</template>
```

## Explanation:

* **`interface EmitEvents`**: Defines the events and their associated payloads.

  * `increment` and `decrement` are two events, each with a payload of type `number`.

* **`defineEmits<EmitEvents>()`**: This ensures that when you call `emit()`, the event name and payload are type-checked.

* If you try to emit an event with an incorrect type or name, TypeScript will give you an error, ensuring that only the defined events are emitted with the right types.

---

# Typing Components (`defineComponent` and `defineProps` with Types)

For components that are not using the `<script setup>` syntax, you can still use TypeScript with the traditional `Options API`. You can type props, events, and other options using `defineComponent`.

## Using `defineComponent` with Types

```vue
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    message: {
      type: String,
      required: true
    },
    count: {
      type: Number,
      default: 0
    }
  },
  emits: ['increment', 'decrement'],
  methods: {
    increment() {
      this.$emit('increment', 1)
    },
    decrement() {
      this.$emit('decrement', 1)
    }
  }
})
</script>
```

## Explanation:

* **`defineComponent`**: This is used to define the component and its options. It also allows you to type props, events, and methods.

* **Props**: The `message` prop is a `String`, and the `count` prop is a `Number` with a default value.

* **Emits**: You specify the event names under `emits` and define the expected types in the methods.

* However, this approach is more verbose than using `defineProps` and `defineEmits` with the `<script setup>` syntax.

---

# Typing Component Props and Events with `defineProps` and `defineEmits` (Advanced)

You can also use **TypeScript types** to make your components even more reusable and flexible.

## Advanced Example: Typing Complex Props and Events

```vue
<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'

// Typing a prop as an object with specific properties
interface User {
  name: string
  age: number
}

const props = defineProps<{
  user: User
}>()

// Typing events with different payloads
interface EmitEvents {
  (event: 'updateUser', user: User): void
}

const emit = defineEmits<EmitEvents>()

// Function to update user and emit event
const updateUser = () => {
  const updatedUser = { name: 'Alice', age: 30 }
  emit('updateUser', updatedUser)
}
</script>

<template>
  <div>
    <h2>{{ props.user.name }} ({{ props.user.age }} years old)</h2>
    <button @click="updateUser">Update User</button>
  </div>
</template>
```

## Explanation:

* **`User` interface**: Used to type the `user` prop, which expects an object with `name` and `age` properties, both of type `string` and `number` respectively.

* **`EmitEvents` interface**: Defines the `updateUser` event, which expects a `user` payload matching the `User` interface.

* **Typing complex objects**: This pattern ensures that your components are highly reusable and that both props and events are fully type-checked.

---

# Typing the Whole Component (`defineComponent` with TypeScript)

In some cases, you may need to type the entire component, including props, events, and methods, when using Vue's traditional **Options API**.

## Component Typing with `defineComponent`

```javascript
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    title: {
      type: String,
      required: true
    },
    count: {
      type: Number,
      default: 0
    }
  },
  emits: ['increment'],
  methods: {
    increment(): void {
      this.$emit('increment', this.count + 1)
    }
  }
})
```

## Explanation:

* **Type for `increment` event**: The event's payload is inferred and type-checked based on the `count` prop.

* This approach is helpful if you're working with larger, more complex components and want full flexibility with TypeScript.

---

# Summary

| Feature | `defineProps()` | `defineEmits()` | `defineComponent()` |
| ----- | ----- | ----- | ----- |
| **Props** | Use `defineProps<Type>()` to define and type props |  | Use `props` option and type manually |
| **Events** |  | Use `defineEmits<Type>()` to define events | Use `emits` option, type events manually |
| **Typing Complex Types** | Use TypeScript interfaces for complex objects | Use TypeScript interfaces for event payloads | Use TypeScript types for props and events |

---

By using TypeScript, **`defineProps`** and **`defineEmits`** make your Vue components type-safe, and ensure props and events are correctly handled. This greatly improves your development experience by catching potential issues early and providing better code completion and error checking.

