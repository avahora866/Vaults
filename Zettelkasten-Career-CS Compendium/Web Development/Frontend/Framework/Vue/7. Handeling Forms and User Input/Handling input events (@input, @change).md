---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js, input events like **`@input`** and **`@change`** are used to handle user interactions with form elements, such as text fields, checkboxes, and select options. These events allow you to **react to user inputs** and update your component's state accordingly.

Both **`@input`** and **`@change`** are commonly used for handling form input, but they behave differently and are suited for different use cases.

---

# `@input` Event

The **`@input`** event is fired **whenever the value of an input element changes**. It’s used for **immediate updates** to the model as the user types or interacts with the input. This is particularly useful for **real-time feedback** or **live updates** as the user interacts with the form element.

## How `@input` Works:

The `@input` event listens for input changes (such as keystrokes in a text field) and emits the new value whenever it changes.

### Example: Text Input with `@input`

```vue
<template>
  <div>
    <input type="text" :value="inputValue" @input="updateValue" placeholder="Type something..." />
    <p>You typed: {{ inputValue }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const inputValue = ref('')

const updateValue = (event) => {
  inputValue.value = event.target.value
}
</script>
```

*   
  **Explanation**:

  * The `inputValue` is bound to the input element using `:value`.

  * The `@input` event is fired whenever the user types in the input field.

  * The `updateValue` method updates `inputValue` with the latest value from the input.

## Use Cases for `@input`:

* **Real-time validation**: You can check input fields for correctness as the user types (e.g., email validation, password strength).

* **Instant feedback**: Displaying live results like a live search, text length counter, or autocomplete suggestions.

---

# `@change` Event

The **`@change`** event is fired when the value of an input element has **changed and the element loses focus**, or when a user **selects a new option** in a dropdown (`<select>`). Unlike `@input`, which triggers on each keystroke or interaction, the `@change` event is triggered only when the user finishes their interaction.

## How `@change` Works:

The `@change` event is useful for cases where you want to wait until the user has finished modifying the input value. It is typically used for **select dropdowns**, **checkboxes**, or **radio buttons**, and in situations where you want to handle input changes after the user confirms their action.

### Example: Checkbox with `@change`

```vue
<template>
  <div>
    <label>
      <input type="checkbox" :checked="isChecked" @change="handleCheckboxChange" />
      I agree to the terms and conditions
    </label>
    <p>{{ isChecked ? 'Checked' : 'Not checked' }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const isChecked = ref(false)

const handleCheckboxChange = (event) => {
  isChecked.value = event.target.checked
}
</script>
```

*   
  **Explanation**:

  * The `isChecked` state tracks whether the checkbox is checked.

  * The `@change` event is triggered when the checkbox state changes (checked or unchecked).

  * The `handleCheckboxChange` method updates the `isChecked` value based on the checkbox state.

### Example: `@change` with `select` Dropdown

```vue
<template>
  <div>
    <select @change="handleSelectChange" :value="selectedOption">
      <option value="option1">Option 1</option>
      <option value="option2">Option 2</option>
      <option value="option3">Option 3</option>
    </select>
    <p>Selected option: {{ selectedOption }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const selectedOption = ref('')

const handleSelectChange = (event) => {
  selectedOption.value = event.target.value
}
</script>
```

*   
  **Explanation**:

  * The `selectedOption` state tracks the selected option in the dropdown.

  * The `@change` event is triggered when the user selects a new option in the dropdown.

  * The `handleSelectChange` method updates the `selectedOption` with the selected value.

## Use Cases for `@change`:

* **Form submission**: Trigger an action only after a user finishes editing or making a selection, such as submitting a form or saving settings.

* **Select dropdowns and checkboxes**: These elements commonly use `@change` to detect when the value has changed and when you need to perform an action only after the user finishes interacting.

---

# Key Differences Between `@input` and `@change`

| Feature | `@input` | `@change` |
| ----- | ----- | ----- |
| **Triggered On** | Any change in the input (e.g., typing) | When the input loses focus or a selection is made |
| **Frequency** | Fired on every input change (e.g., keystroke) | Fired once after the input or selection is confirmed |
| **Use Cases** | Real-time updates (e.g., live search, validation) | Form submission, selection confirmation |
| **Typical Inputs** | Text fields, checkboxes, sliders, textareas | Select dropdowns, checkboxes, radio buttons |

---

# Using Both `@input` and `@change` Together

In some cases, you may want to use both `@input` and `@change` for the same input element. For example, you might want to update a value in real time (using `@input`), but only trigger a specific action when the user has finished their input (using `@change`).

## Example: Input Field with `@input` and `@change`

```vue
<template>
  <div>
    <input 
      type="text" 
      :value="inputValue" 
      @input="handleInput" 
      @change="handleChange" 
      placeholder="Type something..."
    />
    <p>Input value: {{ inputValue }}</p>
    <p>Change value: {{ changeValue }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const inputValue = ref('')
const changeValue = ref('')

const handleInput = (event) => {
  inputValue.value = event.target.value
}

const handleChange = (event) => {
  changeValue.value = event.target.value
}
</script>
```

*   
  **Explanation**:

  * The `@input` event updates `inputValue` in real-time as the user types.

  * The `@change` event updates `changeValue` only when the user finishes typing and the input element loses focus.

---

# Summary

* **`@input`** is used to handle real-time input changes and is fired as the user types or interacts with the input.

* **`@change`** is fired when the input loses focus or when a selection is made, and is used for handling final input changes after the user finishes interacting with the element.

* **Use `@input`** for real-time data binding and **`@change`** when you need to respond to the final value after the user confirms their action.

Both events are essential for handling form inputs effectively in Vue.js, and understanding when to use each will help you manage user interactions in your application more efficiently.

