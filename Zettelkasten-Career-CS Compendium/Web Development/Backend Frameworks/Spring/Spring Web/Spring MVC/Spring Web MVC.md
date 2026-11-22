---
tags:
  - "#TRUNK"
---
Spring Web is the umbrella term for the entire web stack provided by the Spring Framework. It is also the name of a specific JAR dependency (spring-web) that contains common web-related utilities (like RestTemplate and foundational web exceptions) used by both the Servlet and Reactive stacks.

Spring MVC (Model-View-Controller) is a specific module (spring-webmvc) within the Spring Web ecosystem. It is the implementation of the MVC architectural pattern built specifically on the Servlet API (blocking I/O).

They are not exactly the same thing. Think of "Spring Web" as the Department of Physics, and "Spring MVC" as the course on Classical Mechanics. One is the domain; the other is a specific methodology within that domain.

- `spring-web.jar` is the Foundation.
    - This library contains the concepts that are neutral to the underlying technology. It doesn't care if you are blocking (Servlet) or non-blocking (Netty).
    - The Annotations: Yes! The annotations you use every day (@RestController, @GetMapping, @PathVariable, @RequestBody) actually live here.
    - The Clients: Tools to make requests, like RestTemplate and the interface for WebClient.
    - The Exceptions: Standard errors like ResponseStatusException.
    - The Context: The basic WebApplicationContext interface.
- `spring-webmvc.jar` depends on spring-web.jar.
    - This library contains the implementation specifically for the Servlet API (Tomcat/Jetty).
    - The Engine: DispatcherServlet.
    - The Mechanics: HandlerAdapter, HandlerMapping, ViewResolver.
    - The Rendering: JSP/Thymeleaf integration classes.
- `spring-webflux.jar` (the reactive stack) also depends on spring-web.jar.

# The Servlet Container
When a user sends an HTTP request (e.g., GET /users) to your Java application, Spring is not the first entity to receive it. Spring is merely a library of code sitting on your hard drive. It cannot "listen" to a network port on its own.

That responsibility belongs to the Servlet Container (often called the Web Server). The most common examples are Apache Tomcat, Jetty, or Undertow. 

Think of the architecture like a shopping mall:
- The Servlet Container (e.g., Tomcat) is the Mall Management. It provides the building, electricity, security, and the main doors (Network Ports). It handles the flow of customers (HTTP Requests) coming in from the street.
- The Servlet is a Shop inside the mall. It doesn't worry about building maintenance; it only worries about serving the customer once they walk through the door.
- Spring MVC is a massive Department Store that occupies the entire mall space. To the Mall Management (Tomcat), Spring is just another shop (Servlet), but internally, Spring is a complex ecosystem.

## The Container's Duties
The Servlet Container performs three critical tasks that Spring relies upon:
1. Network Management: It listens on a port (e.g., 8080) and decodes the raw TCP byte stream into an HTTP Object (HttpServletRequest).
2. Lifecycle Management: It manages the birth and death of Servlets. It calls init() when the application starts and destroy() when it shuts down.
3. Thread Management (The "Thread-per-Request" Model):
    - This is crucial for performance theory.
    - By default, Tomcat allocates a Thread Pool (typically capped at 200 threads in a standard configuration).
    - When a request arrives, the Container grabs an idle thread from the pool and assigns it to that request.
    - That thread is exclusively "occupied" until the response is fully sent back to the user.

## The Interface
Technically, any Java class can handle web traffic if it implements the javax.servlet.Servlet (or jakarta.servlet.Servlet in newer versions) interface.

The core method is:
```java
public void service(ServletRequest req, ServletResponse res) {
    // Read request
    // Process business logic
    // Write response
}
```
In the context of Spring Web, Spring provides a single, master class that implements this interface. This is the DispatcherServlet. We will analyze this specifically in the next module, but for now, understand that Tomcat hands the request to this one special Spring class.

# The Front Controller Pattern & The DispatcherServlet
Now that Tomcat has assigned a thread to a request, it looks at the URL (e.g., /api/users/1) and asks: "Which Java class is responsible for this?"

In the "old days" of Java Web development (legacy Servlets), you would write a separate Servlet for every distinct function: a UserServlet, a ProductServlet, an OrderServlet, etc. You would map them individually in a massive configuration file. This resulted in duplicated code—every Servlet had to handle its own error catching, logging, and security checks.

