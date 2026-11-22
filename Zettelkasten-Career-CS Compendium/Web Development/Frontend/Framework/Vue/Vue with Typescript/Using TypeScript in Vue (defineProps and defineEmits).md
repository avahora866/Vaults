---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
Vue 3 provides **excellent TypeScript support** that works seamlessly with the Composition API. Using TypeScript with Vue ensures better type inference, safer code, and a more enjoyable development experience.

Two important helpers that Vue provides for TypeScript are **`defineProps`** and **`defineEmits`**. These are used to define and type the component's props and events in a clean and type-safe manner.

---

# `defineProps()` — Defining and Typing Props

* **`defineProps()`** is a special function used to define props for a component when using the Composition API.

* It helps with **type-checking** and **auto-completion** for props.

## Basic Usage

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

* The **`Props` interface** defines the expected types for the props.

* **`defineProps<Props>()`** ensures that the component receives props with the correct types.

* TypeScript will enforce type safety for props, helping with autocomplete and ensuring the correct data types are passed.

---

# `defineEmits()` — Defining and Typing Events

* **`defineEmits()`** is used to define custom events that your component will emit.

* It provides type safety for **emitting events** and ensures the correct payload type.

## Basic Usage

```vue
<script setup lang="ts">
import { defineEmits } from 'vue'

interface EmitEvents {
  (event: 'increment', payload: number): void
}

const emit = defineEmits<EmitEvents>()

// Trigger the 'increment' event
const increment = () => {
  emit('increment', 1)
}
</script>

<template>
  <button @click="increment">Increment</button>
</template>
```

## Explanation:

* **`EmitEvents` interface** defines the event names and their payload types. In this case, the `increment` event takes a `number` payload.

* **`defineEmits<EmitEvents>()`** ensures that only valid events with the correct payload are emitted.

* The `emit('increment', 1)` call will trigger the event and TypeScript will ensure the payload matches the type.

---

# Combining `defineProps()` and `defineEmits()`

You can use both `defineProps` and `defineEmits` together in a component, especially when dealing with both props and events.

## Example: A Counter Component

```vue
<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'

interface CounterProps {
  initialValue: number
}

interface CounterEvents {
  (event: 'update', newValue: number): void
}

const props = defineProps<CounterProps>()
const emit = defineEmits<CounterEvents>()

const count = ref(props.initialValue)

const increment = () => {
  count.value++
  emit('update', count.value)
}
</script>

<template>
  <p>{{ count }}</p>
  <button @click="increment">Increment</button>
</template>
```

## Explanation:

* `defineProps<CounterProps>()` makes sure that the `initialValue` prop is passed with the correct type (`number`).

* `defineEmits<CounterEvents>()` ensures that the `update` event is emitted with the correct payload (`newValue: number`).

* `increment()` increases the count and emits the updated value to the parent component.

---

# `defineProps()` and `defineEmits()` with Default Values and Validation

You can define default values and validation for props, which will be type-checked by TypeScript.

```vue
<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  message: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits<{
  (event: 'update', payload: number): void
}>()

const increment = () => {
  emit('update', props.count + 1)
}
</script>

<template>
  <p>{{ props.message }}</p>
  <p>{{ props.count }}</p>
  <button @click="increment">Increment</button>
</template>
```

## Explanation:

* `props.message` is a required `String`, and `props.count` has a **default value** of `0` if not provided.

* TypeScript will enforce these rules and provide autocomplete.

---

# Benefits of Using TypeScript with Vue

* **Auto-completion**: You'll get suggestions and completions for props, events, and their types.

* **Type Safety**: Prop and event payloads are type-checked, avoiding errors.

* **Improved Development Experience**: Reduced bugs, better tooling (e.g., VSCode IntelliSense), and cleaner code.

---

# Summary

| Feature | `defineProps` | `defineEmits` |
| ----- | ----- | ----- |
| Purpose | Define and type **props** | Define and type **events** |
| Type Definition | `defineProps<Type>()` | `defineEmits<Type>()` |
| Usage | Automatically inject props into component | Safely emit events with type-safe payloads |
| Example Syntax | `const props = defineProps<Props>()` | `const emit = defineEmits<EmitEvents>()` |

---

By using **`defineProps()`** and **`defineEmits()`** in Vue 3 with TypeScript, you ensure **type safety** for props and events, making your code more maintainable, predictable, and scalable.

