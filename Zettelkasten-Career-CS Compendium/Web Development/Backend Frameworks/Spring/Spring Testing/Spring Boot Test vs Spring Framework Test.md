---
tags:
  - FORMAT
  - Spring
---
# Spring Boot Test vs. Spring Framework Test

When testing applications developed using the **Spring Framework** or **Spring Boot**, two major testing strategies come into play: **Spring Boot Test** and **Spring Framework Test**. While they both aim to test Spring-based applications, they have different purposes, configurations, and ease of use.

## Spring Boot Test

* **Definition**: `@SpringBootTest` is an annotation provided by Spring Boot that is specifically designed for integration testing Spring Boot applications. It helps run tests that need to load the full Spring context with an embedded web server (if required), making it useful for end-to-end and integration tests.

* **Key Features**:

  * **Full Application Context**: Loads the entire application context, including beans, configurations, and properties. It's ideal for integration testing where you need the full Spring environment.

  * **Embedded Server Support**: Supports starting an embedded server (like Tomcat or Jetty) for testing web applications in an environment similar to production.

  * **Configuration**: Automatically configures the necessary Spring beans and dependencies.

  * **Test Scope**: More suitable for integration tests or higher-level tests, where you need to ensure that different parts of the system work together.

* **Example**:

```java
@SpringBootTest
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    void testService() {
        assertThat(myService.doSomething()).isEqualTo("Expected result");
    }
}
```

*   
  **Advantages**:

  * Provides a comprehensive testing environment.

  * Ideal for testing the behavior of the application as a whole, including web layers, database connections, etc.

  * Automatically configures components like a mock database, embedded server, and more.

* **Disadvantages**:

  * Can be slower than unit tests because it loads the full context.

  * Not suitable for low-level or unit testing due to the overhead of loading the entire Spring context.

---

## Spring Framework Test

* **Definition**: Spring Framework Test refers to the testing capabilities provided by the **Spring Framework**. These tests, generally marked with annotations like `@ContextConfiguration` or `@Autowired`, focus on testing specific components or beans in isolation. These are more granular tests compared to Spring Boot tests.

* **Key Features**:

  * **Selective Bean Loading**: Tests a smaller portion of the Spring context. You can configure it to load only the necessary parts of the application context rather than the full Spring Boot configuration.

  * **Non-Embedded Server**: Does not require an embedded server like Spring Boot does. More suitable for testing components that do not rely on a web server (e.g., service layer, DAO).

  * **Flexibility**: You can define a custom Spring configuration to test specific components, making it more flexible for unit tests or component-level tests.

* **Example**:

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = {MyServiceConfig.class})
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testService() {
        assertThat(myService.doSomething()).isEqualTo("Expected result");
    }
}
```

*   
  **Advantages**:

  * Faster tests compared to `@SpringBootTest` because the context is smaller and doesn't involve loading an embedded server.

  * Ideal for unit testing and testing individual Spring beans or components.

  * More control over the context configuration, allowing you to test in isolation.

* **Disadvantages**:

  * Doesn't automatically load the entire application context, so you might have to configure it yourself.

  * Less suited for integration tests where you want to test the entire application flow.

---

## Key Differences Between Spring Boot Test and Spring Framework Test

| Aspect | Spring Boot Test | Spring Framework Test |
| ----- | ----- | ----- |
| **Scope** | Loads the full application context, including embedded server. | Loads a smaller, custom part of the application context. |
| **Usage** | Best suited for integration and end-to-end tests. | Best suited for unit tests or component-level tests. |
| **Configuration** | Automatically configures Spring beans and embedded servers. | Requires manual configuration or specific context setups. |
| **Embedded Server** | Supports embedded server (e.g., Tomcat). | Does not support embedded server; focuses on individual beans or services. |
| **Test Speed** | Slower due to loading the full context. | Faster due to more granular context loading. |
| **Annotations** | `@SpringBootTest` | `@ContextConfiguration`, `@RunWith(SpringRunner.class)` |

---

## Conclusion

* **Use `@SpringBootTest`** when you want to test your Spring Boot application in a way that mimics the real running environment. It’s ideal for integration or end-to-end tests that require the entire Spring context, including web and database layers.

* **Use Spring Framework Test** when you want more control over the test environment and focus on testing individual Spring beans or components without the overhead of loading the full application context. It’s suitable for unit tests and lower-level testing.

