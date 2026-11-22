---
tags:
  - APIDesign
---
**Remote Procedure Call (RPC)** is a communication protocol that allows a program to execute a function or procedure on a remote server as if it were a local function. It enables distributed computing by allowing applications to interact over a network.

---

# How RPC Works

1. **Client Sends a Request**

   * The client program calls a remote function, passing input parameters.

   * The RPC framework serializes (marshals) the request into a network-friendly format.

2. **Server Processes the Request**

   * The remote server receives the request, deserializes it, and executes the function.

3. **Server Sends a Response**

   * The function result is serialized and sent back to the client.

   * The client receives the response and continues execution as if it were a local call.

---

# Key Features of RPC

✔ **Abstracts Networking:** Developers don’t need to handle low-level network communication.  
 ✔ **Looks Like a Local Function Call:** The client interacts with the remote function the same way it would with a local function.  
 ✔ **Supports Multiple Protocols:** RPC can be built over HTTP, TCP, [[gRPC|gRPC]], etc.  
 ✔ **Can Be Synchronous or Asynchronous:** Clients can wait for a response or execute calls in the background.

---

# Types of RPC

## Synchronous RPC

* The client waits for the server to respond before continuing.

* Example: Calling a function via [[gRPC|gRPC]] and waiting for the result.

## Asynchronous RPC

* The client sends a request and continues execution without waiting for a response.

* Example: Sending an event to AWS Lambda asynchronously.

---

# Examples of RPC Implementations

1. **[[gRPC|gRPC]] (Google RPC)** – High-performance RPC framework based on Protocol Buffers (protobuf).

2. **XML-RPC** – Uses XML over HTTP for communication.

3. **JSON-RPC** – Uses JSON format over HTTP or WebSockets.

4. **Thrift (Apache Thrift)** – Cross-language RPC framework.

---

# RPC vs REST vs GraphQL

| Feature | RPC | REST | GraphQL |
| ----- | ----- | ----- | ----- |
| **Data Format** | Binary ([[gRPC|gRPC]]), JSON, XML | JSON, XML | JSON |
| **Protocol** | TCP, HTTP, WebSockets | HTTP | HTTP |
| **Speed** | Fast (binary format) | Slower | Flexible but can be complex |
| **Flexibility** | Defined contracts (IDLs) | URL-based endpoints | Clients define responses |

---

# When to Use RPC?

✔ **Microservices Communication:** Fast and efficient interaction between distributed services.  
 ✔ **Low-Latency Applications:** [[gRPC|gRPC]] is optimized for high-speed communication.  
 ✔ **Cross-Language Interoperability:** RPC frameworks like [[gRPC|gRPC]] and Thrift work across different programming languages.  
 ✔ **Cloud & Serverless Systems:** AWS Lambda, Google Cloud Functions, and other services use RPC mechanisms for execution.

