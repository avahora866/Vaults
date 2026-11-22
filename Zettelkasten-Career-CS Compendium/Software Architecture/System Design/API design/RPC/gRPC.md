---
tags:
  - APIDesign
---
gRPC (Google Remote Procedure Call) is an open-source **high-performance RPC framework** that enables efficient communication between distributed services. It was developed by Google and is widely used in **microservices architectures**, **cloud applications**, and **high-performance APIs**.

---

# Why Use gRPC?

Traditional RESTful APIs work well for many cases but have limitations:

- **REST over HTTP/1.1 is inefficient**: Each request opens a new connection, leading to latency.
- **JSON is not efficient for serialization**: JSON is human-readable but larger in size compared to binary formats.
- **No built-in streaming**: RESTful APIs require workarounds for real-time streaming.

gRPC solves these problems with:

1. **Efficient Serialization**: Uses **Protocol Buffers (protobufs)** instead of JSON/XML, reducing data size.
2. **Multiplexed Requests**: Uses **HTTP/2**, allowing multiple requests on a single connection (no need to open/close connections repeatedly).
3. **Streaming Capabilities**: Supports **server-streaming, client-streaming, and bi-directional streaming** out-of-the-box.
4. **Cross-Language Support**: gRPC auto-generates client and server code for multiple programming languages.
5. **Built-in Authentication and Load Balancing**: Supports TLS encryption and integrates with load balancers.

---

# gRPC Communication Model

In gRPC, communication is done via **Remote Procedure Calls (RPCs)**, which allow a client to invoke a function on a remote server.

---

# Types of gRPC Communication

gRPC supports four types of RPC calls:

## Unary RPC (Request-Response)

- Client sends a single request and receives a single response.
- Similar to REST but more efficient.
- **Example:** A client asks the server for user details.

```proto
service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
}
```

- **Use case:** Fetching user information from a database.

---

## Server Streaming RPC

- Client sends a single request, but the server responds with **multiple streamed responses**.
- The connection remains open until all responses are sent.
- **Example:** The server continuously sends stock price updates.

```proto
service StockService {
  rpc GetStockUpdates (StockRequest) returns (stream StockResponse);
}
```

- **Use case:** Real-time data updates (e.g., live sports scores, stock prices).

---

## Client Streaming RPC

- The client sends **multiple requests**, but the server sends a **single response** after processing all data.
- **Example:** Uploading a large file in chunks.

```proto
service FileService {
  rpc UploadFile (stream FileChunk) returns (UploadStatus);
}
```

- **Use case:** Aggregating sensor data before sending a response.

---

## Bi-Directional Streaming RPC

- Both the **client and server** send multiple messages **simultaneously**.
- Ideal for real-time **chat applications, video streaming, or multiplayer games**.
- Uses an **open connection** where both sides communicate independently.

```proto
service ChatService {
  rpc Chat (stream ChatMessage) returns (stream ChatMessage);
}
```

- **Use case:** Live chat, video conferencing.

---

# How gRPC Works Internally

1. **Define a Service**:
    - Create a `.proto` file specifying methods and messages.
2. **Generate Code**:
    - Use `protoc` to generate client/server stubs in Java, Python, Go, etc.
3. **Implement Server**:
    - Implement the generated base class and define RPC behavior.
4. **Implement Client**:
    - Use generated stubs to call remote procedures.
5. **Use HTTP/2 for Communication**:
    - Ensures multiplexing and efficient transport.

---

# When to Use gRPC?

✅ **Best suited for:**

- **Microservices Communication:** Efficient service-to-service communication.
- **High-Performance APIs:** When performance and low latency matter.
- **Streaming Applications:** Live chat, stock updates, gaming.
- **IoT & Mobile Apps:** Low-bandwidth environments.

❌ **Not Ideal for:**

- **Browser-based Clients:** Since browsers don't natively support gRPC over HTTP/2 (requires gRPC-Web).
- **Public APIs:** REST is still the preferred choice for external-facing APIs due to broader client compatibility.

---

# Conclusion

- **gRPC is a powerful alternative to REST** for high-performance communication.
- It uses **Protocol Buffers** for compact, fast serialization.
- Supports **Unary, Server Streaming, Client Streaming, and Bi-Directional Streaming** RPCs.
- Built on **HTTP/2**, reducing overhead and improving efficiency.
- Ideal for **microservices, real-time apps, and high-performance systems**.