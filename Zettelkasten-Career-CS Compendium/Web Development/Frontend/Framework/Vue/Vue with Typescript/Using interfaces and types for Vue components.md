---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue 3, TypeScript provides powerful tools to enhance your development workflow by offering **static typing** and **better tooling** for components, props, events, and more. By using **interfaces** and **types**, you can create more maintainable and reusable components with Vue's Composition API or Options API. Here's a deep dive into how to use interfaces and types in Vue components effectively.

---

# Typing Props Using Interfaces

Interfaces in TypeScript define the structure of an object. In Vue, we can use interfaces to define the expected types for props.

## Example: Typing Props with Interfaces

```vue
<script setup lang="ts">
import { defineProps } from 'vue'

// Define the structure of the prop object using an interface
interface User {
  name: string
  age: number
}

const props = defineProps<{
  user: User
}>()
</script>

<template>
  <h1>{{ props.user.name }}</h1>
  <p>{{ props.user.age }} years old</p>
</template>
```

## Explanation:

* **`User` Interface**: Defines a `user` object with `name` as a `string` and `age` as a `number`.

* **`defineProps<{ user: User }>()`**: This tells TypeScript that the component expects a prop named `user` that follows the structure defined in the `User` interface.

By using interfaces, we ensure that the prop follows the correct structure, and TypeScript will enforce type safety for it.

---

# Typing Events Using Interfaces

Custom events in Vue can be typed to ensure the correct event names and payloads are emitted. You can define the event types using TypeScript interfaces.

## Example: Typing Events with Interfaces

```vue
<script setup lang="ts">
import { defineEmits } from 'vue'

// Define the types of events and their payloads
interface EmitEvents {
  (event: 'update', newValue: string): void
  (event: 'delete', id: number): void
}

const emit = defineEmits<EmitEvents>()

const updateValue = () => {
  emit('update', 'New value')
}

const deleteItem = () => {
  emit('delete', 123)
}
</script>

<template>
  <button @click="updateValue">Update</button>
  <button @click="deleteItem">Delete</button>
</template>
```

## Explanation:

* **`EmitEvents` Interface**: Defines two custom events:

  * `update` event with a `string` payload.

  * `delete` event with a `number` payload.

* **`defineEmits<EmitEvents>()`**: This helps ensure that only valid events (`update` or `delete`) with the correct payload types can be emitted.

---

# Typing `ref`, `reactive`, and `computed`

You can use **interfaces** and **types** to define the types of reactive data (such as `ref`, `reactive`, or `computed`) in Vue components.

## Example: Typing `ref` with Interfaces

```vue
<script setup lang="ts">
import { ref } from 'vue'

// Define the type of the ref
interface Counter {
  count: number
}

const counter = ref<Counter>({ count: 0 })

const increment = () => {
  counter.value.count++
}
</script>

<template>
  <p>{{ counter.value.count }}</p>
  <button @click="increment">Increment</button>
</template>
```

## Explanation:

* **`Counter` Interface**: Defines the structure of the object stored inside the `ref`. In this case, the object has a `count` property of type `number`.

* **`ref<Counter>`**: Tells TypeScript that the `counter` ref will store an object that matches the `Counter` interface. The ref's value will be typed accordingly.

---

# Typing the Component with `defineComponent`

When using the **Options API** or for complex components in the **Composition API**, you can use **`defineComponent`** to explicitly type the component.

## Example: Typing a Component Using `defineComponent`

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
    increment() {
      this.$emit('increment', this.count + 1)
    }
  }
})
```

## Explanation:

* **`props`**: TypeScript doesn't need explicit typing in the object because the `type` is provided (`String` and `Number`).

* **`defineComponent`**: This method explicitly tells TypeScript the shape of your component's options (`props`, `emits`, `methods`, etc.). It will auto-infer the types and provide autocomplete.

---

# Typing Computed and Watchers

You can use **types** and **interfaces** to define the type of data in **computed** properties and **watchers**.

## Example: Typing a Computed Property

```vue
<script setup lang="ts">
import { computed } from 'vue'

const name = 'John'
const greeting = computed<string>(() => `Hello, ${name}`)

</script>

<template>
  <p>{{ greeting }}</p>
</template>
```

## Explanation:

* **`computed<string>`**: This explicitly tells TypeScript that the computed property `greeting` will return a `string`.

---

# Typing Components with Props, Events, and Methods

For more complex components, you can use **interfaces** and **types** for **props**, **events**, and **methods**.

## Example: Fully Typed Component

```vue
<script lang="ts">
import { defineComponent } from 'vue'

interface Props {
  name: string
  age: number
}

interface EmitEvents {
  (event: 'update', newValue: string): void
}

export default defineComponent({
  props: {
    name: {
      type: String,
      required: true
    },
    age: {
      type: Number,
      required: true
    }
  },
  emits: ['update'],
  methods: {
    updateName(newName: string) {
      this.$emit('update', newName)
    }
  }
})
</script>
```

## Explanation:

* **`Props` Interface**: Defines the types for the `name` and `age` props.

* **`EmitEvents` Interface**: Defines the events (`update`) and their associated payloads.

* **`defineComponent`**: Uses TypeScript interfaces to define and type-check the `props`, `emits`, and `methods`.

---

# Typing Default Values for Props

In Vue, props can have default values. You can define the type for these props and ensure their default values match the expected types.

## Example: Default Prop Typing

```vue
<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
  title: {
    type: String,
    default: 'Default Title'
  },
  count: {
    type: Number,
    default: 0
  }
})
</script>

<template>
  <h1>{{ props.title }}</h1>
  <p>{{ props.count }}</p>
</template>
```

## Explanation:

* TypeScript will automatically infer that `title` is a `String` and `count` is a `Number`, while also respecting the default values.

---

# Summary

| Feature | Usage | Explanation |
| ----- | ----- | ----- |
| **Props** | `defineProps<Type>()` | Defines the type of props passed to the component |
| **Events** | `defineEmits<Type>()` | Defines the type of events the component can emit |
| **Component Methods** | `defineComponent({ methods: { ... }})` | Types methods within the component |
| **Reactive Data** | `ref<Type>()`, `reactive<Type>()`, `computed<Type>()` | Types reactive data (e.g., `ref`, `reactive`) |
| **Default Props** | Typing with default values (`type: String, default: 'value'`) | Defines the type for props with default values |
| **Typing Methods** | `methods: { increment(): void }` | Defines the type of methods in the component |
| **Complex Components** | Combining props, events, methods, and types | Uses `defineComponent` with interfaces for complex components |

---

By using **TypeScript** interfaces and types, you can ensure that your Vue components are well-typed, reducing the risk of runtime errors and making your code more predictable and easier to maintain.

