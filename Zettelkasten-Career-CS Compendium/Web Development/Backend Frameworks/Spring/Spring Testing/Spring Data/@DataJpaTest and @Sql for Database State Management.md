---
tags:
  - FORMAT
  - Spring
---
# @DataJpaTest and @Sql for Database State Management

In Spring-based applications, testing the data layer effectively is crucial. To facilitate this, Spring provides specialized annotations like **`@DataJpaTest`** and **`@Sql`** for managing database state during tests. These annotations help isolate the database interactions and ensure a clean and consistent environment for each test.

## @DataJpaTest

The **`@DataJpaTest`** annotation is part of the Spring Boot test suite and is used to test the **JPA repositories** in isolation. It configures the test context with only the necessary beans for JPA testing, such as:

* **Embedded in-memory database** (by default H2, but you can configure others).

* **`EntityManager`** and **`JpaRepository`** beans.

* **Spring Data JPA** integration.

It is commonly used to test **repository layers** (i.e., interactions with the database).

### 1 Usage of @DataJpaTest

When you annotate a test class with `@DataJpaTest`, Spring Boot will:

* Set up an in-memory database (H2 by default).

* Configure JPA-related beans such as `EntityManager` and `JpaRepository`.

* Enable the transaction management necessary for tests.

* Automatically roll back the transaction after each test, ensuring the database remains in a clean state between tests.

Here's an example of how to use `@DataJpaTest` to test a JPA repository.

```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testFindByEmail() {
        // Given: Add a user to the repository
        User user = new User("john.doe@example.com", "John Doe");
        userRepository.save(user);

        // When: Retrieve the user by email
        Optional<User> found = userRepository.findByEmail("john.doe@example.com");

        // Then: Ensure the user is found and their name matches
        assertTrue(found.isPresent());
        assertEquals("John Doe", found.get().getName());
    }
}
```

### 2 Key Points about @DataJpaTest:

* **Transaction Management**: By default, `@DataJpaTest` tests run in a transaction that is rolled back after each test method, ensuring a clean database state.

* **Auto-Configuration**: Spring Boot will auto-configure an embedded database (like H2) for your tests.

* **Minimal Context**: Only the JPA-related beans are initialized, so it's faster than loading the full Spring context.

**Note**: Since `@DataJpaTest` loads only the JPA-related context, it is not suitable for testing services, controllers, or other layers in your application.

## @Sql

The **`@Sql`** annotation is used for running SQL scripts before or after a test method or class is executed. This is useful when you need to:

* Preload the database with specific data before tests run.

* Clean up or modify the database after a test is finished.

With `@Sql`, you can easily set up or tear down database states without manually interacting with the repository layer.

### 1 Usage of @Sql

You can use `@Sql` at both the **method level** or **class level** to execute SQL statements. It can reference SQL scripts or inline SQL queries.

#### 1 Method-Level @Sql Example

```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    @Sql(statements = "INSERT INTO user (email, name) VALUES ('test@example.com', 'Test User')")
    public void testFindUserByEmail() {
        Optional<User> user = userRepository.findByEmail("test@example.com");

        assertTrue(user.isPresent());
        assertEquals("Test User", user.get().getName());
    }
}
```

In this example:

* The `@Sql` annotation is used to insert a record into the `user` table before the test method runs.

* The test then fetches the user by email and asserts that the name is correct.

#### 2 Class-Level @Sql Example

You can also use `@Sql` at the class level to execute SQL scripts before or after all tests in the class:

```java
@RunWith(SpringRunner.class)
@DataJpaTest
@Sql(scripts = "/test-schema.sql", executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD)
@Sql(scripts = "/cleanup.sql", executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD)
public class UserRepositoryTest {
    // Tests will be executed with schema and cleanup scripts
}
```

In this example:

* **`@Sql(scripts = "/test-schema.sql")`**: Runs the `test-schema.sql` script before the test methods.

* **`@Sql(scripts = "/cleanup.sql", executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD)`**: Runs the `cleanup.sql` script after each test method.

### 2 Using Inline SQL Queries

Instead of referencing external SQL scripts, you can use inline SQL queries with `@Sql`. This is useful for simple setup or teardown logic:

```java
@Test
@Sql(statements = "INSERT INTO user (email, name) VALUES ('inline@example.com', 'Inline User')")
public void testInlineSql() {
    Optional<User> found = userRepository.findByEmail("inline@example.com");
    assertTrue(found.isPresent());
    assertEquals("Inline User", found.get().getName());
}
```

### 3 Execution Phases in @Sql

The `@Sql` annotation provides two execution phases:

* **`BEFORE_TEST_METHOD`** (default): Executes the SQL scripts before the test method runs.

* **`AFTER_TEST_METHOD`**: Executes the SQL scripts after the test method runs.

Example with both phases:

```java
@RunWith(SpringRunner.class)
@DataJpaTest
@Sql(scripts = "data.sql", executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD)
@Sql(scripts = "cleanup.sql", executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD)
public class UserRepositoryTest {
    // Test methods here
}
```

## Combining @DataJpaTest and @Sql

You can use `@DataJpaTest` and `@Sql` together to have both automatic configuration of JPA repositories and customized SQL execution for setting up or tearing down test data.

Example:

```java
@RunWith(SpringRunner.class)
@DataJpaTest
@Sql(statements = "INSERT INTO user (email, name) VALUES ('johndoe@example.com', 'John Doe')")
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testFindByEmail() {
        Optional<User> found = userRepository.findByEmail("johndoe@example.com");
        assertTrue(found.isPresent());
        assertEquals("John Doe", found.get().getName());
    }
}
```

In this example:

* The `@DataJpaTest` annotation configures the repository layer.

* The `@Sql` annotation is used to insert a specific user into the database before the test runs.

* After the test runs, the database will be rolled back automatically, ensuring a clean state for other tests.

## Advantages of @DataJpaTest and @Sql

* **Simplified Repository Testing**: `@DataJpaTest` provides an easy way to test JPA repositories without configuring the full Spring context.

* **Database State Management**: `@Sql` allows you to run specific SQL scripts before or after tests, giving you fine-grained control over your test data.

* **Isolation**: Each test can have its own specific setup and teardown logic via `@Sql`, ensuring that tests don’t interfere with each other.

* **Transaction Rollback**: `@DataJpaTest` ensures that all database changes are rolled back after each test method, preventing side effects between tests.

## Considerations

* **Test Data Cleanliness**: Using `@Sql` for setting up and tearing down data can help ensure that tests don’t leave any lingering data in the database. However, be mindful of the complexity of SQL scripts.

* **Database Compatibility**: If you're using an in-memory database like H2, be aware that some database features (e.g., specific SQL functions or data types) may differ from your production database (e.g., PostgreSQL or MySQL). For full end-to-end testing, consider testing against a production-like database in the later stages of testing.

## Conclusion

* **`@DataJpaTest`** simplifies testing JPA repositories by configuring only the necessary components for data access, ensuring fast and isolated tests.

* **`@Sql`** provides powerful tools for managing database state, allowing you to set up or clean up data before and after each test or test class.

* Combining both annotations allows for efficient and organized integration tests with fully managed database state, ensuring reliable and repeatable tests for your persistence layer.

