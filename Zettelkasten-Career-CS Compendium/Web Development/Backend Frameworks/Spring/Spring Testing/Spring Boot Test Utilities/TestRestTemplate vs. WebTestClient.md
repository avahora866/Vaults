---
tags:
  - FORMAT
  - Spring
---
# TestRestTemplate vs. WebTestClient

Both **TestRestTemplate** and **WebTestClient** are used in Spring-based applications to perform HTTP requests in tests, particularly for testing REST APIs. However, they cater to different testing needs and are part of different testing approaches in Spring.

Here's an overview of both and how they differ:

## TestRestTemplate

`TestRestTemplate` is a utility class provided by Spring for testing RESTful web services. It is part of the **Spring Boot Test** framework and is built around **RestTemplate**, a synchronous HTTP client.

### Key Features of `TestRestTemplate`:

1. **Synchronous HTTP Requests**: `TestRestTemplate` is based on **RestTemplate**, which is synchronous. This means each HTTP request is made sequentially and waits for the response before continuing.

2. **Simpler for Traditional Tests**: `TestRestTemplate` is typically used for integration tests in Spring Boot applications where synchronous, blocking HTTP requests are acceptable.

3. **Spring Boot Focused**: It’s specifically designed for use in **Spring Boot** tests, making it very useful in integration testing where Spring Boot applications are tested as a whole.

4. **No Need for WebFlux**: `TestRestTemplate` doesn't require the WebFlux module. It’s a good choice if you're not using reactive programming.

### Example of `TestRestTemplate` Usage:

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class MyControllerTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @LocalServerPort
    private int port;

    @Test
    public void testGetEndpoint() {
        ResponseEntity<String> response = restTemplate.getForEntity("http://localhost:" + port + "/api/my-endpoint", String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("Expected Content");
    }
}
```

In the example:

* `TestRestTemplate` is used to perform a GET request to the endpoint.

* The `@LocalServerPort` annotation gives the actual port number for the embedded server.

* `getForEntity()` sends a synchronous HTTP GET request and expects a `ResponseEntity` as the response.

### When to Use `TestRestTemplate`:

* If you are writing **integration tests** for a Spring Boot application that is not using reactive programming (i.e., not using Spring WebFlux).

* For **simple, synchronous HTTP requests** to test REST APIs.

* When your application uses **traditional Spring MVC** rather than Spring WebFlux for handling HTTP requests.

---

## WebTestClient

`WebTestClient` is a more modern and flexible alternative provided by **Spring WebFlux**. It is designed to work with both **Spring WebFlux** applications (reactive applications) and traditional Spring MVC applications. It supports both **synchronous** and **asynchronous** testing.

### Key Features of `WebTestClient`:

1. **Supports Reactive Programming**: While it can be used with traditional Spring MVC, `WebTestClient` shines when used in **reactive applications** built with Spring WebFlux. It can handle asynchronous, non-blocking I/O operations, making it suitable for testing reactive APIs.

2. **Non-blocking I/O**: Unlike `TestRestTemplate`, `WebTestClient` can be used for making **asynchronous HTTP requests** and is suitable for applications that require high performance and scalability.

3. **Flexible and Powerful**: It provides a fluent API for making requests, asserting results, and handling responses. It allows testing both the server-side logic and client-side logic for HTTP-based communication.

4. **No Need for Separate Threading**: `WebTestClient` does not require separate threads for non-blocking operations, making it easier to work with reactive code in tests.

5. **Works with both WebFlux and non-WebFlux Applications**: It’s suitable for **reactive** (WebFlux) as well as **non-reactive** (MVC) testing.

### Example of `WebTestClient` Usage:

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class MyControllerTest {

    @Autowired
    private WebTestClient webTestClient;

    @LocalServerPort
    private int port;

    @Test
    public void testGetEndpoint() {
        webTestClient.get().uri("http://localhost:" + port + "/api/my-endpoint")
                     .exchange()
                     .expectStatus().isOk()
                     .expectBody(String.class).value(body -> assertThat(body).contains("Expected Content"));
    }
}
```

In the example:

* `WebTestClient` is used to perform a GET request to the endpoint.

* `uri()` specifies the endpoint to test.

* `exchange()` is used to execute the request, which returns a `ClientResponse` object.

* `expectStatus().isOk()` asserts the HTTP status code.

* `expectBody(String.class)` checks the body of the response.

### When to Use `WebTestClient`:

* If you are working with a **reactive application** built with **Spring WebFlux**, `WebTestClient` is the natural choice.

* When you need **asynchronous, non-blocking** testing, particularly for performance-sensitive or high-concurrency applications.

* For testing both **synchronous** and **asynchronous** APIs with flexible and powerful assertions.

* Even when testing **traditional Spring MVC** applications, `WebTestClient` offers a better, more feature-rich experience than `TestRestTemplate`.

---

## Key Differences Between `TestRestTemplate` and `WebTestClient`

| Feature | TestRestTemplate | WebTestClient |
| ----- | ----- | ----- |
| **Use Case** | Ideal for testing **traditional Spring MVC** applications (synchronous requests). | Ideal for testing **reactive Spring WebFlux** applications (asynchronous requests). |
| **HTTP Client Type** | Synchronous, blocking HTTP requests. | Supports both synchronous and asynchronous, non-blocking requests. |
| **Concurrency Model** | Blocking I/O, sequential requests. | Non-blocking I/O, reactive and asynchronous requests. |
| **Response Handling** | Uses `ResponseEntity` to capture HTTP responses. | Provides fluent API with `exchange()` to capture and assert HTTP responses. |
| **Integration with WebFlux** | Not designed for WebFlux; designed for Spring Boot MVC. | Built to work seamlessly with **WebFlux** applications, but also supports Spring MVC. |
| **Ease of Use** | Simpler to use for basic RESTful tests in Spring Boot MVC. | More flexible, with powerful support for both synchronous and asynchronous tests. |
| **Asynchronous Support** | No support for asynchronous/non-blocking tests. | Full support for reactive, non-blocking I/O with `WebTestClient`. |
| **API Style** | Uses methods like `getForEntity()`, `postForObject()`. | Provides a fluent API with methods like `get()`, `uri()`, `exchange()`, `expectStatus()`. |

---

## When to Choose Each Tool?

* **Choose `TestRestTemplate`**:

  * If you're building or testing a **traditional Spring Boot MVC** application.

  * For **synchronous** HTTP tests in **non-reactive** Spring Boot applications.

  * If your project doesn't need to work with **reactive programming** or **non-blocking I/O**.

* **Choose `WebTestClient`**:

  * If you're testing a **Spring WebFlux** (reactive) application.

  * When you need **asynchronous** or **non-blocking** HTTP requests.

  * If you require more flexibility in your tests, especially for **synchronous and asynchronous** behavior.

  * If you prefer a more modern and feature-rich testing framework with powerful assertions.

---

## Conclusion

* **TestRestTemplate** is ideal for simple, synchronous HTTP requests in **traditional Spring MVC** applications, making it great for testing non-reactive REST APIs.

* **WebTestClient**, on the other hand, is more versatile, offering **synchronous and asynchronous** support for testing both **Spring MVC** and **WebFlux** applications. It is especially suited for **reactive applications** with non-blocking I/O.

Both tools are powerful, but the choice between them largely depends on whether you’re working with **reactive** programming or not.

