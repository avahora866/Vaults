---
tags:
  - TAG
  - COMPREHEND
  - Vue
---
Unit testing is an essential part of maintaining and improving the quality of a Vue.js application. In Vue, the two most commonly used tools for testing are **Vue Test Utils** and **Jest**.

* **Vue Test Utils** is the official unit testing library for Vue.js, providing helpers to interact with Vue components in a test environment.

* **Jest** is a JavaScript testing framework that works seamlessly with Vue Test Utils for running tests, making assertions, and mocking dependencies.

Let's break down the basics of unit testing in Vue with these tools.

---

# Setting Up Your Project for Unit Testing

Before starting with tests, you need to install **Vue Test Utils** and **Jest**.

## Install Vue Test Utils and Jest

You can install these dependencies via npm:

```
npm install --save-dev @vue/test-utils jest
```

If you’re using Vue 3, you may need to install specific versions of the tools:

```
npm install --save-dev @vue/test-utils@next jest
```

Additionally, you will want to set up Jest in your project by adding a `jest.config.js` file (if it isn’t automatically configured).

## Example of Jest Configuration (jest.config.js)

```javascript
module.exports = {
  preset: '@vue/cli-plugin-unit-jest/presets/no-babel',
  testEnvironment: 'jsdom',
};
```

This configures Jest to work with Vue components and provides a testing environment for simulating a DOM.

---

# Writing Your First Unit Test

Now let’s take a simple Vue component and write a test for it.

## Sample Vue Component (`HelloWorld.vue`)

```vue
<template>
  <div>
    <h1>{{ greeting }} {{ name }}!</h1>
    <button @click="changeName">Change Name</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: 'Vue',
      greeting: 'Hello',
    };
  },
  methods: {
    changeName() {
      this.name = 'World';
    },
  },
};
</script>
```

This component displays a greeting and a button that, when clicked, changes the name.

## Unit Test for `HelloWorld.vue`

Next, we will write a test for this component to verify its behavior using **Vue Test Utils** and **Jest**.

```javascript
// HelloWorld.spec.js
import { mount } from '@vue/test-utils';
import HelloWorld from '@/components/HelloWorld.vue';

describe('HelloWorld.vue', () => {
  it('renders the greeting and name', () => {
    const wrapper = mount(HelloWorld);
    expect(wrapper.text()).toContain('Hello Vue!');
  });

  it('changes the name when the button is clicked', async () => {
    const wrapper = mount(HelloWorld);
    await wrapper.find('button').trigger('click'); // Simulate button click
    expect(wrapper.text()).toContain('Hello World!');
  });
});
```

## Explanation of the Test Code:

* **`mount`**: This function from Vue Test Utils mounts the `HelloWorld` component in a simulated DOM. It allows us to interact with the component’s template and data.

* **`describe`**: This is a Jest function that groups together similar tests. Here we’re grouping tests related to the `HelloWorld.vue` component.

* **`it`**: This defines individual test cases. Each `it` block should contain a specific test of the component.

* **`expect`**: This is used to make assertions in Jest. We are asserting that the rendered output contains specific text.

* **`trigger('click')`**: This simulates a user interaction (clicking the button) within the test. Since `changeName` is a method triggered by the click event, we test that the button click properly updates the component's output.

## Running the Test

You can run the test using Jest with the following command:

```
npm run test:unit
```

This command will trigger Jest to run all tests in your project and provide feedback on whether they pass or fail.

---

# Testing Component Methods

In Vue, you can test the methods of your components directly by accessing them through the wrapper.

## Example: Testing a Method Directly

Let’s say you want to test a method that increments a counter:

```vue
<template>
  <div>
    <p>{{ counter }}</p>
    <button @click="incrementCounter">Increment</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    incrementCounter() {
      this.counter += 1;
    },
  },
};
</script>
```

## Test for the Increment Method

```javascript
// IncrementCounter.spec.js
import { mount } from '@vue/test-utils';
import IncrementCounter from '@/components/IncrementCounter.vue';

describe('IncrementCounter.vue', () => {
  it('increments the counter on button click', async () => {
    const wrapper = mount(IncrementCounter);
    expect(wrapper.text()).toContain('0');

    await wrapper.find('button').trigger('click'); // Simulate button click
    expect(wrapper.text()).toContain('1');
  });
});
```

## Key Points:

* The **`trigger`** method is useful for simulating events such as `click`, `input`, `submit`, etc.

* **`await`** is used because Vue updates the DOM asynchronously after the button click, and we need to wait for the DOM to update before making assertions.

---

# Testing Vuex Store with Vue Test Utils

If you use Vuex in your application, you can mock the store in your unit tests.

## Example of a Component Using Vuex

```vue
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script>
export default {
  computed: {
    count() {
      return this.$store.state.count;
    },
  },
  methods: {
    increment() {
      this.$store.commit('increment');
    },
  },
};
</script>
```

## Test for Vuex Store Integration

```javascript
// StoreIntegration.spec.js
import { mount } from '@vue/test-utils';
import ComponentWithVuex from '@/components/ComponentWithVuex.vue';
import { createStore } from 'vuex';

describe('ComponentWithVuex.vue', () => {
  let store;

  beforeEach(() => {
    store = createStore({
      state: {
        count: 0,
      },
      mutations: {
        increment(state) {
          state.count += 1;
        },
      },
    });
  });

  it('increments count when button is clicked', async () => {
    const wrapper = mount(ComponentWithVuex, {
      global: {
        plugins: [store],
      },
    });

    expect(wrapper.text()).toContain('0');

    await wrapper.find('button').trigger('click');
    expect(wrapper.text()).toContain('1');
  });
});
```

## Explanation:

* **Vuex Store Setup**: In the `beforeEach` hook, we create a Vuex store instance with a `count` state and an `increment` mutation.

* **`global.plugins`**: This option allows us to inject the Vuex store into the component during the test.

* **Testing Vuex Interaction**: The test checks if the component interacts correctly with Vuex by triggering a mutation and verifying the updated state.

---

# Mocking API Calls in Tests

When testing components that make API calls, you often want to **mock** the API calls so you don't make real network requests.

## Mocking with Jest

```javascript
import { mount } from '@vue/test-utils';
import axios from 'axios';
import MyComponent from '@/components/MyComponent.vue';

// Mock the axios request
jest.mock('axios');

describe('MyComponent.vue', () => {
  it('fetches data on mount', async () => {
    axios.get.mockResolvedValue({ data: { message: 'Hello' } }); // Mocked response
    const wrapper = mount(MyComponent);
    
    await wrapper.vm.$nextTick(); // Wait for async data

    expect(wrapper.text()).toContain('Hello');
  });
});
```

## Explanation:

* **`jest.mock('axios')`**: This mocks the axios library to avoid actual API calls.

* **`mockResolvedValue`**: This mock function provides a fake response when the `axios.get` method is called.

* **`$nextTick()`**: Vue updates the DOM asynchronously, so we wait for the component to re-render with the mocked data.

---

# Conclusion

* **Vue Test Utils** provides methods like `mount`, `shallowMount`, and `find` to interact with components in a test environment.

* **Jest** is used for running tests and making assertions. It works well with Vue Test Utils for unit testing Vue components.

* Unit tests ensure that your components behave as expected in isolation, which helps you detect bugs early and maintain quality in your app.

* You can test component methods, computed properties, Vuex store integration, and mock external API calls easily using Jest and Vue Test Utils.

