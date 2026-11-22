---
tags:
  - FORMAT
  - Spring
---
# Transaction Management in Tests (`@Transactional`)

In Spring-based applications, managing transactions is crucial when performing database operations. During tests, **transaction management** ensures that changes to the database are handled correctly and that tests run in isolation. The `@Transactional` annotation in Spring is an essential tool for managing transactions during tests. It ensures that database changes are rolled back after a test is completed, maintaining a clean and consistent test environment.

## What is @Transactional?

The `@Transactional` annotation is used to define **transaction boundaries** in a Spring application. When applied to a method or class, it indicates that the method or class should be executed within a transaction. Any changes made to the database within a `@Transactional` method are committed at the end of the method, but in the case of a test, they can also be rolled back after the test completes.

When `@Transactional` is applied to a test method or class, it ensures that:

* A **transaction** is started before the method runs.

* Any changes to the database during the test are rolled back automatically after the test completes.

* The **transaction is isolated**, meaning it will not affect the database outside of the test.

This makes `@Transactional` ideal for testing scenarios where you want to modify the database during the test but ensure the changes do not persist between test runs.

## How `@Transactional` Works in Tests

By default, Spring **rolls back the transaction** at the end of a test method when `@Transactional` is used. This means that:

* All the changes made to the database during the test are discarded after the test completes.

* The test will not have any lasting effect on the database, ensuring that subsequent tests run in a clean environment.

This is particularly useful when writing integration tests, as it allows you to run tests that interact with a database without worrying about leaving stale data or affecting the test environment.

### 1 Example of Using `@Transactional` in a Test

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class UserServiceTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testCreateUser() {
        // Given: Create a new user object
        User user = new User("john.doe@example.com", "John Doe");

        // When: Save the user to the repository
        userService.save(user);

        // Then: Assert the user was saved correctly
        Optional<User> found = userRepository.findByEmail("john.doe@example.com");
        assertTrue(found.isPresent());
        assertEquals("John Doe", found.get().getName());
    }
}
```

In this example:

* The `@Transactional` annotation ensures that any database operations made during the `testCreateUser` method will be rolled back after the test completes.

* The database changes made by `userService.save(user)` will not persist, ensuring the database remains in a clean state after the test.

## Rollback Behavior

The **rollback behavior** of `@Transactional` in tests is very important:

* By default, Spring uses **automatic rollback** for tests annotated with `@Transactional`. This means that once the test method completes, any changes made to the database during the test are **rolled back**.

* The rollback occurs even if the test passes, which ensures that the database is left in a consistent state for the next test.

### 1 Explicit Rollback Control

If you want to **disable** the automatic rollback for a particular test, you can configure it by using the `@Rollback` annotation.

For example, to disable rollback for a specific test:

```java
@Test
@Transactional
@Rollback(false)  // Disable rollback for this test
public void testPersistUser() {
    User user = new User("jane.doe@example.com", "Jane Doe");
    userRepository.save(user);
    // The user will be saved permanently in the database
}
```

*   
  **`@Rollback(false)`** ensures that the transaction **is not rolled back**, and the database changes persist.

* This might be useful when you want to check if data is persisted correctly and not rolled back.

## Transaction Propagation and Isolation Levels

Spring also provides the ability to configure **transaction propagation** and **isolation levels** within `@Transactional`:

* **Propagation** determines how transactions behave when nested within other transactions (e.g., whether the nested transaction should join the existing one or start a new one).

* **Isolation level** determines the degree to which a transaction is isolated from other transactions.

These configurations are more commonly used in production code but can also be applied in tests if you need to control how transactions behave.

For example, to set the propagation and isolation level for a test:

```java
@Test
@Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.READ_COMMITTED)
public void testTransactionalBehavior() {
    // Your test logic
}
```

*   
  **`Propagation.REQUIRES_NEW`**: Starts a new transaction even if one is already in progress.

* **`Isolation.READ_COMMITTED`**: Ensures that no other transaction can read data that is being modified by the current transaction.

These settings are typically not necessary for most tests, but they can be useful for testing complex transactional behavior.

## Advantages of Using @Transactional in Tests

1. **Database Isolation**: Each test runs in its own transaction, and any changes to the database are automatically rolled back, ensuring that tests don’t interfere with each other.

2. **Clean Test Environment**: Since the transaction is rolled back after each test, your tests always run in a clean database state.

3. **Simplified Setup**: You don’t have to manually clean up the database after each test, as Spring takes care of that for you.

4. **Automatic Rollback**: With `@Transactional`, you avoid leaving test data in the database, making it easier to maintain a clean environment for testing.

## Considerations When Using @Transactional in Tests

* **Performance**: Although `@Transactional` and rollback provide a clean environment for each test, rolling back transactions after every test can be relatively expensive in terms of performance, especially when working with large datasets. However, for most unit and integration tests, this performance overhead is generally acceptable.

* **Test Boundaries**: Tests that rely heavily on database interactions should be annotated with `@Transactional` to ensure that the database state is isolated for each test. However, it’s essential to ensure that the tests are meaningful and actually verify the expected behavior of your application logic, not just the database state.

* **Use with `@DataJpaTest`**: If you're using `@DataJpaTest`, the test is already wrapped in a transaction, so manually adding `@Transactional` is typically unnecessary.

## Transactional Behavior in Integration Tests

In **integration tests**, where you might be testing the full application context, `@Transactional` ensures that the database remains in a clean state after each test. However, in tests that span multiple layers (e.g., service, repository), `@Transactional` can be helpful to simulate real-world database interactions.

For example:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testCreateAndUpdateUser() {
        User user = new User("alice@example.com", "Alice");
        userService.save(user);

        user.setName("Alice Updated");
        userService.save(user);

        User found = userRepository.findById(user.getId()).get();
        assertEquals("Alice Updated", found.getName());
    }
}
```

Here, `@Transactional` ensures that:

* The database is rolled back after the test completes, keeping the environment clean.

* You don’t need to worry about manual cleanup or database state persistence.

## Conclusion

The **`@Transactional`** annotation in Spring is a powerful tool for managing database transactions in tests. It helps ensure that:

* Database changes made during the test are automatically rolled back.

* Each test is isolated and does not affect others by leaving behind data in the database.

* You can focus on writing tests for application logic without worrying about the underlying data.

By using `@Transactional` in combination with other test-related annotations like `@DataJpaTest`, `@Sql`, and `@Rollback`, you can manage database states efficiently and maintain a consistent, clean testing environment for your Spring applications.

