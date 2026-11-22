---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js, **two-way binding** allows you to bind data between the component's state and the DOM (typically input elements) in such a way that changes to the input automatically update the data, and changes to the data automatically update the input. This behavior is typically used for **forms** and **user inputs**.

Vue's **`v-model`** directive is the most common way to achieve two-way data binding. It is used to bind form input elements (like `<input>`, `<textarea>`, and `<select>`) to a piece of state in your component.

---

# How `v-model` Works

1. **From DOM to Data**: When a user interacts with a form input element (like typing in a text field), the value entered will automatically update the data in the component.

2. **From Data to DOM**: When the data in the component changes, the value of the form input will automatically update to reflect the new data.

---

# Basic Example of `v-model`

In this example, a simple **input field** is bound to a piece of **state** called `message`.

```vue
<template>
  <div>
    <input v-model="message" type="text" placeholder="Type something..."/>
    <p>You typed: {{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('')  // A reactive piece of state
</script>
```

*   
  The `v-model="message"` directive binds the input field to the `message` variable.

* Any changes in the input field automatically update `message`, and any changes to `message` will automatically update the input field.

---

# How Does `v-model` Work Behind the Scenes?

The **`v-model`** directive is actually a shorthand for:

```vue
<input
  :value="message"
  @input="message = $event"
/>
```

*   
  **`value`**: The input field's value is bound to the component's `message`.

* **`@input`**: When the input event occurs (e.g., typing), it updates the `message` data with the new value. The `$event` in this case is the value of the input field.

---

# `v-model` with Different Input Types

You can use `v-model` with different types of form inputs.

## Text Input

```vue
<template>
  <input v-model="text" placeholder="Enter text" />
  <p>{{ text }}</p>
</template>

<script setup>
import { ref } from 'vue'

const text = ref('')
</script>
```

## Checkbox

For checkboxes, `v-model` works in a boolean context, binding the checkbox to a `true` or `false` value.

```vue
<template>
  <label>
    <input type="checkbox" v-model="isChecked" />
    I agree to the terms and conditions
  </label>
  <p>{{ isChecked }}</p>
</template>

<script setup>
import { ref } from 'vue'

const isChecked = ref(false)
</script>
```

*   
  When the checkbox is checked, `isChecked` becomes `true`. When unchecked, it becomes `false`.

## Radio Button

For radio buttons, `v-model` binds to the selected value.

```vue
<template>
  <label>
    <input type="radio" value="male" v-model="gender" />
    Male
  </label>
  <label>
    <input type="radio" value="female" v-model="gender" />
    Female
  </label>
  <p>Selected gender: {{ gender }}</p>
</template>

<script setup>
import { ref } from 'vue'

const gender = ref('')
</script>
```

*   
  Here, `gender` will be set to `'male'` or `'female'`, depending on which radio button is selected.

## Select Dropdown

For a `<select>` dropdown, `v-model` binds to the selected option.

```vue
<template>
  <select v-model="selectedOption">
    <option disabled value="">Please select an option</option>
    <option value="option1">Option 1</option>
    <option value="option2">Option 2</option>
    <option value="option3">Option 3</option>
  </select>
  <p>Selected option: {{ selectedOption }}</p>
</template>

<script setup>
import { ref } from 'vue'

const selectedOption = ref('')
</script>
```

*   
  In this case, the value of `selectedOption` will be set to the value of the selected `<option>`.

---

# Custom Components with `v-model`

Vue’s `v-model` can also be used with **custom components**. To do this, you need to handle the model’s behavior inside the custom component by emitting the `update:modelValue` event.

## Custom Input Component

In the custom component, you emit an event whenever the value changes:

```vue
<!-- CustomInput.vue -->
<template>
  <input :value="modelValue" @input="$emit('update:modelValue', $event)" />
</template>

<script setup>
defineProps({
  modelValue: String
})
</script>
```

*   
  **`modelValue`**: The prop that receives the current value.

* **`@input`**: The input event emits the updated value back to the parent with `update:modelValue`.

## Using the Custom Component with `v-model`

Now, in the parent component, you can use this custom component with `v-model` like this:

```vue
<template>
  <CustomInput v-model="message" />
  <p>You typed: {{ message }}</p>
</template>

<script setup>
import { ref } from 'vue'
import CustomInput from './CustomInput.vue'

const message = ref('')
</script>
```

*   
  When the user types in the custom input, the `message` in the parent will automatically update.

---

# Named `v-model` (Multiple `v-model`s)

Vue 3 allows you to have multiple `v-model` bindings in the same component by **naming** them. This is useful when a component has more than one piece of state that needs to be bound.

## Example with Multiple `v-model`s

```vue
<template>
  <CustomInput v-model:title="title" v-model:content="content" />
</template>

<script setup>
import { ref } from 'vue'
import CustomInput from './CustomInput.vue'

const title = ref('My title')
const content = ref('This is the content')
</script>
```

## Custom Input with Multiple Models

In the custom component, you handle multiple `v-model` bindings like this:

```vue
<template>
  <input :value="modelValueTitle" @input="$emit('update:title', $event)" />
  <textarea :value="modelValueContent" @input="$emit('update:content', $event)"></textarea>
</template>

<script setup>
defineProps({
  modelValueTitle: String,
  modelValueContent: String
})
</script>
```

*   
  Here, `v-model:title` binds to `modelValueTitle`, and `v-model:content` binds to `modelValueContent`.

---

# Summary of Key Points

* **`v-model`** is used for **two-way binding** of form input elements to component state.

* **Basic binding**: `v-model` automatically binds an input element to a reactive data property.

* **Custom components**: You can use `v-model` with custom components by emitting the `update:modelValue` event.

* **Multiple `v-model` bindings**: Vue 3 allows multiple `v-model` bindings with **named models**.

`v-model` makes it easy to manage form elements and sync them with the component state in Vue, providing a straightforward way to handle user input and state changes.

