---
tags:
  - Java
---
Java Servlets are the Java programs that run on the Java-enabled [[Web Server|web server]] or application server. They are used to handle the request obtained from the web server, process the request, produce the response, and then send a response back to the web server.

When a server receives a request for a servlet it hands it over to the servlet container. The servlet container is responsible for instantiating the servlet or creating a new thread to handle the request. It's the job of the servlet container to get the request and response to the servlet. The container creates multiple threads to process multiple requests to a single servlet.  
***Note: A servlet does not have a main() method. The servlet container manages the life cycle of a servlet instance.***

1. User sends request for a servlet by clicking a link that has a URL to a servlet  
2. The container finds the servlet using the **deployment descriptor** and creates 2 objects:  
   1. HTTPServletRequest  
   2. HTTPServletResponse  
3. The servlet container creates or allocates a thread for that request and calls the Servlet’s service() and passess the request, response objects as arguments  
4. The service() then decides which servlet method, doGet() or doPost() to cal based on the HTTPRequestMethod (Get, Post, etc.) sent by the client, Suppose the client sent an HTTP GET request, so the service() will call Servlet’s doGet().  
5. Then the servlet uses response object to write the response back to the client  
6. After the service() method is completed the thread dies. The request and response objects then is ready for **garbage collection**

# Running a servlet application
Servlets don't work like standalone Java applications - they're designed to run within a servlet container (like Tomcat, Jetty, or a Java EE application server).

Here's how it works:

1. You build your project with Maven: `mvn clean package`
2. This creates a WAR (Web Application Archive) file in the `target` directory
3. You deploy this WAR file to a servlet container, or use an embedded container for development

For development, the easiest way is to use the Jetty Maven plugin. You simply run:

```
mvn jetty:run
```

This will:

1. Start an embedded Jetty server
2. Deploy your application to it
3. Make your servlet accessible at the URL path you specified (e.g., http://localhost:8080/simple-servlet/hello)

The servlet container handles the lifecycle of your servlets - it creates instances, calls their methods in response to HTTP requests, and manages their lifecycle. You don't call the methods directly or run a main method.


