---
tags:
  - Java
---
It provides APIs to work with various network protocols, such as **TCP/IP**, **UDP**, and **HTTP**.

# Basics of Networking in Java

Java networking is built on the **java.net** package, which provides classes and interfaces for handling networking operations. Some key concepts include:

* **IP Address**: A unique identifier for devices on a network.  
* **Port Number**: A number assigned to a specific process or service on a device.  
* **Sockets**: Endpoints for communication between two devices.  
* **Protocols**: Rules governing data transmission (e.g., TCP, UDP, HTTP).

---

# Important Java Networking Classes

Java provides several key classes for network communication:

## (A) URL & HttpURLConnection (For HTTP Communication)

Used for making HTTP requests, like fetching web pages.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://example.com");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");

        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String inputLine;
        while ((inputLine = in.readLine()) != null) {
            System.out.println(inputLine);
        }
        in.close();
    }
}
```

🔹 **Use case**: Web scraping, API calls.

## (B) Socket & ServerSocket (For TCP Communication)

TCP (Transmission Control Protocol) ensures reliable communication.

### Client Example:

```java
import java.io.*;
import java.net.*;

public class SimpleClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 5000);
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

        out.println("Hello, Server!");
        System.out.println("Server says: " + in.readLine());

        socket.close();
    }
}
```

### Server Example:

```java
import java.io.*;
import java.net.*;

public class SimpleServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(5000);
        System.out.println("Server is waiting for client...");

        Socket socket = serverSocket.accept();
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

        String clientMessage = in.readLine();
        System.out.println("Client says: " + clientMessage);
        out.println("Message received!");

        socket.close();
        serverSocket.close();
    }
}
```

🔹 **Use case**: Chat applications, multiplayer games, client-server applications.

## (C) DatagramSocket (For UDP Communication)

UDP (User Datagram Protocol) is faster but unreliable.

### UDP Server Example:

```java
import java.net.*;

public class UDPServer {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(9876);
        byte[] receiveData = new byte[1024];

        DatagramPacket packet = new DatagramPacket(receiveData, receiveData.length);
        socket.receive(packet);
        String message = new String(packet.getData(), 0, packet.getLength());
        System.out.println("Received: " + message);

        socket.close();
    }
}
```

### UDP Client Example:

```java
import java.net.*;

public class UDPClient {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket();
        InetAddress address = InetAddress.getByName("localhost");
        byte[] sendData = "Hello, UDP Server!".getBytes();

        DatagramPacket packet = new DatagramPacket(sendData, sendData.length, address, 9876);
        socket.send(packet);

        socket.close();
    }
}
```

🔹 **Use case**: Online gaming, video streaming, IoT.

---

# Networking Protocols

* **TCP (Transmission Control Protocol)**: Reliable, connection-oriented.  
* **UDP (User Datagram Protocol)**: Fast, but connectionless.  
* **HTTP/HTTPS (HyperText Transfer Protocol/Secure)**: Used for web communication.  
* **FTP (File Transfer Protocol)**: Used for transferring files.

