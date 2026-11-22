---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js, **`v-model`** is a directive used for **two-way data binding**. It allows you to create a two-way binding between the data in your Vue component and the input elements in the DOM (like text fields, checkboxes, etc.). This means that when the input element's value changes, the associated data in Vue will automatically update, and vice versa.

# What is Two-Way Data Binding?

**Two-way data binding** means that changes in the **UI** (like a user typing in a text input or checking a checkbox) will automatically update the **Vue data model**, and changes in the **Vue data model** will automatically update the **UI**. This helps keep the DOM and the data in sync without needing to write extra code to handle updates.

# Basic Syntax of `v-model`

The basic syntax of `v-model` is:

```vue
<input v-model="dataProperty" />
```

*   
  **`v-model="dataProperty"`**: This binds the value of the input field to the `dataProperty` in the Vue component’s `data`. It automatically updates the `dataProperty` when the user interacts with the input field and also updates the input field when `dataProperty` changes.

# Example 1: Using `v-model` with Text Inputs

```vue
<div id="app">
  <input v-model="message" placeholder="Type something">
  <p>Message: {{ message }}</p>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    message: ''
  }
});
```

In this example:

* The value of the `<input>` element is bound to the `message` data property.

* When the user types in the input, the `message` property will automatically be updated with the input's value.

* Similarly, if you programmatically update `message`, the input field will automatically reflect the change.

# How `v-model` Works

`v-model` is essentially a shorthand for three things:

1. **Binding the value** of an element to a data property.

2. **Listening for input events** to update the data property whenever the user changes the value.

3. **Updating the element's value** when the data property changes.

It is essentially a combination of:

* **`v-bind`** to bind the element’s value to a data property.

* **`v-on:input`** to listen for input events and update the data property when the value changes.

# Example 2: Using `v-model` with a Checkbox

```vue
<div id="app">
  <label>
    <input type="checkbox" v-model="isChecked"> Check me
  </label>
  <p>Checked: {{ isChecked }}</p>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    isChecked: false
  }
});
```

In this example:

* The checkbox’s checked state is bound to the `isChecked` data property.

* When the checkbox is checked or unchecked, `isChecked` will be updated automatically.

* If `isChecked` changes programmatically, the checkbox will reflect the updated state.

# Example 3: Using `v-model` with Radio Buttons

```vue
<div id="app">
  <label>
    <input type="radio" value="A" v-model="picked"> A
  </label>
  <label>
    <input type="radio" value="B" v-model="picked"> B
  </label>
  <p>Picked: {{ picked }}</p>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    picked: 'A'
  }
});
```

In this example:

* The `v-model` binds the selected radio button value to the `picked` data property.

* When the user selects a radio button, the `picked` property will be updated with the value of the selected radio button (`'A'` or `'B'`).

* If `picked` changes programmatically, the corresponding radio button will automatically be selected.

# Example 4: Using `v-model` with Select Dropdown

```vue
<div id="app">
  <select v-model="selected">
    <option disabled value="">Please select one</option>
    <option>Apple</option>
    <option>Banana</option>
    <option>Cherry</option>
  </select>
  <p>Selected: {{ selected }}</p>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    selected: ''
  }
});
```

In this example:

* The `v-model` binds the selected option in the dropdown to the `selected` data property.

* When the user selects an option, the `selected` property will automatically update.

* If the `selected` value is updated programmatically, the dropdown will update accordingly.

# `v-model` Modifiers

`v-model` also supports **modifiers**, which can change its behavior for specific use cases.

## `lazy` Modifier:

By default, `v-model` listens for the `input` event. However, you can use the `lazy` modifier to make `v-model` only update the bound data when the **change event** is fired (typically when the user leaves the input field).

```vue
<input v-model.lazy="message">
```

This is useful when you want to capture the input value only after the user has finished editing (e.g., after they focus out of the field).

## `number` Modifier:

The `number` modifier tries to **convert the input value into a number** (instead of a string). It is often used for number inputs.

```vue
<input v-model.number="age" type="number">
```

In this example, the input value will automatically be converted to a number, so the `age` property will always be a number.

## `trim` Modifier:

The `trim` modifier automatically **removes leading and trailing whitespace** from the input value.

```vue
<input v-model.trim="message">
```

This is useful when you want to ensure that the value entered into the input doesn’t have unnecessary spaces around it.

# Custom `v-model` Bindings (With Components)

`v-model` is most commonly used with standard form elements, but it can also be used with custom components. By default, `v-model` works with a property called `value`, but you can customize the binding by specifying a prop and event.

## Example of `v-model` with a Custom Component:

```vue
<my-input v-model="message"></my-input>
```

In the custom component (`my-input`), the following must be set up:

* The prop that will hold the `v-model` value (usually `value` by default).

* The event to emit to update the parent component’s data (usually `input` by default).

```javascript
Vue.component('my-input', {
  props: ['value'],
  template: '<input :value="value" @input="$emit(\'input\', $event.target.value)">'
});
```

In this example:

* The `v-model` directive is bound to the `message` property in the parent component.

* The `my-input` component uses the `value` prop to bind the input value and emits the `input` event to update the parent’s `message` property.

# Conclusion

* **`v-model`** creates **two-way data binding** between an input element and a Vue data property.

* It listens for changes in the input (e.g., typing, checking a checkbox, selecting an option) and automatically updates the associated data property.

* It also updates the DOM element when the data property changes.

* **Modifiers** such as `lazy`, `number`, and `trim` help control the behavior of `v-model`.

* **Custom components** can use `v-model` too, by defining the correct prop and event to handle the data binding.

By using `v-model`, you can easily manage form inputs and keep the UI and data in sync, leading to more reactive and dynamic user interfaces.

