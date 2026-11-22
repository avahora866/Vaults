---
tags:
  - FORMAT
  - Spring
---
# `@RunWith(SpringRunner.class)` vs. `@ExtendWith(SpringExtension.class)`

Both `@RunWith(SpringRunner.class)` and `@ExtendWith(SpringExtension.class)` are used to integrate **Spring Testing** with **JUnit**. These annotations enable Spring's testing features like dependency injection, transaction management, and context loading in test classes. The difference between them primarily arises from the versions of **JUnit** they are associated with.

## `@RunWith(SpringRunner.class)`

**`@RunWith(SpringRunner.class)`** is used in **JUnit 4** to indicate that the Spring testing support should be applied to a test class. The `SpringRunner` is a JUnit runner that provides the necessary integration between Spring and JUnit 4\.

### Key Features of `@RunWith(SpringRunner.class)`:

* **JUnit 4 Integration**: It allows you to run a JUnit 4 test with the Spring context configuration and feature set.

* **Test Context Setup**: It tells JUnit to initialize the Spring context for the test, including loading beans and managing dependencies (e.g., `@Autowired`, `@MockBean`).

* **Test Lifecycle Management**: Handles lifecycle management such as context initialization, dependency injection, and cleanup.

### Usage Example:

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = MyAppConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testServiceMethod() {
        assertThat(myService.doSomething()).isEqualTo("Expected result");
    }
}
```

In the example:

* `@RunWith(SpringRunner.class)` tells JUnit 4 to use Spring's test runner.

* The `@ContextConfiguration(classes = MyAppConfig.class)` annotation specifies the configuration class for the Spring context.

* Spring's dependency injection will be applied to inject the `MyService` bean into the test class.

### Why Use `@RunWith(SpringRunner.class)`?

* This annotation is used when you are working with **JUnit 4**. Since JUnit 4 uses the concept of runners, `SpringRunner` is the runner that integrates Spring's test features into JUnit 4 tests.

* It enables Spring features like dependency injection, transaction management, and context initialization within the test.

---

## `@ExtendWith(SpringExtension.class)`

**`@ExtendWith(SpringExtension.class)`** is used in **JUnit 5** (Jupiter) to integrate Spring's testing framework. It is the JUnit 5 equivalent of `@RunWith(SpringRunner.class)` in JUnit 4\. **`SpringExtension`** is a JUnit 5 extension that provides similar functionality to `SpringRunner` but is designed for the newer, more flexible JUnit 5 framework.

### Key Features of `@ExtendWith(SpringExtension.class)`:

* **JUnit 5 Integration**: It is specifically designed for **JUnit 5 (Jupiter)**. It integrates Spring's test context and lifecycle features with the JUnit 5 framework.

* **Test Context Setup**: Just like `@RunWith(SpringRunner.class)`, it manages the Spring application context, wiring dependencies like `@Autowired` and other Spring-specific behaviors.

* **Test Lifecycle Management**: It also handles lifecycle management, including context initialization, bean wiring, and cleanup of resources.

### Usage Example:

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = MyAppConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testServiceMethod() {
        assertThat(myService.doSomething()).isEqualTo("Expected result");
    }
}
```

In the example:

* `@ExtendWith(SpringExtension.class)` integrates Spring testing with JUnit 5\.

* `@ContextConfiguration(classes = MyAppConfig.class)` loads the specified Spring configuration.

* The test class will have Spring's dependency injection applied, and the Spring context will be initialized.

### Why Use `@ExtendWith(SpringExtension.class)`?

* This annotation is required when you are using **JUnit 5** (Jupiter). Unlike JUnit 4, which uses runners, JUnit 5 uses extensions to provide additional functionality, and `SpringExtension` is the extension responsible for integrating Spring's features.

* JUnit 5 offers more flexibility, such as better support for parallel test execution, nested tests, and dynamic tests. `@ExtendWith(SpringExtension.class)` ensures that Spring's features are available within this modern test framework.

---

## Key Differences Between `@RunWith(SpringRunner.class)` and `@ExtendWith(SpringExtension.class)`

| Aspect | `@RunWith(SpringRunner.class)` | `@ExtendWith(SpringExtension.class)` |
| ----- | ----- | ----- |
| **JUnit Version** | JUnit 4 | JUnit 5 (Jupiter) |
| **Type** | JUnit 4 **runner** | JUnit 5 **extension** |
| **Functionality** | Integrates Spring into JUnit 4 tests by using a test runner. | Integrates Spring into JUnit 5 tests by using an extension. |
| **Test Context Setup** | Loads the Spring context and injects dependencies. | Loads the Spring context and injects dependencies. |
| **Test Lifecycle Management** | Manages lifecycle features such as context initialization. | Manages lifecycle features such as context initialization. |
| **Use Case** | Used with JUnit 4 tests (typically in legacy projects). | Used with JUnit 5 tests (in newer projects or modern setups). |

---

## When to Use Each?

* **Use `@RunWith(SpringRunner.class)`** when you are working with **JUnit 4** in your project. This annotation allows Spring to integrate with the older JUnit version and enables dependency injection, context loading, and other Spring-specific features.

* **Use `@ExtendWith(SpringExtension.class)`** when you are using **JUnit 5** (Jupiter). This is the modern way to integrate Spring with JUnit 5, and it works with the extension model introduced in JUnit 5 for more flexible test configurations.

---

## Conclusion

* **`@RunWith(SpringRunner.class)`** is used in **JUnit 4** to enable Spring's testing capabilities, including context management and dependency injection.

* **`@ExtendWith(SpringExtension.class)`** is used in **JUnit 5** for the same purpose but leverages JUnit 5's extension mechanism, providing more flexibility and support for modern test practices.

* The choice between them depends on the version of JUnit you are using in your project. If you are working with JUnit 4, use `@RunWith(SpringRunner.class)`, and for JUnit 5, use `@ExtendWith(SpringExtension.class)`.

