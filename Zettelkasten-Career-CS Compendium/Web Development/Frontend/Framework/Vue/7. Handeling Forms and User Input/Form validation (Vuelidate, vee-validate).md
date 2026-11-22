---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
Form validation is an essential part of any web application, ensuring that the data submitted by users is correct and meets specified criteria. In Vue.js, there are several libraries designed to simplify form validation, two of the most popular being **Vuelidate** and **vee-validate**.

Let's explore these two libraries and how they can be used for form validation in Vue.js.

---

# Vuelidate

**Vuelidate** is a lightweight, simple-to-use validation library for Vue.js. It allows you to define validation rules for your form fields using reactive data, and it provides an easy way to check if the form is valid or not.

## Installation of Vuelidate:

To use Vuelidate in your Vue.js project, you can install it via npm:

```
npm install @vuelidate/core
```

## Basic Example of Vuelidate

In this example, we will use **Vuelidate** to validate an input field that should be a non-empty text.

```vue
<template>
  <div>
    <form @submit.prevent="submitForm">
      <input
        v-model="name"
        type="text"
        placeholder="Enter your name"
        :class="{ 'is-invalid': !$v.name.$pending && !$v.name.$model }"
      />
      <div v-if="!$v.name.$pending && !$v.name.$model">
        <span class="error">Name is required.</span>
      </div>

      <button type="submit" :disabled="!$v.$anyDirty || !$v.$pending">Submit</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'

const name = ref('')

const rules = {
  name: { $pending: required },
}

const $v = useVuelidate(rules, { name })

const submitForm = () => {
  // Handle form submission
  console.log('Form submitted with name:', name.value)
}
</script>

<style>
.is-invalid {
  border: 1px solid red;
}
.error {
  color: red;
}
</style>
```

## Explanation:

* **`useVuelidate`**: This function is used to register the validation rules (`required` in this case) and bind them to the data (here, `name`).

* **Validation Rules**: The validation rule for the `name` field is `required`. If the field is empty, it will trigger the error.

* **Error Display**: If the validation fails (e.g., the field is empty), the `is-invalid` class is added to the input, and an error message is shown.

* **`$v.$pending`**: It’s a flag that tells whether the validation is still pending. This helps to avoid displaying error messages before the user has interacted with the field.

## Common Validators in Vuelidate:

* **`required`**: Checks if the field is not empty.

* **`minLength(n)`**: Checks if the input length is greater than or equal to `n`.

* **`maxLength(n)`**: Checks if the input length is less than or equal to `n`.

* **`email`**: Checks if the input is a valid email format.

* **`between(min, max)`**: Checks if the value is between `min` and `max`.

---

# vee-validate

**vee-validate** is another popular form validation library for Vue.js that provides a more declarative approach to form validation. It integrates nicely with Vue's reactivity and supports complex validations, asynchronous rules, and custom validation logic.

## Installation of vee-validate:

To use **vee-validate** in your Vue.js project, install it via npm:

```
npm install vee-validate
```

## Basic Example of vee-validate

Here is an example of using **vee-validate** to create a simple form with validation.

```vue
<template>
  <form @submit.prevent="submitForm">
    <div>
      <label for="name">Name</label>
      <Field
        id="name"
        name="name"
        v-model="name"
        :rules="required"
        placeholder="Enter your name"
      />
      <ErrorMessage name="name" />
    </div>

    <div>
      <label for="email">Email</label>
      <Field
        id="email"
        name="email"
        v-model="email"
        :rules="emailRule"
        placeholder="Enter your email"
      />
      <ErrorMessage name="email" />
    </div>

    <button type="submit" :disabled="!isValid">Submit</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import { useForm, Field, ErrorMessage } from 'vee-validate'
import { required, email } from '@vee-validate/rules'

const name = ref('')
const email = ref('')

const { isValid, submitForm } = useForm()

const emailRule = email
const required = required
</script>

<style>
/* Add custom styles here */
</style>
```

## Explanation:

* **`Field`**: The `Field` component is used to create a form input with validation rules attached to it. It binds the input value to the `v-model` property and applies validation rules (`required`, `email`).

* **`ErrorMessage`**: The `ErrorMessage` component automatically displays validation error messages for the corresponding field if the validation fails.

* **`useForm`**: This is the function that manages the form state, including tracking the validity of the form (`isValid`) and handling form submission.

* **Validation Rules**: You can define built-in validation rules like `required` or `email` from `@vee-validate/rules` and apply them directly to the fields.

* **Form Submission**: The form submission is handled with the `submitForm` function, which can be triggered only if the form is valid (checked via `isValid`).

## Common Features of vee-validate:

* **Custom Validation Rules**: You can define custom validation logic for fields.

* **Async Validation**: Supports asynchronous validation, useful for validating fields like email or username availability against a server.

* **Field-level Validation**: Allows validating individual fields separately, which is ideal for complex forms.

* **Form-Level Validation**: Easily manage the overall form validity and submit logic.

---

# Comparison Between Vuelidate and vee-validate

| Feature | Vuelidate | vee-validate |
| ----- | ----- | ----- |
| **Installation** | `npm install @vuelidate/core` | `npm install vee-validate` |
| **Validation Approach** | Imperative (set up validators on data properties) | Declarative (use components like `Field` and `ErrorMessage`) |
| **Custom Validators** | Supports custom validators via functions | Supports custom validation logic and rules |
| **Form Validation** | Works with reactive state for form validation | Supports both field-level and form-level validation |
| **Async Validation** | No built-in support for async validation | Built-in support for async validation (e.g., server-side checks) |
| **Integration** | Simple and lightweight integration | More feature-rich with complex form handling |
| **Ease of Use** | Easier to set up for small projects | More flexible and powerful for complex forms |

---

# Which One to Use?

* **Use Vuelidate** if:

  * You need a **simple and lightweight** validation solution.

  * You want to easily define validation rules directly on your component's reactive state.

  * You are working with smaller forms or want fine-grained control over validation.

* **Use vee-validate** if:

  * You need a **more powerful and declarative** validation solution.

  * You are working with complex forms or need features like **field-level validation**, **async validation**, or **form submission handling**.

  * You want an **out-of-the-box solution** for handling different types of inputs (text, select, checkboxes) with custom error messages and validation feedback.

---

# Conclusion

Both **Vuelidate** and **vee-validate** are excellent libraries for form validation in Vue.js, each with its strengths:

* **Vuelidate** is great for projects requiring simple, reactive validation.

* **vee-validate** is better suited for more complex forms with features like custom rules, async validation, and form-level handling.

Choose the one that best fits the complexity and requirements of your form validation needs.

