---
tags:
  - Spring
  - FORMAT
---
Spring Testing involves verifying the functionality of Spring applications, ensuring that the components work correctly and integrate well. It primarily uses **JUnit** and **Mockito**. Here are the main types of testing in Spring:

# Unit Testing

Focuses on individual components like services and controllers without loading the entire Spring context. We use **Mockito** for mocking dependencies.

Example: Testing a Service Layer

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public User getUserById(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    }
}
```

Unit Test for `UserService`:

```java
@RunWith(MockitoJUnitRunner.class)
public class UserServiceTest {

    @InjectMocks
    private UserService userService;

    @Mock
    private UserRepository userRepository;

    @Test
    public void testGetUserById() {
        User user = new User();
        user.setId(1L);
        user.setName("John Doe");

        Mockito.when(userRepository.findById(1L)).thenReturn(Optional.of(user));

        User result = userService.getUserById(1L);

        assertEquals("John Doe", result.getName());
    }
}
```

This test:

- Mocks `UserRepository`.
- Tests `getUserById` without loading the full Spring context.

# Integration Testing

Verifies interactions between components, such as Service and Repository layers, with a real database or in-memory database like **H2**.

Example:

```java
@SpringBootTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
public class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testGetUserByIdIntegration() {
        User user = new User();
        user.setName("Jane Doe");
        userRepository.save(user);

        User fetchedUser = userService.getUserById(user.getId());

        assertEquals("Jane Doe", fetchedUser.getName());
    }
}
```

This test:

- Loads the Spring context.
- Interacts with the real database.

# Controller Testing

Uses `MockMvc` to simulate HTTP requests, verifying controller endpoints without starting a web server.

Example:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}
```

Controller Test:

```java
@WebMvcTest(UserController.class)
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void testGetUserById() throws Exception {
        User user = new User();
        user.setId(1L);
        user.setName("John Doe");

        Mockito.when(userService.getUserById(1L)).thenReturn(user);

        mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("John Doe"));
    }
}
```

This test:

- Uses `MockMvc` to simulate HTTP requests.
- Mocks `UserService`.

# Testing Annotations

- `@SpringBootTest`: Loads the entire Spring context.
- `@WebMvcTest`: Loads only web layer components.
- `@MockBean`: Replaces a Spring bean with a Mockito mock.
- `@DataJpaTest`: Configures JPA components with an in-memory database.
- `@RunWith(MockitoJUnitRunner.class)`: Initializes Mockito annotations.

# Summary

- **Unit Testing**: Fast, isolated tests using Mockito.
- **Integration Testing**: Tests multiple layers with real dependencies.
- **Controller Testing**: Uses MockMvc to simulate HTTP requests.
- **Annotations**: Manage context loading and mocking behavior.

Spring Testing ensures your application is robust, maintainable, and free from regression issues. If you need more examples or explanations, let me know!