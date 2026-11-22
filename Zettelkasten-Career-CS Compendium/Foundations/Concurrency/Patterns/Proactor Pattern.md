---
tags:
  - FLASHCARD
  - Concurrency
---
The **Proactor** pattern is an event-driven concurrency model that handles I/O operations asynchronously. Instead of an application thread blocking while waiting for I/O to complete (like in the synchronous model) or actively checking if I/O is ready (like in the Reactor pattern), the Proactor pattern initiates an asynchronous operation and relies on the operating system to notify it via a **completion handler** only *after* the operation has fully finished.

# Explanation
This pattern maximizes concurrency by allowing application threads to perform other tasks while potentially slow I/O operations (like network reads/writes or file access) are handled in the background by the OS or framework.

The core components are:

1. **Asynchronous Operation Processor:** The part of the system (usually the OS kernel) that performs the I/O operations asynchronously.
2. **Completion Dispatcher:** Waits for completion events from the Asynchronous Operation Processor and dispatches them to the appropriate handler.
3. **Proactor:** The central component that initiates asynchronous operations, registers completion handlers, and manages the Completion Dispatcher.
4. **Initiator:** The application code that starts an asynchronous operation (e.g., "start reading from this socket"). It provides a buffer, a completion handler, and possibly some context.
5. **Completion Handler:** Application-specific code that is *invoked* by the Completion Dispatcher *after* the asynchronous operation completes (successfully or with an error).

**Key Differences from Reactor:**
  * **Reactor:** Waits for readiness notification (`select()`), then the application thread performs the I/O (`read()`, `write()`). The application actively *drives* the I/O.
  * **Proactor:** Initiates the I/O operation and provides a callback. The application thread does *nothing* related to this I/O until the OS calls the callback *after* the I/O is done. The OS *drives* the I/O.

This makes the Proactor pattern generally more efficient for high-volume I/O, as application threads spend less time managing I/O readiness and execution.

# Code
In Java, this pattern is implemented by the Asynchronous I/O framework (NIO.2), primarily using `AsynchronousServerSocketChannel`, `AsynchronousSocketChannel`, and `CompletionHandler`.

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.AsynchronousServerSocketChannel;
import java.nio.channels.AsynchronousSocketChannel;
import java.nio.channels.CompletionHandler;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ProactorPatternDemo {

    public static void main(String[] args) throws IOException, InterruptedException {
        // Optional: Provide a thread pool for handlers to run on
        ExecutorService handlerThreadPool = Executors.newCachedThreadPool();

        // 1. Create the Asynchronous Server Socket Channel
        AsynchronousServerSocketChannel server = AsynchronousServerSocketChannel.open();
        server.bind(new InetSocketAddress("localhost", 9091));

        System.out.println("Proactor: Server started on port 9091...");

        // 2. Initiate the first asynchronous accept operation
        // Provide a CompletionHandler to be called when a client connects
        server.accept(null, new AcceptCompletionHandler(server, handlerThreadPool));

        // Keep the main thread alive (in a real app, this would be managed differently)
        System.out.println("Proactor: Main thread waiting indefinitely...");
        Thread.currentThread().join(); 

        handlerThreadPool.shutdown();
    }
}

/**
 * Completion Handler for accepting new connections.
 */
class AcceptCompletionHandler implements CompletionHandler<AsynchronousSocketChannel, Void> {
    private final AsynchronousServerSocketChannel server;
    private final ExecutorService handlerThreadPool;

    public AcceptCompletionHandler(AsynchronousServerSocketChannel server, ExecutorService pool) {
        this.server = server;
        this.handlerThreadPool = pool;
    }

    @Override
    public void completed(AsynchronousSocketChannel client, Void attachment) {
        // 3. This method is called by the OS/framework *after* a connection is accepted.
        
        System.out.println("Proactor: Accepted connection from " + getRemoteAddress(client));

        // Immediately initiate the *next* accept operation to handle more clients
        server.accept(null, this);

        // 4. Initiate an asynchronous read operation on the new client socket
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        client.read(buffer, buffer, new ReadWriteCompletionHandler(client));
    }

