---
tags:
  - FORMAT
  - Spring
---
# Using Mock Objects with Mockito

**Mockito** is a popular mocking framework for Java, mainly used in unit tests to create mock objects, stub methods, and verify interactions in tests. It is often used in combination with frameworks like JUnit to facilitate the testing of individual components by replacing their dependencies with mock objects. This approach allows you to isolate the behavior of the component under test and focus on its logic without involving real dependencies, which may be slow, complex, or unavailable during testing.

## What Are Mock Objects?

A **mock object** is a simulated object that mimics the behavior of real objects in a controlled way. They allow you to:

* **Isolate components** in your system during testing.

* **Stub methods** to return specific values, bypassing complex or slow functionality like database access or network calls.

* **Verify interactions** with your objects (e.g., checking if a method was called, how many times it was called, etc.).

Mockito helps in creating mock objects for testing purposes, making it easier to write unit tests by isolating code dependencies.

## Key Mockito Concepts

1. **Mocking**: Creating mock objects that simulate the behavior of real dependencies.

2. **Stubbing**: Configuring mock objects to return specific values when methods are called.

3. **Verification**: Checking whether certain methods on mock objects were called and verifying the number of interactions.

4. **Spying**: Wrapping real objects to spy on their behavior, allowing for partial mocking.

## Basic Mockito Syntax

1. **Mock Creation**: The core feature of Mockito is creating mock objects using `Mockito.mock()` or `@Mock`.

```java
MyService myServiceMock = Mockito.mock(MyService.class);
```

2.   
   **Stubbing**: Once you have a mock object, you can specify what values it should return when its methods are called. This is known as **stubbing**.

```java
Mockito.when(myServiceMock.getData()).thenReturn("Mocked Data");
```

3.   
   **Verifying Interactions**: After the code runs, you can verify whether specific methods were called on the mock.

```java
Mockito.verify(myServiceMock).getData();
```

---

## Example: Using Mockito in Unit Tests

Let’s look at an example where we test a service that depends on a repository. We'll mock the repository to isolate the logic in the service.

### Step 1: Set Up the Class to Test

Imagine we have a simple service that depends on a `Repository` to fetch data:

```java
public class MyService {

    private final MyRepository myRepository;

    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }

    public String getData() {
        return myRepository.fetchData();
    }
}
```

Here, `MyService` depends on `MyRepository`. Instead of testing the actual `MyRepository` (which might involve a database call), we can mock `MyRepository` to isolate the service logic.

### Step 2: Create a Unit Test with Mockito

```java
import static org.mockito.Mockito.*;
import static org.junit.Assert.*;

import org.junit.Before;
import org.junit.Test;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

public class MyServiceTest {

    @Mock
    private MyRepository myRepositoryMock;

    private MyService myService;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this); // Initializes mock objects
        myService = new MyService(myRepositoryMock);
    }

    @Test
    public void testGetData() {
        // Stubbing: Define what happens when fetchData is called
        when(myRepositoryMock.fetchData()).thenReturn("Mocked Data");

        // Call method under test
        String result = myService.getData();

        // Assertion: Verify the result
        assertEquals("Mocked Data", result);

        // Verify interaction: Check if fetchData was called
        verify(myRepositoryMock).fetchData();
    }
}
```

In this test:

* We use the `@Mock` annotation to create a mock `MyRepository` object.

* In the `setUp` method, `MockitoAnnotations.initMocks(this)` initializes the mock objects.

* We use `when().thenReturn()` to stub the `fetchData()` method to return `"Mocked Data"`.

* We call `myService.getData()` and assert that the returned value is `"Mocked Data"`.

* Finally, we use `verify()` to check if `fetchData()` was indeed called on the mock object.

### Key Components in the Example:

* **Mockito.mock(Class classToMock)**: Creates a mock instance of the class.

* **when(mock.someMethod()).thenReturn(value)**: Stubs a method to return a specified value.

* **verify(mock).someMethod()**: Verifies that a method was called on the mock object.

* **@Mock**: Annotation that allows for the automatic creation of mock objects when used in combination with `MockitoAnnotations.initMocks(this)` or `MockitoJUnitRunner`.

---

## More Mockito Features

1. **Argument Matchers**: Mockito provides argument matchers to match arguments when verifying or stubbing method calls, allowing for more flexible testing.

    Example:

```java
when(myRepositoryMock.fetchData(anyString())).thenReturn("Mocked Data");
```

2.   
   Here, `anyString()` is an argument matcher that accepts any `String`.

3. **Multiple Stubbing**: You can stub a method to return different values on consecutive calls.

    Example:

```java
when(myRepositoryMock.fetchData()).thenReturn("First Call").thenReturn("Second Call");

System.out.println(myRepositoryMock.fetchData()); // Outputs: First Call
System.out.println(myRepositoryMock.fetchData()); // Outputs: Second Call
```

4.   
   **Exception Stubbing**: You can also stub methods to throw exceptions when called.

    Example:

```java
when(myRepositoryMock.fetchData()).thenThrow(new RuntimeException("Error occurred"));

try {
    myRepositoryMock.fetchData();
} catch (RuntimeException e) {
    assertEquals("Error occurred", e.getMessage());
}
```

5.   
   **Partial Mocking (Spying)**: Sometimes, you need to mock only certain methods of a real object. Mockito allows you to spy on real objects, which means calling real methods while mocking others.

    Example:

```java
List<String> list = new ArrayList<>();
List<String> spyList = spy(list);

spyList.add("Test");
verify(spyList).add("Test"); // Verifies that add() was called
```

6.   
   **Mocking Final Classes and Methods**: By default, Mockito doesn’t mock `final` methods or classes. However, you can enable this feature using the **Mockito extension**.

    Example:

```java
@Mock
private FinalClass finalClassMock;
```

7.   
   To enable mocking of final methods, use the `mock-maker-inline` option in `mockito-extensions`.

---

## Benefits of Using Mockito in Unit Tests

1. **Isolation**: Mockito allows you to isolate the unit of work by mocking its dependencies, ensuring that the test focuses solely on the component’s behavior.

2. **No External Dependencies**: You can avoid the need to interact with external systems like databases, web services, or filesystems during testing.

3. **Easy Setup**: Mockito’s fluent API makes it simple to create mock objects, stub method responses, and verify interactions.

4. **Control Over Behavior**: You can control the behavior of mock objects to test various scenarios, such as handling exceptions or verifying edge cases.

5. **Improves Test Speed**: By mocking slow or resource-intensive dependencies (like databases), tests run much faster.

---

## Conclusion

Mockito is a powerful and easy-to-use framework that helps you write unit tests by mocking dependencies. With mock objects, you can isolate the class under test, control the behavior of dependencies, and verify interactions, making unit tests more focused and reliable.

By using **mocking**, **stubbing**, and **verification** features, you can simulate different scenarios in your tests and ensure that the code behaves as expected in various conditions without the need for complex setup or integration with external systems.

