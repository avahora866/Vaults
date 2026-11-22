---
tags:
  - FORMAT
  - Theory
---
A **web server** is software (or hardware) that handles **HTTP requests** from clients (like web browsers) and **serves web pages** or other resources as responses.

A web server’s job is to:

1. **Listen for incoming HTTP requests** from clients (e.g., browsers, mobile apps).  
2. **Process the request** and determine what content to return.  
3. **Send the response** back to the client.

# General Web Server Behavior (Applies to All Web Servers)

All web servers (e.g., Apache, Nginx, Tomcat, Node.js) follow this basic process:

1. **Open a Network Socket on a Port**  
   * A web server binds to a specific port (e.g., `80` for HTTP, `443` for HTTPS, `8080` for Tomcat by default).  
   * It listens for incoming TCP connections.  
2. **Accept Incoming HTTP Requests**  
   * When a client (browser, mobile app) sends a request, the web server reads the request.  
   * The request contains:  
     * URL path (e.g., `/home`)  
     * HTTP method (GET, POST, etc.)  
     * Headers and body  
3. **Route the Request to the Right Handler**  
   * The web server checks its **configuration** (e.g., Apache `httpd.conf`, Nginx `nginx.conf`) to determine how to handle the request.  
   * If the request is for a static file (HTML, CSS, images), it serves it directly.  
   * If it's for dynamic content (like a Java Servlet or Node.js API), it forwards the request to an application handler.  
4. **Process and Send Response**  
   * The web server generates an HTTP response.  
   * The response is sent back to the client over TCP.

# How Does a Web Server Open a Socket?

When Tomcat (or any web server) starts:

1. It **creates a socket** and **binds it to a port** (e.g., `8080`).  
2. It **listens for incoming connections** on that port.  
3. When a client (browser, mobile app) sends a request, the server **accepts the connection** and processes it.

**Example (Java Code)** Tomcat internally does something like this:

```java
ServerSocket serverSocket = new ServerSocket(8080); // Open socket on port 8080
while (true) {
    Socket clientSocket = serverSocket.accept(); // Wait for incoming request
    handleRequest(clientSocket); // Process request and send response
}

```

This means:

* The server **waits for requests** on `port 8080`.  
* When a client sends a request, the server **accepts the connection** and **handles the request**.

# Types of Web Servers

There are two major types:

## A. Static Web Servers

* Serve **pre-existing files** like HTML, CSS, and images.  
* No backend logic, just file delivery.  
* Example: **Nginx, Apache HTTP Server (httpd)**

## B. Dynamic Web Servers

* Process requests using backend code before sending a response.  
* Used for dynamic websites (e.g., web apps with login systems).  
* Example: **Apache Tomcat (for Java), Node.js (for JavaScript), Django (for Python), PHP servers**

# Examples of Popular Web Servers

| Web Server | Type | Use Case |
| ----- | ----- | ----- |
| **Apache HTTP Server** | Static & Dynamic | General-purpose web hosting |
| **Nginx** | Static & Dynamic | High-performance web serving & load balancing |
| **Tomcat** | Dynamic | Java web applications (Servlets, JSP, Spring) |
| **Node.js** | Dynamic | JavaScript-based backend applications |

# Difference Between a Web Server and an Application Server

A **web server** mainly handles **HTTP requests** and serves static content, while an **application server** runs **backend logic** and interacts with databases.

| Feature | Web Server | Application Server |
| ----- | ----- | ----- |
| **Purpose** | Serves static content (HTML, CSS, JS, images) | Runs business logic & dynamic content |
| **Example** | Apache HTTP Server, Nginx | Tomcat, WildFly, Spring Boot |
| **Handles** | Requests & file serving | API calls, database queries, session management |

🚀 **Example:**

* `Nginx` serves static HTML pages.  
* `Tomcat` runs a Java backend that generates HTML dynamically.

# Web Servers in Real Life

1. **Hosting Websites:** Every website runs on a web server.  
2. **APIs:** Web servers handle API requests for mobile and web apps.  
3. **Load Balancing:** Large websites use multiple web servers to distribute traffic.  
4. **Reverse Proxy:** Nginx can act as a middleman between clients and backend services.

