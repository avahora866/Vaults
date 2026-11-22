---
tags:
  - FLASHCARD
  - Concurrency
---

The **Reactor** pattern is an event-driven concurrency pattern that handles service requests delivered concurrently to an application. A single "Reactor" object runs in an event loop, waiting for events (like I/O) on multiple resources. When an event occurs, the Reactor "dispatches" it to a pre-registered event handler to be processed, allowing a single thread to manage many concurrent connections.

# Explanation
This pattern is the foundation of modern high-performance I/O, such as non-blocking web servers. It avoids the "thread-per-connection" model, which is inefficient and scales poorly.

The core components are:
- **Resources (Handles):** These are the resources that can generate events, such as network sockets (`SocketChannel`) or server sockets (`ServerSocketChannel`).
- **Demultiplexer (Selector):** This is an OS-level mechanism (in Java, `java.nio.channels.Selector`) that can monitor multiple resources at once. This is the key to the pattern. The application asks the demultiplexer, "Please block and wake me up only when one of these specific events (like 'new connection' or 'data available to read') happens on any of these sockets."
- **Reactor:** A single object that runs the main event loop. It takes the ready events from the Demultiplexer and dispatches them to the correct handler.
- **Event Handlers:** Application-specific code that knows what to do with an event (e.g., an "Accept Handler" accepts a new connection, a "Read Handler" reads data from a socket).

The flow is:
1. Handlers register their interest in specific events on specific resources with the Reactor (e.g., "Tell me when this socket is readable").
2. The Reactor's event loop makes a single blocking call to the Demultiplexer (`selector.select()`).
3. The Demultiplexer waits for any registered event to occur.
4. When data arrives on a socket, the Demultiplexer unblocks and returns the set of resources that are "ready."
5. The Reactor iterates over the ready set and calls the appropriate handler for each event.
6. The handler processes the event (e.g., reads the data) *without blocking* and returns control to the Reactor.

This allows a single thread to efficiently manage thousands of network connections.

# Code
In Java, the Reactor pattern is implemented directly by the `java.nio` (New I/O) package. The `Selector` is the Demultiplexer, and the `SelectionKey` represents the registration.

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class ReactorPatternDemo {

    public static void main(String[] args) {
        try {
            // 1. Create the Demultiplexer (Selector)
            Selector selector = Selector.open();

            // 2. Create the Server Socket resource
            ServerSocketChannel serverSocket = ServerSocketChannel.open();
            serverSocket.bind(new InetSocketAddress("localhost", 9090));
            serverSocket.configureBlocking(false); // Must be non-blocking

            // 3. Register the server socket with the Selector
            // We are interested in "accept" events.
            serverSocket.register(selector, SelectionKey.OP_ACCEPT);

            ByteBuffer buffer = ByteBuffer.allocate(256);
            System.out.println("Reactor: Server started on port 9090...");

            // 4. This is the Reactor's event loop
            while (true) {
                // Wait (block) until an event occurs
                selector.select();

                // 5. Get the set of ready events
                Set<SelectionKey> selectedKeys = selector.selectedKeys();
                Iterator<SelectionKey> iter = selectedKeys.iterator();

                while (iter.hasNext()) {
                    SelectionKey key = iter.next();

                    // 6. Dispatch: Check which event happened
                    
                    // Handle a new connection event
                    if (key.isAcceptable()) {
                        handleAccept(key, selector);
                    }

                    // Handle a read data event
                    if (key.isReadable()) {
                        handleRead(key, buffer);
                    }

                    // 7. Remove the event from the set
                    iter.remove();
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * Event Handler for new connections.
     */
    private static void handleAccept(SelectionKey key, Selector selector) throws IOException {
        ServerSocketChannel serverSocket = (ServerSocketChannel) key.channel();
        
        SocketChannel clientChannel = serverSocket.accept(); // Will not block
        clientChannel.configureBlocking(false);
        
        // Register the new client socket with the selector
        // We are now interested in "read" events from this client
        clientChannel.register(selector, SelectionKey.OP_READ);
        System.out.println("Reactor: Accepted new connection from " 
            + clientChannel.getRemoteAddress());
    }

    /**
     * Event Handler for readable data.
     */
    private static void handleRead(SelectionKey key, ByteBuffer buffer) throws IOException {
        SocketChannel clientChannel = (SocketChannel) key.channel();
        buffer.clear();
        
        int bytesRead = clientChannel.read(buffer);

        if (bytesRead == -1) {
            // Connection closed by client
            key.cancel();
            clientChannel.close();
            System.out.println("Reactor: Client disconnected.");
            return;
        }

        // Process the data
        buffer.flip();
        String received = new String(buffer.array(), 0, bytesRead).trim();
        System.out.println("Reactor: Received message: " + received);

        // Echo back the data (this is also a non-blocking write)
        clientChannel.write(ByteBuffer.wrap(("ECHO: " + received + "\n").getBytes()));
    }
}
```

-----

## Code Explanation
1. **`Selector.open()`**: Creates the demultiplexer.
2. **`ServerSocketChannel.open()`**: Creates the main resource for listening for connections.
3. **`configureBlocking(false)`**: This is **critical**. It puts the channel in non-blocking mode, so calls like `accept()` return immediately if no connection is present.
4. **`serverSocket.register(...)`**: This registers the `ServerSocketChannel` with the `Selector`, telling it we are interested in `OP_ACCEPT` events.
5. **`selector.select()`**: This is the main blocking call of the event loop. This single thread will sleep here until one or more registered events (e.g., `OP_ACCEPT`) occur.
6. **`selectedKeys()`**: When `select()` returns, this set contains the `SelectionKey` objects for all events that are ready.
7. **`key.isAcceptable()`**: This is the dispatching logic. The Reactor checks the event type. If it's an "accept" event, it calls the `handleAccept` method.
8. **`handleAccept`**: This is the "Accept Handler." It calls `serverSocket.accept()` to get the new client, configures that new client as non-blocking, and then registers *it* with the *same selector*, but this time listening for `OP_READ` events.
9. **`key.isReadable()`**: Later, when the client sends data, `select()` will unblock and this key will be in the set. The Reactor dispatches to the `handleRead` method.
10.**`iter.remove()`**: This is vital. We must manually remove the key from the `selectedKeys` set after processing it, or the selector will report it as "ready" again in the next loop.