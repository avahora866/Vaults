---
tags:
  - FORMAT
  - Spring
---
# Embedded Server for Integration Testing

In the context of **Spring Boot integration testing**, an **embedded server** refers to running a lightweight web server within the application during testing. This allows you to test your web layer (controllers, services, etc.) without needing to deploy the application to a full-fledged external server. The embedded server runs in the same process as the tests, making it faster and easier to execute integration tests.

## Key Features of an Embedded Server in Testing

* **Runs in-memory**: It operates entirely within the test process and does not require deploying to an external server.

* **No Need for External Setup**: You don't need to manually configure and manage a separate server.

* **Realistic Environment**: It provides a more realistic environment for testing because it simulates the behavior of a full application.

* **Fast Test Execution**: Since the server is embedded and starts with the test context, tests can run faster compared to using a full external server.

Spring Boot provides built-in support for embedded servers like **Tomcat**, **Jetty**, or **Undertow**, and you can configure the embedded server to run within the test environment automatically.

## Why Use an Embedded Server for Integration Testing?

1. **Simplified Testing Environment**:

   * By using an embedded server, you can run integration tests in the same process, without the overhead of deploying your application to an external server. It makes integration testing much easier and faster.

2. **Realistic Test Conditions**:

   * With an embedded server, you are testing your application in an environment that closely matches production conditions, including HTTP request handling, routing, and error handling.

3. **No Need for Additional Infrastructure**:

   * It eliminates the need for setting up and managing separate infrastructure for running tests (like a standalone server or container), which can simplify the setup for testing in continuous integration (CI) pipelines.

4. **Parallel Testing**:

   * You can execute multiple integration tests in parallel, each with its own embedded server instance, which reduces test execution time and avoids resource contention.

---

## Types of Embedded Servers in Spring Boot

Spring Boot supports three common embedded servers, all of which can be used for integration testing:

1. **Tomcat** (Default in Spring Boot):

   * A widely used Java-based web server.

2. **Jetty**:

   * A lighter and faster alternative to Tomcat. It's a more flexible server and is suitable for high-performance environments.

3. **Undertow**:

   * A non-blocking, high-performance web server. It's known for being lightweight and fast, especially in environments with high concurrency.

Spring Boot automatically configures an embedded web server (default is Tomcat) when you include web-related dependencies (like `spring-boot-starter-web`) in your project.

---

## How Embedded Servers Are Used for Integration Testing

In **Spring Boot integration tests**, you typically use **`@SpringBootTest`** combined with an embedded server to test your web application in a real environment. When you use `@SpringBootTest`, it automatically starts up the embedded server to handle HTTP requests during the test.

Here’s how it works:

1. **Start the Embedded Server**: When you run a test with `@SpringBootTest`, Spring Boot starts up the application context and initializes the embedded web server.

2. **Run Tests Against the Server**: Your tests can now make HTTP requests to the embedded server, interact with controllers, and verify that the responses are correct.

3. **Stop the Server After Test**: The embedded server is automatically stopped after the tests finish executing, and no external infrastructure is needed.

---

## Example of Using Embedded Server in Tests

Here’s an example of a typical integration test in Spring Boot using an embedded server:

### Define a Controller

Let’s start by defining a simple Spring MVC controller that you want to test.

```java
@RestController
@RequestMapping("/api")
public class GreetingController {

    @GetMapping("/greet")
    public ResponseEntity<String> greet(@RequestParam String name) {
        return ResponseEntity.ok("Hello, " + name);
    }
}
```

### Write an Integration Test

Now, we’ll write an integration test to test this controller using the embedded server.

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // Start embedded server on a random port
public class GreetingControllerTest {

    @LocalServerPort
    private int port; // Injects the random port assigned to the embedded server

    @Autowired
    private TestRestTemplate restTemplate;  // Inject TestRestTemplate to make HTTP requests

    @Test
    public void testGreet() {
        String url = "http://localhost:" + port + "/api/greet?name=John";

        // Perform an HTTP GET request using TestRestTemplate
        ResponseEntity<String> response = restTemplate.getForEntity(url, String.class);

        // Verify the response
        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals("Hello, John", response.getBody());
    }
}
```

### Explanation of the Test:

1. **`@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)`**:

   * The `webEnvironment` attribute tells Spring Boot to start the embedded web server (Tomcat by default) with a random port.

   * `@SpringBootTest` initializes the entire Spring context, including the web server, and makes it available for the test.

2. **`@LocalServerPort`**:

   * This annotation injects the random port number assigned to the embedded server into the `port` field. This is necessary since the port is dynamically assigned when you use `SpringBootTest.WebEnvironment.RANDOM_PORT`.

3. **`TestRestTemplate`**:

   * This is a Spring Boot utility that allows you to make HTTP requests to the embedded server during the test. It works similarly to `RestTemplate` but is designed for testing purposes.

4. **Making HTTP Requests**:

   * We use `TestRestTemplate.getForEntity()` to make a GET request to the `greet` endpoint, passing a `name` parameter in the URL.

5. **Assertions**:

   * We assert that the HTTP status code is `200 OK` and the response body is `"Hello, John"`.

---

## Benefits of Using an Embedded Server for Integration Testing

* **Fast and Efficient**:

  * Since the server is embedded and doesn’t require external deployment, integration tests run much faster.

* **Simplified Configuration**:

  * No need to configure and manage a separate web server for testing. Spring Boot handles it for you.

* **Realistic Testing**:

  * Tests interact with the actual Spring web layer, ensuring that the controllers, filters, and other web components are working as expected.

* **Isolation**:

  * The tests run in isolation from the production environment, which allows you to focus on the functionality of the code you are testing.

* **Flexible Testing Environment**:

  * You can use `SpringBootTest.WebEnvironment.RANDOM_PORT` or `SpringBootTest.WebEnvironment.DEFINED_PORT` to control how the embedded server is started (random or a fixed port).

* **No Dependencies on External Servers**:

  * You don't need to rely on external server configurations, which can be challenging in CI/CD pipelines. The embedded server is self-contained.

---

## Common Annotations for Embedded Server Testing

1. **`@SpringBootTest`**:

   * This annotation is used for running integration tests with a fully initialized Spring context. It automatically starts an embedded server for web-related tests.

2. **`@LocalServerPort`**:

   * This annotation is used to inject the port number of the embedded server. It’s useful for dynamically determining the port to send HTTP requests to during the test.

3. **`@WebMvcTest`**:

   * This annotation is used when you want to test just the web layer (controllers) without starting the entire Spring Boot context or embedded server. It is often used with **MockMvc** for unit tests.

4. **`@TestConfiguration`**:

   * This annotation allows you to define additional configurations specific to your test environment, such as custom beans or profiles, without affecting the main application configuration.

---

## Conclusion

Using an **embedded server** for integration testing in **Spring Boot** provides a fast, efficient, and realistic way to test your web layer, such as controllers and APIs, in a contained environment. It removes the need for deploying to external servers and simplifies the testing process by handling HTTP requests and responses entirely in-memory. This approach ensures that your tests are both isolated and comprehensive, covering end-to-end functionality in a real server environment.

