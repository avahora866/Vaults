---
tags:
  - FORMAT
  - Spring
---
# MockMvc for Testing Spring MVC Controllers

`MockMvc` is a powerful utility provided by **Spring Test** to simulate HTTP requests and test the behavior of **Spring MVC controllers**. It allows you to test your Spring web layer in a **Spring MVC environment** without needing to start a real HTTP server, making it ideal for testing your controllers in isolation.

With `MockMvc`, you can perform integration tests of your Spring controllers, verify their behavior, and assert responses to ensure that your web endpoints function as expected.

## Key Features of MockMvc

* **Simulates HTTP requests**: You can send mock HTTP requests to your controllers and get mock responses.

* **No real HTTP server**: Unlike integration tests that require starting up a full server, `MockMvc` runs entirely in memory.

* **Asserts on Responses**: You can check if the responses from controllers match your expectations (e.g., status code, headers, content).

* **Flexible Configuration**: `MockMvc` works seamlessly with Spring’s `@WebMvcTest`, `@SpringBootTest`, or `@ContextConfiguration`, depending on the testing context.

---

## How to Use MockMvc

To use `MockMvc`, you generally follow these steps:

1. **Set up the test context** (using annotations like `@WebMvcTest` or `@SpringBootTest`).

2. **Inject `MockMvc`** into the test class.

3. **Perform HTTP requests** using `MockMvc` methods like `perform()`, `get()`, `post()`, etc.

4. **Assert on the response** to ensure that the controller's behavior is correct.

---

## Basic Example of MockMvc Usage

### Step 1: Define a Simple Controller

Let's say you have a simple controller that you want to test:

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/greet")
    public ResponseEntity<String> greet(@RequestParam String name) {
        return ResponseEntity.ok("Hello, " + name);
    }
}
```

In this example, `MyController` defines a simple REST endpoint `/greet` that accepts a query parameter `name` and returns a greeting.

### Step 2: Test the Controller Using MockMvc

Now, you want to test this controller with `MockMvc`.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;  // Inject MockMvc into the test class

    @Test
    public void testGreet() throws Exception {
        // Perform a GET request to /api/greet with a query parameter "name=John"
        mockMvc.perform(get("/api/greet")
                .param("name", "John"))
                .andExpect(status().isOk())  // Check if status is OK (200)
                .andExpect(content().string("Hello, John"));  // Assert the response content
    }
}
```

In this test:

* **`@WebMvcTest(MyController.class)`**: This annotation configures the test to focus on the `MyController` class. It starts an embedded Spring context that only contains the MVC layer, meaning it doesn’t load the entire application context.

* **`@Autowired MockMvc`**: The `MockMvc` instance is automatically injected by Spring. It provides methods for sending requests and verifying responses.

* **`perform(get("/api/greet").param("name", "John"))`**: The `perform()` method simulates an HTTP GET request to the `/api/greet` endpoint with the query parameter `name=John`.

* **`andExpect(status().isOk())`**: This assertion checks that the HTTP status of the response is `200 OK`.

* **`andExpect(content().string("Hello, John"))`**: This checks that the body of the response contains the string `"Hello, John"`.

## MockMvc Methods for HTTP Requests

MockMvc provides several methods for different HTTP operations like GET, POST, PUT, DELETE, etc. Here are some of the most common ones:

* **GET**: Simulates an HTTP GET request.

```java
mockMvc.perform(get("/api/greet").param("name", "John"));
```

*   
  **POST**: Simulates an HTTP POST request. You can send data in the request body, for example, when submitting forms or JSON.

```java
mockMvc.perform(post("/api/users")
              .contentType(MediaType.APPLICATION_JSON)
              .content("{\"name\": \"John\", \"age\": 30}"));
```

*   
  **PUT**: Simulates an HTTP PUT request to update an existing resource.

```java
mockMvc.perform(put("/api/users/{id}", 1)
              .contentType(MediaType.APPLICATION_JSON)
              .content("{\"name\": \"John\", \"age\": 31}"));
```

*   
  **DELETE**: Simulates an HTTP DELETE request to remove a resource.

```java
mockMvc.perform(delete("/api/users/{id}", 1));
```

## Common Assertions with MockMvc

MockMvc allows you to perform various assertions on the HTTP response. Some of the most common assertions are:

* **Status Code Assertions**:

  * `andExpect(status().isOk())`: Checks for a 200 OK status.

  * `andExpect(status().isCreated())`: Checks for a 201 Created status.

  * `andExpect(status().isNotFound())`: Checks for a 404 Not Found status.

* **Content Assertions**:

  * `andExpect(content().string("Expected Content"))`: Asserts the content of the response.

  * `andExpect(content().json("{\"key\":\"value\"}"))`: Asserts the response body contains a valid JSON string.

* **Header Assertions**:

  * `andExpect(header().string("Content-Type", "application/json"))`: Checks for a specific header value in the response.

* **JSON Path Assertions**: Use this for more advanced assertions on JSON responses.

  * `andExpect(jsonPath("$.name").value("John"))`: Asserts that the JSON response contains `"name": "John"`.

---

## Example with JSON and POST Request

Here’s an example that tests a `POST` request where you send JSON data and assert the response:

### Step 1: Modify Controller to Handle POST Request

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @PostMapping("/user")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        // Simulating user creation
        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }
}
```

### Step 2: Define a User POJO

```java
public class User {
    private String name;
    private int age;

    // Getters and setters
    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getters and setters
}
```

### Step 3: Test the Controller with MockMvc

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testCreateUser() throws Exception {
        // Create a User object in JSON format
        String userJson = "{\"name\": \"John\", \"age\": 30}";

        // Perform a POST request with JSON content
        mockMvc.perform(post("/api/user")
                .contentType(MediaType.APPLICATION_JSON)
                .content(userJson))
                .andExpect(status().isCreated())  // Status should be 201 Created
                .andExpect(jsonPath("$.name").value("John"))  // Assert the name field in the response
                .andExpect(jsonPath("$.age").value(30));  // Assert the age field in the response
    }
}
```

In this test:

* We create a `User` object in JSON format.

* We use the `post()` method to simulate a POST request to the `/api/user` endpoint, passing the user data in the request body.

* We use assertions to verify the response status and the content of the response (JSON).

---

## Benefits of Using MockMvc

* **No Need for a Real HTTP Server**: You don't need to start a web server to test your controllers. Everything is done in memory.

* **Isolation**: You can focus purely on testing the controller logic without involving other layers like the service or repository.

* **Comprehensive Testing**: You can test various HTTP methods, response codes, headers, and content, including JSON responses.

* **Performance**: Tests are fast because they don't involve a real network or server, making it easier to run multiple tests quickly.

---

## Conclusion

`MockMvc` is an essential tool for testing **Spring MVC controllers** without the need to deploy a real server. It allows you to simulate HTTP requests, verify responses, and test your controllers in isolation. By using `MockMvc`, you can test different HTTP methods, status codes, content, and headers, ensuring that your Spring MVC controllers are behaving as expected in various scenarios.

