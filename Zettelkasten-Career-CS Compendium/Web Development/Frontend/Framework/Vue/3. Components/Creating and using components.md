---
tags:
  - Vue
---
In Vue.js, **components** are reusable, self-contained units of functionality or UI that allow you to break down your application into smaller, manageable pieces. Components can be anything from a simple button to a complex form or entire page layout.

# What is a Component?

A **component** in Vue.js is a Vue instance with its own template, logic, and style. Components allow you to encapsulate a piece of the user interface and make it reusable across different parts of your application.

# Creating a Component

There are multiple ways to create a component in Vue, including registering it globally or locally. Here’s how to create a simple component.

## Global Component

A global component can be used throughout your entire Vue instance. It’s registered once and can be accessed in any part of the app.

**Example:**

```javascript
// Registering a global component
Vue.component('my-component', {
  template: '<div>Hello from My Component!</div>'
});
```

You can now use the `my-component` in any template:

```vue
<div id="app">
  <my-component></my-component>
</div>
```

## Local Component

You can also define and use components locally inside another component. Local components are only available within the parent component where they’re defined.

**Example:**

```javascript
// Defining a local component within a Vue instance
new Vue({
  el: '#app',
  data: {
    message: 'Hello from Parent Component!'
  },
  components: {
    'my-component': {
      template: '<div>Hello from My Component!</div>'
    }
  }
});
```

This local component will be available only in the parent component's template.

```vue
<div id="app">
  <my-component></my-component>
</div>
```

# Component Structure

A component consists of three main parts:

1. **Template**: Defines the HTML structure of the component.

2. **Script**: Defines the logic, data, methods, and lifecycle hooks of the component.

3. **Style**: Defines the CSS (or styles) for the component (optional).

## Example of a Single File Component (SFC):

A Vue Single File Component (SFC) combines all these parts into one file with the `.vue` extension:

```vue
<!-- MyComponent.vue -->
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="changeMessage">Change Message</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, Vue Component!'
    };
  },
  methods: {
    changeMessage() {
      this.message = 'Message changed!';
    }
  }
};
</script>

<style scoped>
h1 {
  color: blue;
}
button {
  padding: 10px;
  background-color: #42b983;
  color: white;
}
</style>
```

# Using Components in Templates

Once you have created a component, you can use it inside other components by including it in the `components` option of the parent component. For example:

```javascript
new Vue({
  el: '#app',
  components: {
    'my-component': MyComponent  // Importing and registering the component
  }
});
```

Then, you can use it in your template:

```vue
<div id="app">
  <my-component></my-component>
</div>
```

This would render the `MyComponent` template inside the parent component.

# Props

Components can receive **props** (short for properties) from their parent component. Props are a way to pass data from the parent component to the child component.

## Passing Props:

In the parent component, you can pass data to a child component using `v-bind` (or just as an attribute in the template).

```vue
<my-component :message="parentMessage"></my-component>
```

## Defining Props in the Child Component:

In the child component, you define the props it can accept:

```javascript
Vue.component('my-component', {
  props: ['message'],
  template: '<div>{{ message }}</div>'
});
```

Now, the child component will display whatever data is passed from the parent’s `message` property.

# Emitting Events

Child components can communicate with their parent components by emitting custom events. The parent can listen for these events and respond accordingly.

## Emitting an Event from the Child Component:

```javascript
Vue.component('my-component', {
  template: '<button @click="emitEvent">Click Me</button>',
  methods: {
    emitEvent() {
      this.$emit('child-click', 'Hello from Child!');
    }
  }
});
```

## Listening for the Event in the Parent Component:

```vue
<my-component @child-click="handleChildClick"></my-component>
```

```javascript
new Vue({
  el: '#app',
  methods: {
    handleChildClick(message) {
      alert(message);  // Will show "Hello from Child!"
    }
  }
});
```

# Slots (Content Projection)

Slots allow you to pass content from a parent component into a child component, making the child component more flexible.

## Named Slot:

```vue
<!-- Parent Component -->
<child-component>
  <template v-slot:header>
    <h1>This is a custom header</h1>
  </template>
</child-component>
```

```vue
<!-- Child Component -->
<template>
  <div>
    <slot name="header"></slot>
    <p>Child content goes here.</p>
  </div>
</template>
```

In this example, the content inside the `<template v-slot:header>` tag in the parent component will be injected into the `slot` tag in the child component.

# Lifecycle Hooks in Components

Each Vue component has a set of **lifecycle hooks** that allow you to run code at different stages of the component’s lifecycle. These hooks are helpful for performing tasks such as fetching data or cleaning up when a component is destroyed.

Common lifecycle hooks include:

* `created`: Called after the component instance has been created.

* `mounted`: Called after the component has been added to the DOM.

* `updated`: Called after the component’s reactive data has been updated.

* `destroyed`: Called when the component is removed from the DOM.

Example:

```javascript
Vue.component('my-component', {
  data() {
    return {
      message: 'Hello!'
    };
  },
  created() {
    console.log('Component created');
  },
  mounted() {
    console.log('Component mounted');
  }
});
```

# Summary of Key Concepts:

* **Component Basics**: Vue components allow for reusable, encapsulated pieces of functionality.

* **Global vs. Local Components**: Global components can be used anywhere in the app, while local components are used only within the parent component.

* **Props**: Allow passing data from parent to child components.

* **Event Emitting**: Child components can communicate with parents via custom events.

* **Slots**: Allow you to pass content into child components dynamically.

* **Lifecycle Hooks**: Allow you to run code at specific stages of the component's life.

Components help structure your Vue application in a modular way, making it more maintainable, reusable, and organized.

