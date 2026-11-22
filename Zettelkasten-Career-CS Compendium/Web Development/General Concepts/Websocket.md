---
tags:
  - FORMAT
  - Comprehend
---
WebSockets are a communication protocol that provides **full-duplex** (two-way) communication between a client (like a web browser) and a server over a single, long-lived connection. Unlike traditional HTTP, which follows a **request-response model**, WebSockets allow **continuous data exchange** without repeatedly opening new connections.

# How WebSockets Work

1. **Handshake**:
	- The client initiates a WebSocket connection by sending an HTTP request with the `Upgrade` header set to `websocket`.  
	- If the server supports WebSockets, it responds with a **101 Switching Protocols** status, and the connection is upgraded to WebSocket.  
2. **Persistent Connection**:
	- After the upgrade, both client and server keep the TCP connection open.
    - Messages can flow in both directions until one side closes it.
    - This reduces the overhead of repeated HTTP requests and enables real-time data transfer.
3. **Communication**:
	- Messages are exchanged in **frames**, which are lightweight compared to full HTTP requests. 
	- The connection remains open until either the client or server decides to close it.

# Why Use WebSockets?

- **Low latency**: No need to repeatedly request updates.
- **Real-time communication**: Ideal for apps that need instant updates. Chat apps, online gaming, stock market updates, and live notifications.  
* **Bidirectional**: The server can push updates to the client instantly instead of waiting for a request.
# WebSockets vs. HTTP Polling

* **HTTP Polling**: The client repeatedly sends requests to check for updates (inefficient).  
* **WebSockets**: A single connection stays open, allowing immediate updates (efficient).

# Basic WebSocket Example

## Client (JavaScript)

```javascript
const socket = new WebSocket("ws://example.com/socket");

socket.onopen = () => {
  console.log("Connected to the WebSocket server");
  socket.send("Hello, Server!");
};

socket.onmessage = (event) => {
  console.log("Received from server:", event.data);
};

socket.onclose = () => {
  console.log("WebSocket connection closed");
};
```

## Server (Node.js using `ws` library)

```javascript
const WebSocket = require("ws");
const server = new WebSocket.Server({ port: 8080 });

server.on("connection", (socket) => {
  console.log("Client connected");

  socket.on("message", (message) => {
    console.log("Received:", message);
    socket.send("Hello, Client!");
  });

  socket.on("close", () => {
    console.log("Client disconnected");
  });
});
```

# Stateful
A WebSocket is stateful which means once a WebSocket connection is established between a client and server, both sides keep track of the connection and its context until it’s explicitly closed.

## Why WebSocket's are stateful

- **Persistent connection**: The TCP connection stays open instead of being re-established for every request like HTTP.
- **Session context**: The server knows which client it’s talking to because the connection is unique and continuous.
- **Bidirectional communication**: Since both sides can send messages at any time, they must maintain state about the connection (open, closed, what’s been sent, what’s acknowledged).

## Contrast with HTTP
- **HTTP (stateless)**: Each request is independent. The server doesn’t remember anything about previous requests unless you add external mechanisms (cookies, tokens, sessions).
- **WebSocket's (stateful)**: Once connected, the server and client maintain the same session for the lifetime of the connection.

## Implications
- **Resource use**: Each WebSocket connection consumes memory/CPU on the server, since it must keep state per client.
- **Scalability**: Large-scale WebSocket apps often need load balancing and connection-aware infrastructure (sticky sessions, or shared state via Redis, etc.).
- **Reliability**: If the connection drops, state is lost unless you design reconnection logic.