Spring MVC solves this using the Front Controller Pattern.

## The Concept: The Front Controller
Imagine a large corporate headquarters.
- Without a Front Controller: Visitors wander into side doors, loading docks, and fire exits, trying to find the Finance department. Security is a nightmare, and no one knows who is in the building.
- With a Front Controller: There is exactly one main entrance with a Reception Desk. Everyone must go through the Receptionist. The Receptionist checks their ID (Security), logs their arrival (Logging), and then gives them a badge and directs them to the correct floor (Routing).

In Spring MVC, that Receptionist is the DispatcherServlet.

## The Mechanism: DispatcherServlet
The DispatcherServlet is a standard Servlet (it extends HttpServlet). As far as Tomcat is concerned, it is the only shop in the mall.

When the application starts:
1. Spring registers the DispatcherServlet with the container.
2. It typically maps it to the root path: / (meaning, "send me everything").
3. The DispatcherServlet spins up its own internal dependency injection container, called the WebApplicationContext.

When a request arrives at DispatcherServlet, its job is not to execute business logic. Its job is to delegate. It acts as the traffic cop, coordinating the different components we will study in later modules (Handlers, Adapters, Resolvers).

## The WebApplicationContext
This is a specialized version of the standard Spring ApplicationContext. It contains all your Controllers, Services, and Repositories.

Crucially, the DispatcherServlet links itself to this context. When a request hits the doDispatch() method inside the Servlet, it has immediate access to every bean in your application, allowing it to route the request to the correct Controller method.

# The Request Execution Chain
The DispatcherServlet has received the request. It is holding a GET /users/1 request in its hand. However, it does not inherently know that you wrote a class called UserController with a method called getUser().

It needs help to bridge the gap between the HTTP request and your Java code. This process involves two critical interfaces: the HandlerMapping and the HandlerAdapter.

## The Lookup: HandlerMapping
First, the Dispatcher needs to find who is responsible. It consults the HandlerMapping.

Think of the HandlerMapping as the Corporate Directory.
- The Dispatcher asks: "I have a request for /users/1. Who handles this?"
- The HandlerMapping scans its internal registry (built at startup by scanning your @Controller and @RequestMapping annotations).
- It returns a HandlerExecutionChain.

Crucial Detail: It does not just return the Controller. It returns a "Chain" object containing:
1. The Handler (your Controller bean).
2. A list of Interceptors (more on these in Module 5) that must run before and after the handler.

## The Execution: HandlerAdapter
This is the part that confuses most developers. Once the Dispatcher knows who the handler is (e.g., your UserController), why doesn't it just call the method?

The Problem: Java is strongly typed. The DispatcherServlet is generic framework code. Your Controller is custom code.
- One controller might use annotations (@RequestMapping).
- Another might implement the legacy Controller interface.
- Another might be a functional endpoint.

The Dispatcher cannot write code like if (handler is Annotation) do X, else if (handler is Legacy) do Y. That would be tight coupling.

The Solution: The HandlerAdapter. Think of the HandlerAdapter as a Universal Plug Adapter.
- The Dispatcher says to the Adapter: "I have this Handler object (your controller) and this Request. I don't know how the Handler works. You deal with it."
- The Adapter (specifically the RequestMappingHandlerAdapter) knows how to read your @PathVariable, @RequestParam, and @RequestBody annotations.
- It uses Reflection to invoke your specific method: getUser(Integer id).
- It returns a standard result (usually ModelAndView) back to the Dispatcher.

Summary of Flow:
1. Request arrives at DispatcherServlet.
2. Dispatcher asks HandlerMapping: "Find me the code." -> Returns HandlerExecutionChain.
3. Dispatcher asks HandlerAdapter: "Run this code." -> Adapter runs your method -> Returns ModelAndView.

### Clarification: The "ModelAndView" vs. POJO/JSON
It highlights the evolution of Spring Web from Monolithic apps to REST APIs.
1. Classic MVC (Server-Side Rendering):
    - Historically, Spring was used to serve HTML (JSP, Thymeleaf).
    - The Controller returned a ModelAndView containing data (the Model) and the name of the HTML file (the View).
    - The Dispatcher would then pass this to a ViewResolver to render the HTML.
