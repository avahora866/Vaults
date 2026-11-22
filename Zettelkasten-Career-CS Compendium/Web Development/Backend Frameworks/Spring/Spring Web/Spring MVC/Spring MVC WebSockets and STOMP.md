---
tags:
  - FORMAT
  - Spring
---
# WebSockets and STOMP in Spring WebSocket

## Understanding WebSockets

WebSockets provide a **full-duplex**, **bidirectional** communication channel between a client and a server over a single TCP connection. Unlike traditional HTTP, which follows a request-response model, WebSockets allow real-time communication where the server can push updates to the client without waiting for requests.

### Key Features of WebSockets

* **Persistent connection**: Once established, the connection remains open, reducing overhead.  
* **Low latency**: Ideal for real-time applications like chat, stock price updates, and collaborative tools.  
* **Event-driven**: Both the client and server can send messages independently.

## Why Use STOMP over Raw WebSockets?

WebSockets only define a **communication channel** but do not specify a **message format or protocol**. This means developers must manually implement features like routing, message acknowledgment, and handling different message types.

### STOMP (Simple Text Oriented Messaging Protocol)

STOMP is a **higher-level messaging protocol** built on top of WebSockets. It provides:

* **Message Routing**: Allows subscribing to specific topics (e.g., `/topic/messages`).  
* **Message Acknowledgment**: Ensures reliability.  
* **Interoperability**: Works with traditional message brokers like ActiveMQ and RabbitMQ.

## Implementing Spring WebSocket with STOMP

We'll build a **real-time chat application** using **Spring Boot, WebSockets, and STOMP**.

### Step 1: Add Dependencies (`pom.xml`)

```
<dependencies>
    <!-- Spring Boot WebSocket -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-websocket</artifactId>
    </dependency>

    <!-- STOMP support -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-messaging</artifactId>
    </dependency>

    <!-- JSON processing -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
    </dependency>
</dependencies>
```

### Step 2: Configure WebSocket & STOMP (`WebSocketConfig.java`)

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws") // WebSocket endpoint
                .setAllowedOrigins("*") // Allow all origins
                .withSockJS(); // Enable SockJS for fallback
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic"); // Enable message broker
        registry.setApplicationDestinationPrefixes("/app"); // Prefix for sending messages
    }
}
```

### Step 3: Create a Message DTO (`ChatMessage.java`)

```java
public class ChatMessage {
    private String sender;
    private String content;
    private String type; // message or join/leave event

    // Getters and setters
}
```

### Step 4: Create WebSocket Controller (`ChatController.java`)

```java
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.handler.annotation.SendTo;
import org.springframework.stereotype.Controller;

@Controller
public class ChatController {

    @MessageMapping("/chat.sendMessage") // Endpoint for sending messages
    @SendTo("/topic/public") // Broadcast message to subscribers
    public ChatMessage sendMessage(ChatMessage message) {
        return message; // Return message to be sent to clients
    }
}
```

---

## Implementing the Client-Side (JavaScript with SockJS & Stomp.js)

### Step 5: Connect to WebSocket and Subscribe

```javascript
const socket = new SockJS("http://localhost:8080/ws");
const stompClient = Stomp.over(socket);

stompClient.connect({}, function (frame) {
    console.log("Connected: " + frame);

    // Subscribe to topic
    stompClient.subscribe("/topic/public", function (message) {
        const receivedMessage = JSON.parse(message.body);
        console.log("Received:", receivedMessage);
    });

    // Send a test message
    stompClient.send("/app/chat.sendMessage", {}, JSON.stringify({
        sender: "User1",
        content: "Hello, WebSockets!",
        type: "message"
    }));
});
```

---

## WebSocket vs. STOMP in Spring Boot

| Feature | Raw WebSockets | WebSockets with STOMP |
| ----- | ----- | ----- |
| **Message Routing** | Manual handling | Automatic with `@MessageMapping` |
| **Message Format** | Any (JSON, XML, etc.) | Uses structured STOMP frames |
| **Subscription Model** | Manual implementation | Built-in with `/topic` |
| **Broker Support** | No | Works with ActiveMQ, RabbitMQ |

---

## Security Considerations

* **Authentication**: Use JWT or OAuth to secure WebSocket connections.  
* **CORS Handling**: Configure allowed origins in `WebSocketConfig.java`.  
* **Scaling**: Use an external message broker (e.g., RabbitMQ) for distributed systems.

---

## Final Thoughts

* **WebSockets** provide a low-latency, event-driven communication channel.  
* **STOMP** simplifies message handling by adding structured routing and broker support.  
* **Spring WebSocket** makes it easy to implement real-time features with minimal configuration.