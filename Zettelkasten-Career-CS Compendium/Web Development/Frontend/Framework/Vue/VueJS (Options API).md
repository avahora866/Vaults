
# State
We use the `data` option to declare a reactive state. Vue will call the `data` option (basically a function) when instantiating the component, and wrap the returned object in its reactivity system.
## Reactive Proxy vs Original
In Vue 3 data is made reactive through JavaScript Proxies, which leads to the following edge case:
```js
export default {
  data() {
    return {
      someObject: {}
    }
  },
  mounted() {
    const newObject = {}
    this.someObject = newObject

    console.log(newObject === this.someObject) // false
  }
}
```
When you access `this.someObject` after assigning it, the value is a reactive proxy of the original `newObject`. **Unlike in Vue 2, the original `newObject` is left intact and will not be made reactive: make sure to always access reactive state as a property of `this`.**

## Deep Reactivity
In Vue state is deeply reactive by default meaning you can expect changes to occur if you mutate nested objects or arrays
## DOM Update timings
When you mutate reactive state, the DOM is updated automatically, but the updates are not applied synchronously. Instead Vue buffers them until the "next tick" in the update cycle to make sure that each component only updates once no matter how many state changes you have made.

If you would like to wait for the "next tick" to occur you can use the `nextTick()` method

## Stateful methods

// Something about debouncing methods, which I don't understand
https://vuejs.org/guide/essentials/reactivity-fundamentals.html

# Methods
We use the methods option to add methods. These methods are different from normal js methods in that Vue automatically binds the `this` value for methods so that it always refers to the component instance
```js
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  },
  mounted() {
    // methods can be called in lifecycle hooks, or other methods!
    this.increment()
  }
}
```
# Bindings
## v-bind

## v-model

# Props

# Event Handling

# Computed

# Watchers

# Lifecycle Hooks

# State Management
# Routing

# Components (advanced)
- v-model on compoenent
- slots
- provide / inject
- async components
# Composables

