---
tags:
  - FORMAT
  - Spring
---

In **Spring Boot testing**, the `@MockBean` and `@SpyBean` annotations are used to create mock or spy beans that can be injected into the **Spring application context** for integration testing purposes. These annotations are provided by **Spring Boot Test** to facilitate mocking and spying on beans directly within the Spring context.

Let's dive into both annotations, their differences, and their use cases.

---

# @MockBean Annotation

The `@MockBean` annotation is used to create and inject a mock bean into the **Spring ApplicationContext**. It replaces a bean of the specified type in the application context with a mock instance. This is useful in **integration tests** where you want to mock certain dependencies (like services or repositories) in the application while testing other components of the system.

## Key Features:

* **Mocks the Bean**: It mocks a real Spring bean, so when the system calls this bean during testing, it will interact with the mock object instead of the real one.

* **Part of Spring Context**: The mock bean is added to the Spring context, replacing the actual bean.

* **Ideal for External Dependencies**: It's typically used for mocking dependencies like service layers, repositories, or third-party libraries, making tests more isolated and easier to control.

## Example of `@MockBean`:

```java
@SpringBootTest
public class MyServiceTest {

    @MockBean
    private MyRepository myRepositoryMock;  // This is the mock bean that replaces the real repository

    @Autowired
    private MyService myService;  // The service under test, which will use the mocked repository

    @Test
    public void testServiceMethod() {
        // Stubbing: Define behavior of mock repository
        when(myRepositoryMock.getData()).thenReturn("Mocked Data");

        // Call the method under test
        String result = myService.getData();

        // Verify the result
        assertEquals("Mocked Data", result);
        
        // Verify that the method was called on the mock
        verify(myRepositoryMock).getData();
    }
}
```

In this example:

* The `@MockBean` annotation is used to create a mock of `MyRepository`.

* The mock repository is injected into the Spring context, replacing any real `MyRepository` bean.

* The behavior of the mock is stubbed, and the service (`MyService`) uses the mock during the test.

* `verify()` is used to check that the mock's method was called.

## Use Cases for `@MockBean`:

* Mocking **external dependencies** or **data layers** like repositories or REST clients in **integration tests**.

* Isolating the unit of work you want to test by replacing certain beans in the Spring context with mocks.

* Testing interactions between beans without invoking the actual logic of their dependencies.

---

# @SpyBean Annotation

The `@SpyBean` annotation is similar to `@MockBean`, but instead of creating a fully mocked version of a bean, it creates a **spy** on the bean. A spy allows you to **partially mock** a bean, meaning you can mock only specific methods of a real object while preserving the behavior of others. It’s useful when you want to mock certain behaviors but still maintain the actual logic of some methods.

## Key Features:

* **Partial Mocking**: It allows for partial mocking of a real bean. The methods of the real bean are executed unless you explicitly define behavior for specific methods.

* **Preserves Real Logic**: Unlike `@MockBean`, which creates a completely mocked version of the bean, `@SpyBean` allows the real logic to execute unless explicitly stubbed.

* **Used for Spying on Beans in Spring Context**: It is ideal when you need to test an object’s actual behavior, but with some mocked methods.

## Example of `@SpyBean`:

```java
@SpringBootTest
public class MyServiceTest {

    @SpyBean
    private MyRepository myRepositorySpy;  // This is a spy bean on the real repository

    @Autowired
    private MyService myService;  // The service under test

    @Test
    public void testServiceMethod() {
        // Stubbing: Define behavior for a method of the spy object
        doReturn("Mocked Data").when(myRepositorySpy).getData();

        // Call the method under test
        String result = myService.getData();

        // Verify the result
        assertEquals("Mocked Data", result);
        
        // Verify that the method was called on the spy
        verify(myRepositorySpy).getData();
    }
}
```

In this example:

* The `@SpyBean` annotation is used to spy on `MyRepository`, so it is not fully mocked.

* The method `getData()` is partially mocked using `doReturn()` to return `"Mocked Data"`.

* The rest of the behavior of the `MyRepository` bean is still intact and executed as is.

## Use Cases for `@SpyBean`:

* When you want to **partially mock** a real bean and control specific methods, but still keep the behavior of other methods intact.

* For testing **interactions** with real beans, where you want to verify if the real methods are being called but with some specific behavior modified.

* Ideal for testing **integrations** where the majority of the bean's logic is correct, but you want to mock certain aspects of its behavior.

---

# Differences Between `@MockBean` and `@SpyBean`

| Feature | @MockBean | @SpyBean |
| ----- | ----- | ----- |
| **Behavior** | Creates a fully mocked version of the bean. | Creates a spy, allowing partial mocking of the real bean. |
| **Methods** | All methods are mocked. | Real methods are called unless explicitly stubbed. |
| **Use Case** | Ideal for mocking **external dependencies** like repositories or services. | Ideal for **partial mocking** where most behavior is real, but some methods are stubbed. |
| **Method Invocation** | Methods return the mocked value, no real method execution. | Real method behavior is executed unless a stub is set. |
| **Test Scope** | Replaces the bean with a mock in the Spring context. | Wraps the real bean with a spy in the Spring context. |

---

# When to Use `@MockBean` and `@SpyBean`

* **Use `@MockBean`**:

  * When you need to **mock** an entire bean and control its behavior completely.

  * If you're testing **components** that depend on external systems or services (e.g., REST clients, repositories) and want to replace them with mocks to isolate the test.

  * If you want to replace a bean entirely with a mock for testing purposes.

* **Use `@SpyBean`**:

  * When you need to **partially mock** a real bean (i.e., retain the actual behavior of some methods but mock others).

  * If you are testing a **real bean** but need to mock specific behaviors or interactions within it, especially when the majority of the logic should remain unchanged.

  * When you want to **verify** that a real method was called on a bean, but also change the behavior of certain methods in a controlled way.

---

# Conclusion

* **`@MockBean`**: Replaces a bean in the Spring context with a fully mocked version, ideal for isolating tests from dependencies and controlling all behavior of the mocked bean.

* **`@SpyBean`**: Wraps a real bean with a spy, allowing partial mocking where some methods are stubbed, and the others run with their real logic. This is useful when you need to test the real behavior but also mock specific methods for testing purposes.

Both annotations are extremely useful in Spring Boot integration testing, but the choice between them depends on whether you need full control over the mocked behavior or want to retain some real logic while controlling specific interactions.

