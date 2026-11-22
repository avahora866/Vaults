---
tags:
  - FORMAT
  - NodeJs
---
# WebSockets & Real-Time Communication in Node.js

**WebSockets** is a protocol that allows for **bi-directional communication** between a client and a server over a single, persistent connection. Unlike traditional HTTP, which is request-response based, WebSockets enable **real-time** communication, where both the server and the client can send messages at any time without needing to re-establish a connection.

This makes WebSockets ideal for applications that require real-time updates, such as **chat apps**, **live notifications**, **online games**, and **live data feeds**.

There are libraries in Node.js, such as `ws` and `socket.io`, that simplify the process of implementing WebSockets.

---

# Using `ws` or `socket.io` for WebSockets

## `ws` Library

[`ws`](https://www.npmjs.com/package/ws) is a simple WebSocket library for Node.js. It provides an easy-to-use interface for both the client and server to communicate over WebSockets.

### Setting up a WebSocket server with `ws`:

1. Install `ws` via npm:

```
npm install ws
```

2.   
   Create a basic WebSocket server:

```javascript
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  console.log('A new client connected');
  ws.send('Hello from the server');

  ws.on('message', (message) => {
    console.log(`Received: ${message}`);
  });
});

console.log('WebSocket server is running on ws://localhost:8080');
```

In this example:

* The server listens on port `8080` and waits for client connections.

* When a client connects, the server sends a greeting message to the client.

* The server also listens for messages sent by the client.

## `socket.io` Library

[`socket.io`](https://socket.io/) is a higher-level library that builds on WebSockets and provides additional features such as **automatic reconnection**, **broadcasting**, and **rooms**. It’s ideal for real-time applications that require more functionality.

### Setting up a WebSocket server with `socket.io`:

1. Install `socket.io` via npm:

```
npm install socket.io
```

2.   
   Create a basic WebSocket server:

```javascript
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

io.on('connection', (socket) => {
  console.log('A new client connected');
  socket.emit('message', 'Hello from server');

  socket.on('chat message', (msg) => {
    console.log(`Message from client: ${msg}`);
    io.emit('chat message', msg);  // Broadcasting to all clients
  });

  socket.on('disconnect', () => {
    console.log('A client disconnected');
  });
});

server.listen(3000, () => {
  console.log('Socket.io server is running on http://localhost:3000');
});
```

In this example:

* The server is set up using **Express** and **Socket.IO**.

* The server listens for a connection from clients, emits a greeting message, and handles incoming **chat messages**.

* When a client sends a chat message, the server broadcasts it to **all connected clients**.

---

# Broadcasting Messages

**Broadcasting** refers to sending a message to all connected clients (or a specific subset of clients) from the server. Both `ws` and `socket.io` support broadcasting messages to multiple clients.

## Broadcasting with `ws`:

To broadcast a message to all connected clients, you can iterate over all WebSocket connections:

```javascript
wss.on('connection', (ws) => {
  console.log('A new client connected');
  
  // Broadcasting a message to all clients
  wss.clients.forEach((client) => {
    if (client !== ws && client.readyState === WebSocket.OPEN) {
      client.send('A new user has joined');
    }
  });
  
  // Handle incoming message from a client
  ws.on('message', (message) => {
    console.log(`Received: ${message}`);
    // Broadcast received message to all other clients
    wss.clients.forEach((client) => {
      if (client !== ws && client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });
});
```

In this example:

* The message `"A new user has joined"` is broadcasted to all other clients when a new user connects.

* When a client sends a message, it’s broadcasted to all other clients connected to the WebSocket server.

## Broadcasting with `socket.io`:

With `socket.io`, broadcasting is even easier with the built-in `.broadcast` method, or `.emit` can be used to broadcast messages to all clients.

```javascript
// Broadcasting to all clients
socket.emit('chat message', msg);

// Broadcasting to all clients except the sender
socket.broadcast.emit('chat message', msg);

// Broadcasting to a specific room
io.to('room1').emit('chat message', msg);

// Broadcasting to everyone except the sender
socket.broadcast.emit('message', 'User disconnected');
```

In this example:

* `socket.emit` sends a message to all connected clients.

* `socket.broadcast.emit` sends a message to all clients **except** the sender.

* You can also use **rooms** to send messages to a subset of clients.

---

# Real-Time Chat Applications

**Real-time chat applications** are one of the most common use cases for WebSockets. Here's how WebSockets can be used to create a simple chat application:

## Steps for Creating a Basic Real-Time Chat Application:

1. **Backend (Server-side) with `socket.io`**:

   * Set up an Express server with `socket.io` to handle real-time connections.

   * Use broadcasting to send messages to all connected users.

```javascript
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

app.use(express.static('public'));  // Serve the frontend files

io.on('connection', (socket) => {
  console.log('A user connected');
  
  // Broadcast the message to all clients
  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });

  socket.on('disconnect', () => {
    console.log('A user disconnected');
  });
});

server.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

2.   
   **Frontend (Client-side) with HTML and JavaScript**:

   * Use `socket.io-client` on the client to connect to the WebSocket server.

   * Set up an HTML page for displaying and sending messages.

```
<!DOCTYPE html>
<html>
<head>
  <title>Real-Time Chat</title>
  <script src="/socket.io/socket.io.js"></script>
  <script>
    const socket = io();

    // Send message
    function sendMessage() {
      const msg = document.getElementById('message').value;
      socket.emit('chat message', msg);
      document.getElementById('message').value = '';
    }

    // Listen for new messages
    socket.on('chat message', (msg) => {
      const chat = document.getElementById('chat');
      const newMessage = document.createElement('li');
      newMessage.textContent = msg;
      chat.appendChild(newMessage);
    });
  </script>
</head>
<body>
  <ul id="chat"></ul>
  <input id="message" type="text">
  <button onclick="sendMessage()">Send</button>
</body>
</html>
```

In this example:

* The backend (server) is handling the WebSocket connection and broadcasting messages using `socket.io`.

* The frontend (client) connects to the server, listens for new messages, and sends messages using the input field.

## Features of a Real-Time Chat Application:

* **User Presence**: Display when a user joins or leaves the chat.

* **Typing Indicator**: Notify when someone is typing a message.

* **Private Messaging**: Allow users to send direct messages to each other.

* **Rooms/Channels**: Group users into rooms or channels for specific topics.

---

# Conclusion

**WebSockets** are a powerful tool for enabling real-time communication in web applications. Libraries like `ws` and `socket.io` make it easy to implement WebSockets in Node.js.

* **WebSocket server**: You can set up a simple WebSocket server using `ws` or `socket.io` to allow bi-directional communication.

* **Broadcasting messages**: Both libraries provide ways to broadcast messages to all or selected clients.

* **Real-time chat**: WebSockets are ideal for building real-time chat applications by continuously pushing updates to clients.

By leveraging these technologies, you can build interactive, real-time applications that keep users engaged with dynamic content and seamless communication.

