---
tags:
  - Vue
---
Snapshot testing is a powerful feature that allows you to ensure the structure of your components or outputs remains consistent over time. **Jest** offers built-in support for snapshot testing, which helps you automatically verify the output of a component (or any JavaScript object) matches a stored snapshot. If the output changes, Jest will flag it, allowing you to decide whether to accept the new output or keep the old one.

In the context of Vue.js and **Vue Test Utils**, snapshot tests can be used to compare the rendered output of Vue components to previously saved snapshots.

---

# What is Snapshot Testing?

Snapshot testing is a type of test where you:

1. Render a component.

2. Serialize its output (usually HTML, but it can be any JavaScript object).

3. Save that serialized output as a snapshot file.

4. On subsequent test runs, compare the current output to the saved snapshot.

5. If the output has changed, Jest will report a "mismatch," and you can choose to update the snapshot.

This ensures that the rendered output remains the same unless a deliberate change is made.

---

# Setting Up Snapshot Testing in Vue

## Prerequisites

1. **Vue Test Utils**: To mount Vue components and simulate rendering.

2. **Jest**: For running the tests and handling snapshot comparison.

Ensure that Jest and Vue Test Utils are installed in your project.

```
npm install --save-dev @vue/test-utils jest
```

---

# Basic Snapshot Testing Example

Let's walk through a simple example of snapshot testing for a Vue component.

## Step 1: Create a Simple Component

Consider this simple Vue component:

```vue
<!-- HelloWorld.vue -->
<template>
  <div>
    <h1>{{ greeting }} {{ name }}!</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      greeting: 'Hello',
      name: 'Vue',
    };
  },
};
</script>
```

## Step 2: Write the Snapshot Test

You can write a snapshot test for this component. When the component is mounted, we capture its rendered output and compare it to the saved snapshot.

```javascript
// HelloWorld.spec.js
import { mount } from '@vue/test-utils';
import HelloWorld from '@/components/HelloWorld.vue';

describe('HelloWorld.vue', () => {
  it('matches the snapshot', () => {
    const wrapper = mount(HelloWorld);  // Mount the component
    expect(wrapper.html()).toMatchSnapshot();  // Compare HTML to saved snapshot
  });
});
```

## Explanation:

* **`mount(HelloWorld)`**: Mounts the `HelloWorld` component.

* **`wrapper.html()`**: Gets the rendered HTML of the component.

* **`toMatchSnapshot()`**: Jest's built-in matcher that checks if the HTML matches the saved snapshot.

## Step 3: Run the Test

When you run the test, Jest will automatically create a snapshot file if it doesn't already exist. You can run the test with:

```
npm run test:unit
```

After running the test for the first time, Jest will generate a snapshot file and store it in a `__snapshots__` directory next to your test file. The file might look like this:

```
__snapshots__/HelloWorld.spec.js.snap
```

This file will look something like this:

```javascript
// HelloWorld.spec.js.snap
exports[`HelloWorld.vue matches the snapshot 1`] = `
<div>
  <h1>Hello Vue!</h1>
</div>
`;
```

---

# Step 4: Changing the Component

Let's say you change the component's `name` property:

```vue
<!-- HelloWorld.vue -->
<template>
  <div>
    <h1>{{ greeting }} {{ name }}!</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      greeting: 'Hello',
      name: 'Vue.js',
    };
  },
};
</script>
```

Now, run the test again:

```
npm run test:unit
```

## What Happens Next?

Jest will compare the current output with the previously stored snapshot. Since the output has changed (`Vue` is now `Vue.js`), Jest will fail the test and display the difference:

```
Snapshot Summary
 › 1 snapshot test failed from 1 test suite. Inspect your code changes or run `jest -u` to update the snapshot.
```

Jest will also display the difference in the output:

```
  - <h1>Hello Vue!</h1>
  + <h1>Hello Vue.js!</h1>
```

At this point, you have two options:

1. **Update the Snapshot**: If the changes are intentional, you can update the snapshot to reflect the new output by running:

```
npm run test:unit -- -u
```

2.   
   This command updates the saved snapshot with the new output.

3. **Investigate the Change**: If the change wasn't intentional, you can leave the snapshot as-is and investigate the issue.

---

# Advanced Snapshot Testing

## Snapshot Testing with Props and Dynamic Content

You can also test components with props, which will affect the rendered output.

```vue
<template>
  <div>
    <h1>{{ greeting }} {{ name }}!</h1>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
  },
  data() {
    return {
      greeting: 'Hello',
    };
  },
};
</script>
```

## Test with Props

```javascript
import { mount } from '@vue/test-utils';
import HelloWorld from '@/components/HelloWorld.vue';

describe('HelloWorld.vue', () => {
  it('matches the snapshot with dynamic prop', () => {
    const wrapper = mount(HelloWorld, {
      props: {
        name: 'Vue.js',
      },
    });
    expect(wrapper.html()).toMatchSnapshot();
  });
});
```

In this example:

* We pass a `name` prop to the component (`Vue.js`), which affects the rendered output.

* The snapshot is now generated based on this prop.

## Snapshot Testing with Slots

You can also snapshot test components that accept slots:

```vue
<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'MySlotComponent',
};
</script>
```

## Test for Slots

```javascript
import { mount } from '@vue/test-utils';
import MySlotComponent from '@/components/MySlotComponent.vue';

describe('MySlotComponent.vue', () => {
  it('renders slot content correctly', () => {
    const wrapper = mount(MySlotComponent, {
      slots: {
        default: '<div>Slot Content</div>',
      },
    });
    expect(wrapper.html()).toMatchSnapshot();
  });
});
```

This will generate a snapshot for the component including its slot content.

---

# Best Practices for Snapshot Testing

1. **Keep Snapshots Small and Focused**: Large snapshots can become hard to maintain. Keep snapshots focused on the most important or high-level components.

2. **Review Snapshot Changes Carefully**: Snapshots can change if the component's structure changes. Always review changes to ensure they’re intentional and correct.

3. **Don’t Use Snapshots for Complex Logic**: Snapshots are good for verifying the rendered output, but don't rely on them for testing complex logic or state management.

4. **Update Snapshots Intentionally**: If the structure of a component changes intentionally, update the snapshot using `jest --updateSnapshot` (`-u` flag) after confirming that the changes are correct.

---

## Conclusion

Snapshot testing in Jest allows you to:

* Automatically verify the rendered output of your Vue components.

* Ensure that changes to your components are intentional and consistent over time.

* Easily detect changes to the structure of your UI that might otherwise be missed.

It's a simple yet effective tool for ensuring the stability of your component templates and making refactors safer.

