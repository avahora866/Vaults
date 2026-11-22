---
tags:
  - Vue
---
In Vue.js, **slots** provide a way to distribute content to child components from the parent component. This feature allows child components to be more flexible and reusable, as the parent can inject custom content into the child’s template, making the child component more dynamic.

# What Are Slots?

A **slot** is a placeholder in a child component’s template that allows the parent component to pass content into that placeholder. You can think of slots as a way to define "holes" in a component that can be filled with custom content from the parent.

Slots help make child components flexible, as they allow the parent to control what gets rendered inside the child component.

# Basic Slot

A basic slot is the simplest form of content distribution in Vue. You define a `<slot>` element in the child component, and the parent can place any content inside the child component's tags. That content will be inserted into the `<slot>`.

## Basic Slot Example:

**ChildComponent.vue** (Child Component):

```vue
<template>
  <div>
    <h2>This is the child component</h2>
    <slot></slot>  <!-- Content from the parent will be injected here -->
  </div>
</template>
```

**ParentComponent.vue** (Parent Component):

```vue
<template>
  <child-component>
    <p>This content comes from the parent!</p>
  </child-component>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
};
</script>
```

In this example:

* The `child-component` contains a `<slot>`.

* The parent passes a `<p>` tag as content to the `child-component`.

* This content is rendered inside the `<slot>` of the child component.

The result would be:

```vue
<div>
  <h2>This is the child component</h2>
  <p>This content comes from the parent!</p>
</div>
```

# Named Slots

You can have **multiple slots** in a child component, each identified by a unique name. This allows the parent to specify where different pieces of content should go in the child.

## Named Slot Example:

**ChildComponent.vue** (Child Component):

```vue
<template>
  <div>
    <h2>This is the child component</h2>
    <slot name="header"></slot>  <!-- Named slot -->
    <slot></slot>  <!-- Default slot -->
  </div>
</template>
```

**ParentComponent.vue** (Parent Component):

```vue
<template>
  <child-component>
    <template v-slot:header>
      <h1>This is a custom header</h1>
    </template>
    <p>This content is passed to the default slot</p>
  </child-component>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
};
</script>
```

In this example:

* The `child-component` has two slots: a default slot and a named slot (`header`).

* In the parent, the `v-slot:header` directive is used to pass content to the `header` slot, and default content is placed inside the default slot.

The result would be:

```vue
<div>
  <h2>This is the child component</h2>
  <h1>This is a custom header</h1> <!-- Content for the "header" slot -->
  <p>This content is passed to the default slot</p> <!-- Content for the default slot -->
</div>
```

# Scoped Slots

**Scoped slots** allow you to pass data from the child component to the content being injected by the parent. This is a powerful feature that enables more dynamic content distribution, where the parent has access to properties from the child component.

## Scoped Slot Example:

**ChildComponent.vue** (Child Component):

```vue
<template>
  <div>
    <slot :message="message"></slot>  <!-- Scoped slot passing data -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello from the child!'
    };
  }
};
</script>
```

**ParentComponent.vue** (Parent Component):

```vue
<template>
  <child-component>
    <!-- Using the scoped slot with v-slot -->
    <template v-slot:default="slotProps">
      <p>{{ slotProps.message }}</p>  <!-- Accessing data passed from the child -->
    </template>
  </child-component>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
};
</script>
```

In this example:

* The child component provides a scoped slot, passing the `message` data to the slot.

* The parent component uses `v-slot:default` to access the data and render it inside the content.

The result would be:

```vue
<div>
  <p>Hello from the child!</p>
</div>
```

Here, the parent can access the `message` passed from the child component and display it.

# Default Slot Content

You can provide default content for slots. If the parent does not pass any content to a slot, the default content is rendered instead.

## Default Slot Content Example:

**ChildComponent.vue** (Child Component):

```vue
<template>
  <div>
    <slot>Default content when no content is passed</slot>
  </div>
</template>
```

**ParentComponent.vue** (Parent Component):

```vue
<template>
  <child-component></child-component>  <!-- No content passed to the slot -->
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
};
</script>
```

If no content is passed by the parent, the default content (`Default content when no content is passed`) is displayed.

# Using `v-slot` Shorthand

Vue provides a shorthand syntax for `v-slot` when you're working with the default slot. Instead of writing `v-slot:default`, you can use the shorthand syntax.

## Shorthand Slot Syntax Example:

**ChildComponent.vue** (Child Component):

```vue
<template>
  <div>
    <slot></slot>  <!-- Default slot -->
  </div>
</template>
```

**ParentComponent.vue** (Parent Component):

```vue
<template>
  <child-component>
    <template #default>
      <p>This is content passed to the default slot</p>
    </template>
  </child-component>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
};
</script>
```

The `#default` is shorthand for `v-slot:default`.

# Summary of Slot Types

| Slot Type | Usage |
| ----- | ----- |
| **Default Slot** | Simple slot to inject content from parent. Use `<slot></slot>`. |
| **Named Slot** | Allows multiple slots with unique names. Use `<slot name="name"></slot>`. |
| **Scoped Slot** | Allows passing data from child to parent via a slot. Use `v-slot` syntax. |
| **Default Content** | Provides fallback content if the parent doesn't pass anything to the slot. |

# When to Use Slots

* Use **basic slots** when you want to allow the parent to inject any content into the child component.

* Use **named slots** when you want to allow the parent to inject content into specific sections of the child.

* Use **scoped slots** when the parent needs access to data from the child to render dynamic content based on that data.

Slots provide a powerful way to build flexible, reusable, and customizable components in Vue.js. They allow for better content distribution and offer a higher degree of flexibility in how components are used.

