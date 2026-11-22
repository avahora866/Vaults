---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
In Vue.js, **props** are a mechanism for passing data from a **parent component** to a **child component**. This allows child components to be reusable with different data while maintaining a unidirectional data flow (from parent to child). Props make it easy to share information and customize child components.

# What Are Props?

Props are custom attributes that you can define on a child component to accept data from its parent component. The parent component sends data to the child through these props, and the child component can access and use that data in its template or logic.

# Passing Props from Parent to Child

In the parent component, you bind data to the child component using the prop name as an attribute:

```vue
<!-- ParentComponent.vue -->
<template>
  <div>
    <child-component :message="parentMessage"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentMessage: 'Hello from Parent!'
    };
  }
};
</script>
```

In this example, `parentMessage` is passed to the child component via the `message` prop.

# Receiving Props in Child Component

In the child component, you declare the props that it will accept in the `props` option. The name of the prop in the `props` array should match the name used by the parent when binding data.

```vue
<!-- ChildComponent.vue -->
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  props: ['message']
};
</script>
```

In this case, the child component receives the `message` prop and displays it in a `<p>` tag.

# Dynamic Prop Binding

You can bind dynamic values to props by using the `v-bind` directive (or shorthand `:`). This allows you to pass not just static values but also reactive data, computed properties, or other dynamic expressions.

Example:

```vue
<!-- ParentComponent.vue -->
<template>
  <div>
    <child-component :message="computedMessage"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  computed: {
    computedMessage() {
      return `Message at ${new Date().toLocaleTimeString()}`;
    }
  }
};
</script>
```

In this example, the `computedMessage` is dynamically generated, and each time it updates, the child component will automatically reflect the new value.

# Type Validation for Props

Vue allows you to specify the type of the prop to ensure the data passed to the child component matches the expected type. This is useful for catching potential errors.

For example, you can specify that a prop should be a string, number, or even a more complex type:

```javascript
props: {
  message: {
    type: String,
    required: true,  // This prop must be passed
    default: 'Default message',  // Default value if not passed
  },
  count: {
    type: Number,
    default: 0,
  }
}
```

*   
  **`type`**: Specifies the expected type of the prop.

* **`required`**: Specifies whether the prop is required to be passed.

* **`default`**: Provides a default value if the prop is not provided.

# Default Values for Props

You can define default values for props. This can be useful for ensuring that the component behaves as expected even if the parent does not provide a prop.

```javascript
props: {
  title: {
    type: String,
    default: 'Default Title'
  }
}
```

If no `title` prop is passed from the parent, the child will default to `"Default Title"`.

# Validation for Props

Vue also supports more advanced prop validation, such as using `validator` to check if a prop satisfies specific conditions:

```javascript
props: {
  age: {
    type: Number,
    validator(value) {
      // The value must be between 0 and 120
      if (value < 0 || value > 120) {
        console.warn('Invalid age');
        return false;
      }
      return true;
    }
  }
}
```

In this example, the `age` prop must be a number between 0 and 120\. If the validation fails, a warning is logged.

# Passing Multiple Props

You can pass multiple props to a component in the same way. Here’s how you might pass more than one prop from the parent:

```vue
<!-- ParentComponent.vue -->
<template>
  <div>
    <child-component :message="parentMessage" :count="counter"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentMessage: 'Hello from Parent!',
      counter: 42
    };
  }
};
</script>
```

In the child component, both `message` and `count` props are received:

```javascript
props: {
  message: String,
  count: Number
}
```

# Passing Complex Data (Objects and Arrays)

You can pass complex data types like arrays or objects as props:

```vue
<!-- ParentComponent.vue -->
<template>
  <child-component :items="items"></child-component>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      items: ['Apple', 'Banana', 'Cherry']
    };
  }
};
</script>
```

In the child component, you can access the `items` array prop:

```javascript
props: {
  items: Array
}
```

You can also pass an object:

```vue
<child-component :user="user"></child-component>
```

```javascript
props: {
  user: Object
}
```

# Using `v-bind` for Binding Multiple Props at Once

You can also pass an entire object of props to a component using `v-bind` with an object:

```vue
<child-component v-bind="parentProps"></child-component>
```

Where `parentProps` is an object in the parent component’s `data`:

```javascript
data() {
  return {
    parentProps: {
      message: 'Hello',
      count: 42
    }
  };
}
```

In the child component, the `props` option would define the expected props:

```javascript
props: ['message', 'count']
```

# Prop Default Values with Objects and Arrays

For objects and arrays, it's important to be careful when using default values. Vue uses **object references** and **arrays by reference**, so if you use a non-literal default (like an object or array), all instances of the component will share the same reference. To avoid this, define defaults using a function.

```javascript
props: {
  items: {
    type: Array,
    default: () => []
  }
}
```

This ensures each component gets its own separate array.

# Summary

* **Props** are used to pass data from a parent component to a child component in Vue.js.

* You can pass dynamic data, and Vue will update the child component automatically when the data changes.

* You can validate props using `type`, `required`, and `default`.

* Complex data types (arrays, objects) can be passed as props.

* Props help keep components reusable and maintain a unidirectional flow of data, which is a core concept in Vue’s reactivity system.

Props are a powerful feature for creating reusable and configurable components that can work with different data in different contexts.

