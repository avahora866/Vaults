---
tags:
  - FORMAT
  - NodeJs
---
Absolutely. The `EventEmitter` class in Node.js is part of the built-in `events` module. It provides a way to **handle custom events** — kind of like a pub/sub system where you can "emit" events and other parts of your code can "listen" for them.

---

# Basic Concept

* **Emit**: You trigger an event.

* **Listen**: You wait for that event and respond when it occurs.

This is useful for building asynchronous, event-driven systems — which is a core concept in Node.js.

---

# How to Use `EventEmitter`

## Import the module and create an instance

```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();
```

---

## Listen to an event with `.on()`

```javascript
emitter.on('greet', (name) => {
  console.log(`Hello, ${name}`);
});
```

---

## Emit an event with `.emit()`

```javascript
emitter.emit('greet', 'Alice'); // Output: Hello, Alice
```

---

# Example: Basic Usage

```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.on('order', (item, quantity) => {
  console.log(`Order received: ${quantity} ${item}(s)`);
});

emitter.emit('order', 'pizza', 2); // Output: Order received: 2 pizza(s)
```

---

# Useful Methods

| Method | Description |
| ----- | ----- |
| `on(event, fn)` | Register a listener |
| `emit(event, ...)` | Trigger the event |
| `once(event, fn)` | Listen only the first time it's emitted |
| `off(event, fn)` | Remove a specific listener (alias: `removeListener`) |

## `.once()` example:

```javascript
emitter.once('connect', () => {
  console.log('Connected once');
});

emitter.emit('connect'); // Connected once
emitter.emit('connect'); // No output
```

---

# Why Use `EventEmitter`?

* To create loosely coupled components

* For handling asynchronous events (like streams, sockets, or timers)

* To avoid callback hell in some custom logic

---

# Built-in Usage

Many core modules in Node.js are instances of `EventEmitter`, including:

* `fs.ReadStream`

* `http.Server`

* `process` (e.g., `process.on('exit', () => {})`)

---

Let me know if you want a practical use case like building a mini event-driven order system or connecting it with HTTP servers.

