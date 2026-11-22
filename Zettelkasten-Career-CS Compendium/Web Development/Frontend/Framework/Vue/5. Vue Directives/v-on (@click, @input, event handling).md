---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js, `v-on` is a directive used to listen to DOM events and execute some JavaScript when those events are triggered. It is often used with event modifiers and can be shorthand using `@`. Let's go through how this works with common events like `@click` and `@input`.

# Basic Usage of `v-on` and `@` Shorthand

The `v-on` directive is used to bind an event to an element. The `@` symbol is simply a shorthand for `v-on`. For example:

* **Using `v-on`**:

```vue
<button v-on:click="handleClick">Click Me</button>
```

*   
  **Using `@` shorthand**:

```vue
<button @click="handleClick">Click Me</button>
```

Both of these will call the `handleClick` method when the button is clicked.

# Event Handling

When an event occurs (like a click or input), you can specify what method or function should run. For example:

```vue
<button @click="handleClick">Click Me</button>
```

```javascript
new Vue({
  el: '#app',
  methods: {
    handleClick() {
      alert('Button clicked!');
    }
  }
});
```

When the button is clicked, the `handleClick` method will be executed, showing an alert.

# Event Modifiers

Vue provides event modifiers to make it easier to handle common use cases like preventing the default behavior or stopping event propagation.

Here are some common modifiers:

* **`.prevent`**: Prevents the default behavior of the event (e.g., form submission).

```vue
<form @submit.prevent="handleSubmit">Submit</form>
```

*   
  **`.stop`**: Stops the event from propagating further up the DOM.

```vue
<button @click.stop="handleClick">Click Me</button>
```

*   
  **`.capture`**: Adds event listeners in capture mode, triggering before the event reaches the target element.

```vue
<button @click.capture="handleClick">Click Me</button>
```

*   
  **`.once`**: Makes the event listener trigger only once, then removes it.

```vue
<button @click.once="handleClick">Click Me</button>
```

*   
  **`.self`**: Only triggers if the event is fired on the element itself, not on its children.

```vue
<button @click.self="handleClick">Click Me</button>
```

# Handling Form Inputs with `@input`

The `@input` event is commonly used with form elements like `<input>`, `<textarea>`, and `<select>`. This event is fired whenever the user types in an input field or changes a selection.

```vue
<input @input="handleInput" v-model="message" />
```

In this case, the `@input` event is bound to the `handleInput` method, and the `v-model` directive binds the input value to the `message` data property.

# Accessing the Event Object

Sometimes, you may want to access the native event object itself. You can do this by passing `$event` to the method:

```vue
<button @click="handleClick($event)">Click Me</button>
```

```javascript
methods: {
  handleClick(event) {
    console.log(event);  // Logs the native click event object
  }
}
```

# Event Handling with Arguments

You can also pass additional arguments to the method:

```vue
<button @click="handleClick('Hello')">Click Me</button>
```

```javascript
methods: {
  handleClick(message) {
    console.log(message);  // Logs 'Hello'
  }
}
```

This is useful when you want to pass dynamic data or extra arguments when the event occurs.

# Summary

* `v-on` listens for DOM events and can be shortened to `@`.

* Common event handlers include `@click`, `@input`, `@submit`, etc.

* You can modify events with `.prevent`, `.stop`, `.capture`, `.once`, and `.self`.

* You can access the native event object using `$event` or pass custom arguments to event handlers.

This flexibility in Vue.js event handling makes it easy to manage complex user interactions in your applications.

