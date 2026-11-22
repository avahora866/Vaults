---
tags:
  - FORMAT
  - NodeJs
---
Yes\! Creating and handling **custom events** in Node.js is done using the `EventEmitter` class from the built-in `events` module.

This allows your application to **emit its own events** and **react to them**, which helps you build modular, event-driven code.

---

# Step-by-Step: Creating and Handling Custom Events

## Import the `events` module and create an instance

```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();
```

---

## Set up an event listener with `.on()`

This defines what should happen when the event is triggered.

```javascript
emitter.on('userRegistered', (username) => {
  console.log(`New user registered: ${username}`);
});
```

---

## Emit the event using `.emit()`

This triggers the event, optionally passing data with it.

```javascript
emitter.emit('userRegistered', 'johndoe');
```

---

# Full Example

```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();

// Listener
emitter.on('orderPlaced', (orderId, amount) => {
 console.log(`Order #${orderId} placed. Total: $${amount}`);
});

// Emitting the custom event
emitter.emit('orderPlaced', 1234, 49.99);
```

**Output:**

```
rder #1234 placed. Total: $49.99
```

---

# Using `.once()` to Handle an Event Only Once

```javascript
emitter.once('login', (user) => {
  console.log(`${user} logged in`);
});

emitter.emit('login', 'Alice'); // Output: Alice logged in
emitter.emit('login', 'Bob');   // No output
```

---

# Removing Event Listeners

You can remove an event listener using `.off()` or `.removeListener()`:

```javascript
function notifyUser(name) {
  console.log(`${name}, you have a message`);
}

emitter.on('message', notifyUser);
emitter.emit('message', 'Tom');

emitter.off('message', notifyUser);
emitter.emit('message', 'Tom'); // No output
```

---

# When to Use Custom Events

* Breaking code into smaller, independent parts

* Responding to asynchronous operations

* Avoiding deeply nested callbacks

* Decoupling different modules (e.g., order system, notification system)

---

Let me know if you want an example involving multiple modules or a custom event system for a mini app.

