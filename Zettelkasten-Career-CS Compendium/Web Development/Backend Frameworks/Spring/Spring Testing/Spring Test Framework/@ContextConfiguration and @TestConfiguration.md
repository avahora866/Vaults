---
tags:
  - FORMAT
  - Spring
---
# `@ContextConfiguration` and `@TestConfiguration`

Both **`@ContextConfiguration`** and **`@TestConfiguration`** are important annotations used in Spring testing to help configure and manage the application context during tests. While both deal with context configuration, they serve slightly different purposes and use cases.

## `@ContextConfiguration`

The **`@ContextConfiguration`** annotation is used to specify the configuration classes or XML files that should be loaded to configure the **Spring application context** for a test class. It allows you to customize the application context for your test scenarios by defining the necessary beans, configurations, or profiles.

It can be applied to a test class to load one or more configuration files or classes, which Spring will use to set up the application context for the test.

### Key Features:

* **Configures the Spring Context for Tests**: It tells Spring where to look for the configuration files or classes that should be used to initialize the Spring application context.

* **Flexible Configuration**: You can use Java-based configuration classes or XML configuration files to define your Spring context.

* **Used with JUnit Tests**: Typically used in integration tests where you need a customized Spring context.

### Usage Example:

```java
@ContextConfiguration(classes = MyConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testService() {
        assertThat(myService.doSomething()).isEqualTo("Expected result");
    }
}
```

In the example:

* `@ContextConfiguration(classes = MyConfig.class)` tells Spring to load the `MyConfig` class to initialize the application context for the test.

* The `MyService` bean is injected and tested within the context of this configuration.

### Key Attributes of `@ContextConfiguration`:

* **`classes`**: Specifies the Java configuration classes to load. If you're using Java-based configuration, you use this attribute to define the configuration classes.

* **`locations`**: Specifies the location of XML configuration files. This is useful if your configuration is defined in XML rather than Java configuration.

* **`initializers`**: Allows you to specify Spring `ApplicationContextInitializer` classes to initialize the context further before it is used in tests.

Example with XML configuration:

```java
@ContextConfiguration(locations = "classpath:/test-context.xml")
public class MyServiceTest {
    // Test logic here
}
```

---

## `@TestConfiguration`

The **`@TestConfiguration`** annotation is a specialized form of `@Configuration` that is **only** used for **test-specific configuration**. It is used to define beans and configurations that are specific to tests, without affecting the main application configuration.

It allows you to define additional beans or configurations required only for the scope of the test. Beans defined within `@TestConfiguration` are **only available in the test context**, and they are not part of the main application context unless explicitly imported.

### Key Features:

* **Test-Specific Configuration**: It allows you to define additional configurations specifically for the testing environment. The beans defined in `@TestConfiguration` will only be available in the test context.

* **Scoped to Tests**: Unlike `@Configuration`, which is used for application-wide configurations, `@TestConfiguration` is scoped to the test class.

* **Flexibility**: You can define mock beans, service overrides, or other test-specific configurations that don't affect the main application.

### Usage Example:

```java
@TestConfiguration
public class TestConfig {

    @Bean
    public MyService myService() {
        return new MyServiceMock();  // Mock or test-specific implementation
    }
}

@ContextConfiguration(classes = TestConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testService() {
        assertThat(myService.doSomething()).isEqualTo("Mocked result");
    }
}
```

In this example:

* `TestConfig` is marked with `@TestConfiguration`, which means it contains test-specific configurations, such as a mocked version of `MyService`.

* The test class uses `@ContextConfiguration(classes = TestConfig.class)` to load the test-specific configuration provided by `TestConfig`.

### Key Points about `@TestConfiguration`:

* Beans defined within `@TestConfiguration` are **not loaded into the main Spring context** but only into the test context, making it a powerful way to customize beans for testing.

* You can use it to define mocks, stubs, or custom configurations needed only for the test scenario.

---

## Comparison Between `@ContextConfiguration` and `@TestConfiguration`

| Aspect | `@ContextConfiguration` | `@TestConfiguration` |
| ----- | ----- | ----- |
| **Purpose** | Configures the Spring context for the test class, either using Java config or XML files. | Defines beans or configurations that are **test-specific** and do not affect the main application context. |
| **Scope** | The configuration is part of the **global test context** (i.e., it can affect the entire test lifecycle). | The configuration is scoped **only to the test context**, providing test-specific beans. |
| **Use Case** | Use it to load configuration classes or XML files for the application context in a test. | Use it to define beans or configurations that are specific to testing (like mocks or test data). |
| **Annotation Type** | Applied to a test class to configure the Spring context. | Applied to a configuration class to define test-specific beans. |
| **Bean Accessibility** | Beans from `@ContextConfiguration` are available to the test class via `@Autowired`. | Beans defined in `@TestConfiguration` are only available to the test class and test context. |

---

## When to Use Each?

* **Use `@ContextConfiguration`** when you need to configure the Spring context for your tests by specifying the configuration files or classes that should be loaded. This is especially useful for integration tests where the entire application context is required.

* **Use `@TestConfiguration`** when you want to define **test-specific configurations** (such as mock beans, stubs, or special configurations) that should not interfere with your main application context. This is ideal for unit tests or isolated tests that require specialized beans for testing purposes.

---

## Conclusion

* **`@ContextConfiguration`** is a powerful annotation used to configure the Spring context for tests, letting you specify the classes or XML configuration files that should be used to initialize the application context.

* **`@TestConfiguration`** is a specialized configuration for testing, allowing you to define beans and configurations that are scoped only to the test context. It provides a flexible way to introduce custom behavior in tests without affecting the global application context.

By understanding how and when to use these annotations, you can better manage Spring's testing environment and create more effective, isolated tests that closely mirror your real application setup.