2. Modern REST (What you use):
    - When you annotate a method with @ResponseBody (or use @RestController), you are telling the HandlerAdapter: "Do not go to the ViewResolver. Bypass the ModelAndView flow. Just write this POJO directly to the HTTP Response body."
    - So, while the interface technically supports ModelAndView, in your case, the Adapter handles the writing immediately using Message Converters (which we will cover in Module 4) and tells the Dispatcher, "My job is done."

You are effectively using a "headless" version of the MVC pattern where the "View" is simply the JSON representation.

# Data Binding & The WebDataBinder
We have discussed finding the code (Mapping) and running the code (Adapter). Now we must discuss the Inputs.

HTTP is a text-based protocol. A request comes in as loose Strings:
- Query Param: ?age=25 (String "25")
- Path Variable: /users/101 (String "101")
- Headers: X-Expires: 2025-01-01 (String)

However, your Java method looks like this:
```java
public void updateUser(@PathVariable Long id, @RequestParam int age, @RequestHeader LocalDate expires)
```
How does the String "101" safely become a Long object? How does "2025-01-01" become a LocalDate? This magic is performed by the WebDataBinder.

## The Problem: The Impedance Mismatch
There is a fundamental mismatch between the loose, string-based nature of HTTP and the strong, type-safe nature of Java. If we just cast (int) "25", Java would crash with a Runtime Exception. We need an intelligent translation layer.

## The Mechanism: WebDataBinder
For every request, the HandlerAdapter creates a temporary WebDataBinder. Its job is to bind request parameters to the method arguments. It uses two main systems to do this:

### A. PropertyEditors (Legacy but distinct)
- Originally from the java.beans package (yes, standard Java desktop UI tech!).
- They convert String <-> Object.
- Example: If you have a java.util.Date, Spring uses a CustomDateEditor.
- Limitation: They are not thread-safe. A new instance is created for every single request.

