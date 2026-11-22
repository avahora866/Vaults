---
tags:
  - Vue
  - FORMAT
---
In Vue 3, the `defineExpose` function allows you to **explicitly expose methods or properties** from a component to its parent or to the outside world when you're using the **Composition API**. This is especially useful when working with child components and you want to make certain methods or properties accessible outside the component, without relying on event emission or `ref` access.

The `defineExpose` function provides better control over what methods or properties are made accessible from a child component to its parent, improving the encapsulation and maintainability of your components.

---

# Why Use `defineExpose`?

In Vue 3, when you use the Composition API, **methods and data** defined in the `setup()` function are not automatically accessible to the outside world, like they would be in the Options API with `methods` or `data`. This is because the `setup()` function is a private scope within the component. If you need to allow the parent component to call certain methods or access specific data in the child component, you can use `defineExpose`.

Using `defineExpose` prevents unnecessary methods and properties from being accessible outside the component, helping you maintain a clean and controlled API for your components.

---

# How `defineExpose` Works

The `defineExpose` function is used inside the `setup()` function of the child component to **explicitly define what should be accessible from the outside** (e.g., from a parent component or through a `ref`).

# Syntax

```javascript
defineExpose({
  method1,
  method2,
  property1
});
```

*   
  **`method1`, `method2`, `property1`**: These are the methods or properties you want to expose to the outside world (parent or ref).

* When the parent component accesses the child via `ref`, the exposed methods or properties will be available on the ref object.

---

# Basic Example: Exposing Functions with `defineExpose`

Let’s see how you can use `defineExpose` to expose a method from a child component to its parent.

## Child Component (Child.vue)

```vue
<template>
  <div>
    <button @click="incrementCounter">Increment Counter</button>
    <p>Counter: {{ counter }}</p>
  </div>
</template>

<script>
import { ref, defineExpose } from 'vue';

export default {
  setup() {
    const counter = ref(0);

    // Method to increment the counter
    const incrementCounter = () => {
      counter.value++;
    };

    // Expose incrementCounter function to the parent
    defineExpose({
      incrementCounter
    });

    return {
      counter,
      incrementCounter
    };
  }
};
</script>
```

## Parent Component (App.vue)

```vue
<template>
  <div>
    <Child ref="childComponent" />
    <button @click="callChildMethod">Call Child Method</button>
  </div>
</template>

<script>
import { ref } from 'vue';
import Child from './Child.vue';

export default {
  components: {
    Child
  },
  setup() {
    const childComponent = ref(null);

    // Call the method exposed by the child component
    const callChildMethod = () => {
      if (childComponent.value) {
        childComponent.value.incrementCounter();
      }
    };

    return {
      childComponent,
      callChildMethod
    };
  }
};
</script>
```

# How It Works:

1. **Child Component**:

   * The `incrementCounter` method is defined in the `setup()` function and is **exposed** using `defineExpose`. This makes it accessible from the parent component.

2. **Parent Component**:

   * The `ref` is used to get a reference to the child component (`childComponent`).

   * The parent can then access the `incrementCounter` method on the child component through `childComponent.value.incrementCounter()`.

By using `defineExpose`, the parent can directly interact with the child’s exposed methods, without having to emit events or pass data back and forth through props.

---

# Example: Exposing Multiple Methods

You can expose multiple methods or properties from the child component using `defineExpose`.

## Child Component (Child.vue)

```vue
<template>
  <div>
    <button @click="incrementCounter">Increment</button>
    <button @click="decrementCounter">Decrement</button>
    <p>Counter: {{ counter }}</p>
  </div>
</template>

<script>
import { ref, defineExpose } from 'vue';

export default {
  setup() {
    const counter = ref(0);

    const incrementCounter = () => {
      counter.value++;
    };

    const decrementCounter = () => {
      counter.value--;
    };

    // Expose both increment and decrement functions to the parent
    defineExpose({
      incrementCounter,
      decrementCounter
    });

    return {
      counter,
      incrementCounter,
      decrementCounter
    };
  }
};
</script>
```

## Parent Component (App.vue)

```vue
<template>
  <div>
    <Child ref="childComponent" />
    <button @click="incrementFromParent">Increment from Parent</button>
    <button @click="decrementFromParent">Decrement from Parent</button>
  </div>
</template>

<script>
import { ref } from 'vue';
import Child from './Child.vue';

export default {
  components: {
    Child
  },
  setup() {
    const childComponent = ref(null);

    const incrementFromParent = () => {
      if (childComponent.value) {
        childComponent.value.incrementCounter();
      }
    };

    const decrementFromParent = () => {
      if (childComponent.value) {
        childComponent.value.decrementCounter();
      }
    };

    return {
      childComponent,
      incrementFromParent,
      decrementFromParent
    };
  }
};
</script>
```

---

# How `defineExpose` Helps with Encapsulation

* **Encapsulation**: By default, methods and properties in the `setup()` function are not exposed outside the component. `defineExpose` allows you to explicitly define which methods or properties should be accessible to the parent.

* **Controlled Access**: This gives you fine-grained control over what the parent component can access. Without `defineExpose`, the parent can access everything in `setup()` through a `ref`, but with `defineExpose`, you limit the parent to only the methods and properties you expose.

* **Better Maintainability**: It improves the maintainability of your components by reducing unnecessary exposure of methods or internal state, following the principle of least privilege (only expose what's necessary).

---

# When to Use `defineExpose`

You would typically use `defineExpose` in cases where:

* You need to **allow direct interaction** with a child component from the parent (e.g., calling a method from the parent component).

* You want to **limit access** to certain methods or properties of a component and **hide others**.

* You're using **refs** to access child component methods directly, and you want a more controlled and explicit way of exposing specific functionality.

---

# Conclusion

In Vue 3, `defineExpose` is a useful tool in the Composition API for **explicitly exposing methods, properties, or functions** to the parent or outside the component. It helps you:

* Improve encapsulation by only exposing what is necessary.

* Maintain cleaner component APIs.

* Provide controlled access to child component logic from the parent.

By using `defineExpose`, you can better manage component interactions while keeping the logic well-encapsulated and maintainable.