    @Override
    public void failed(Throwable exc, Void attachment) {
        System.err.println("Proactor: Failed to accept connection: " + exc.getMessage());
        // Handle accept failure (e.g., log, maybe retry)
    }
    
    private String getRemoteAddress(AsynchronousSocketChannel channel) {
        try { return channel.getRemoteAddress().toString(); } 
        catch (IOException e) { return "[unknown]"; }
    }
}

/**
 * Completion Handler for reading data from and writing data to a client.
 */
class ReadWriteCompletionHandler implements CompletionHandler<Integer, ByteBuffer> {
    private final AsynchronousSocketChannel client;

    public ReadWriteCompletionHandler(AsynchronousSocketChannel client) {
        this.client = client;
    }

    @Override
    public void completed(Integer bytesRead, ByteBuffer buffer) {
        // 5. This method is called *after* a read operation completes.
        
        if (bytesRead == -1) {
            // Client disconnected
            System.out.println("Proactor: Client disconnected.");
            try { client.close(); } catch (IOException e) { /* ignore */ }
            return;
        }

        buffer.flip();
        String received = new String(buffer.array(), 0, bytesRead).trim();
        System.out.println("Proactor: Received from client: " + received);

        // Prepare response (echo)
        String response = "ECHO: " + received + "\n";
        ByteBuffer writeBuffer = ByteBuffer.wrap(response.getBytes());

        // 6. Initiate an asynchronous write operation.
        // Provide a nested handler for when the write completes.
        client.write(writeBuffer, writeBuffer, new CompletionHandler<Integer, ByteBuffer>() {
            @Override
            public void completed(Integer bytesWritten, ByteBuffer writeAttachment) {
                // 7. This is called *after* the write finishes.
                if (writeAttachment.hasRemaining()) {
                    // If not all data was written, initiate another write
                    client.write(writeAttachment, writeAttachment, this);
                } else {
                    // Write complete, initiate the *next* read operation
                    buffer.clear();
                    client.read(buffer, buffer, ReadWriteCompletionHandler.this);
                }
            }

            @Override
            public void failed(Throwable exc, ByteBuffer writeAttachment) {
                System.err.println("Proactor: Failed to write to client: " + exc.getMessage());
                try { client.close(); } catch (IOException e) { /* ignore */ }
            }
        });
    }

    @Override
    public void failed(Throwable exc, ByteBuffer attachment) {
        System.err.println("Proactor: Failed to read from client: " + exc.getMessage());
        try { client.close(); } catch (IOException e) { /* ignore */ }
    }
}
```

## Code Explanation
1. **`AsynchronousServerSocketChannel.open()`**: Creates the server channel capable of asynchronous operations.
2. **`server.accept(null, new AcceptCompletionHandler(...))`**: This is the key Proactor call. It tells the OS: "Start listening for connections. Don't block my thread. When a connection *is fully accepted*, invoke the `completed` method of this `AcceptCompletionHandler`." The calling thread continues immediately.
3. **`AcceptCompletionHandler.completed(...)`**: This method is executed by a background framework/OS thread *only after* a client has successfully connected. The application thread didn't wait or poll. Inside this handler, we immediately call `server.accept()` again to listen for the *next* client.
4. **`client.read(buffer, buffer, new ReadWriteCompletionHandler(...))`**: Similar to `accept`, this initiates an asynchronous read. It tells the OS: "Start reading data into this `buffer` from this `client`. Don't block. When data *has been read*, call the `completed` method of this `ReadWriteCompletionHandler`."
5. **`ReadWriteCompletionHandler.completed(...)`**: This is called by the framework *after* data has been successfully read into the buffer.
6. **`client.write(writeBuffer, writeBuffer, new CompletionHandler<...>())`**: We initiate an asynchronous write to echo the data back. A *nested* completion handler is provided to handle the result of the write.
7. **Write Handler's `completed(...)`**: This is called *after* the write operation finishes. Crucially, *after* the write is done, we initiate the *next* read (`client.read(...)`) using the same handler instance, creating a loop of asynchronous read-then-write operations for that client.

Notice how the application threads primarily just *initiate* operations and define *handlers*. They don't block waiting for I/O, allowing them to handle other tasks or initiate operations for many other clients.