### B. Converters & Formatters (Modern Standard)
- Introduced in Spring 3 to fix the thread-safety issue.
- Converter<S, T>: Converts type S to T. (e.g., String -> User).
- Formatter<T>: String-centric, handles localization (e.g., parsing "$1,000" vs "1.000€" depending on the user's locale).
- These are thread-safe and are singletons registered in the ConversionService.

## Validation
The WebDataBinder is also responsible for Validation. Once the data is converted (e.g., "25" -> integer 25), the Binder checks the constraints (e.g., @Min(18)). If the integer is 15, the Binder creates a BindingResult object containing the error, rather than executing the method.

# Content Negotiation & Message Conversion
We are now at the end of the request lifecycle.
1. Dispatcher received the request.
2. HandlerAdapter executed your Controller logic.
3. Controller returned a User POJO (a plain Java object).

Now, the server needs to send this object back over the network. But you cannot send a "Java Object" over HTTP. HTTP understands Bytes, Strings, JSON, XML, etc. It does not understand Java Heap memory.

We need a serializer. This is the domain of Content Negotiation.

## The Negotiation: "What do you speak?"
This is a dialogue between the Client (Browser/Postman) and the Server (Spring).

- The Client sends an Accept Header.
    - Example: Accept: application/json (I prefer JSON).
    - Example: Accept: application/xml (I prefer XML).
    - Example: Accept: */* (I don't care, send me anything).
- The Server checks the Produces condition on the Controller.
    - @GetMapping(value = "/user", produces = "application/json")

Spring calculates the intersection of "What the Client Wants" and "What the Server Can Do."

## The Mechanism: HttpMessageConverter
Once the format (MIME type) is decided (e.g., "application/json"), Spring searches for a compatible HttpMessageConverter.

Spring Web comes pre-configured with a list of converters. The most famous one is MappingJackson2HttpMessageConverter.
- It sees the MIME type is application/json.
- It sees the return object is class User.
- It checks: "Can I write class User as JSON?" (Yes).
- It calls the underlying Jackson library (ObjectMapper) to serialize the POJO into a JSON string and writes it to the HttpServletResponse output stream.

## The "406 Not Acceptable" Error
If the Client demands Accept: application/xml, but your application does not have an XML converter registered (e.g., you forgot to add the jackson-dataformat-xml dependency), Spring panics.

It cannot fulfill the contract. It cannot send JSON because the client explicitly refused it. It cannot send XML because it doesn't know how. It throws a 406 Not Acceptable status code.

# The Interception Pipeline
In a complex application, you rarely want the request to go straight to the Controller. You usually need to check things first:
- Is the user logged in?
- Is the request compressed?
- Does this request contain a malicious SQL injection pattern?

To handle this, we use the Interception Pipeline. There are two major players here, and developers often confuse them: Servlet Filters and Spring Interceptors.

## The Outer Shell: Servlet Filters
Filters are part of the standard Servlet API (not specific to Spring).
- Location: They sit outside the DispatcherServlet. They are the first line of defense.
- Scope: They surround the entire Servlet Container entry point.
- Capability: They work with raw ServletRequest and ServletResponse objects. They do not know anything about Spring Controllers, Beans, or Exceptions.
- Use Cases:
    - Compression (GZIP).
    - CORS setup (Cross-Origin Resource Sharing).
    - Security (Spring Security acts largely as a chain of Filters).
    - Character Encoding (UTF-8).

Think of the Filter as the Security Guard at the Mall Entrance. They check everyone coming in, but they don't know which specific shop the person is visiting.

## The Inner Circle: Spring HandlerInterceptors
Interceptors are specific to the Spring Framework.
- Location: They sit inside the DispatcherServlet, specifically wrapping the HandlerExecutionChain.
- Scope: They operate only on requests that are successfully mapped to a Spring Controller.
- Capability: Crucially, the preHandle method of an Interceptor gives you access to the Object handler. This means the Interceptor knows exactly which Java Controller class and method is about to be called.
- Use Cases:
    - Authorization checks specific to a Controller.
    - Adding global data to the Model (e.g., adding a user's avatar URL to every page).
    - Logging execution time of specific API endpoints.

Think of the Interceptor as the receptionist's assistant. They know exactly who the visitor is meeting (the Controller) and can prepare the meeting room.

## The Execution Flow
1. Request In -> Filter Chain -> DispatcherServlet -> HandlerMapping -> Interceptor preHandle -> Controller.
2. Response Out -> Controller -> Interceptor postHandle -> DispatcherServlet -> Filter Chain -> Response.

# The Reactive Stack (Spring WebFlux) (Overview)

## The Problem: The "Thread-per-Request" Ceiling
We established that Tomcat has a thread pool (e.g., 200 threads). In a standard Spring MVC app, when a thread makes a database call or calls an external API, it blocks.
- The Scenario: You call a database. It takes 2 seconds to respond.
- The Consequence: For those 2 seconds, that Java Thread sits idle. It cannot do anything else. It holds onto memory stack space but utilizes 0% CPU.
- The Failure State: If you receive 201 simultaneous requests, and the first 200 are "waiting" for the database, your server becomes unresponsive, even though the CPU is sitting at 1% usage.

## The Solution: The Event Loop (Non-Blocking I/O)
Spring WebFlux (the reactive stack) abandons the "Thread-per-Request" model. Instead, it typically runs on Netty, which uses an Event Loop model (similar to Node.js).
The Architecture:
- Instead of 200 threads, the server starts a very small number of threads (usually equal to the number of CPU cores, e.g., 4 or 8).
- These threads never block.

The Flow:
1. Request A comes in. Thread 1 picks it up.
2. Request A needs the database. Thread 1 sends the signal to the DB driver: "Call me back when you have the data."
3. Crucially: Thread 1 does not wait. It immediately turns around and picks up Request B.
4. Later, the DB responds. An "event" is triggered. Thread 1 (or any available thread) picks up the context of Request A and finishes the job.

## The Tooling: Project Reactor (Mono and Flux)
Because we can't write linear code anymore (since we can't wait for results), we need a way to express "data that will arrive in the future." We use Project Reactor.
- Mono<T>: A publisher that produces 0 or 1 item. (Like a CompletableFuture or a Promise).
    - Use case: findById, save.
- Flux<T>: A publisher that produces 0 to N items. (A stream of data).
    - Use case: findAll, infinite data streams.

In Spring WebFlux, your controller does not return User. It returns Mono<User>. You are returning a promise of a User, not the User itself.