---
tags:
  - FORMAT
  - Spring
---
# Using Testcontainers for Real Database Integration

**Testcontainers** is a popular library that provides a simple and efficient way to create and manage **Docker containers** in tests. It is primarily used to spin up real instances of databases (and other services) in a containerized environment for integration testing. This approach allows you to run tests with actual database instances, ensuring your application works as expected in a real-world setup, while still maintaining test isolation and environment consistency.

Testcontainers is particularly useful when you want to test against databases like PostgreSQL, MySQL, or any other database without needing to configure them manually. It provides a **realistic environment** for testing while offering the benefits of automation, isolation, and easy cleanup.

## What is Testcontainers?

Testcontainers is a Java library that makes use of Docker to provide lightweight, throwaway containers for use in tests. It can be used to run real instances of databases, messaging systems, or any other service required for integration testing. Testcontainers ensures that services are only started for the duration of the test, and once the test finishes, they are shut down, leaving no trace.

In the context of database integration tests, Testcontainers allows you to:

* **Spin up a database container** (e.g., PostgreSQL, MySQL, etc.) as part of the test setup.

* Use a **real database instance** instead of an in-memory or mocked database, ensuring that you test with real database behavior (e.g., connection pooling, transaction management).

* **Ensure database isolation** between tests, as each test can run with its own instance.

## Why Use Testcontainers?

Traditional unit tests often use **in-memory databases** (like H2) to simulate real database behavior. However, in-memory databases can have limitations that do not fully represent production databases, leading to discrepancies between testing and production environments.

Testcontainers addresses these issues by:

* **Providing real database instances**: You can spin up a Docker container running a database like PostgreSQL, MySQL, or others, and connect your application to it just like you would in production.

* **Eliminating manual configuration**: No need to manually configure a local database or worry about database state between tests. Testcontainers does this automatically.

* **Offering environment consistency**: Testcontainers helps ensure your tests run consistently across different environments (locally, CI/CD pipelines, etc.) since the container is identical in all environments.

* **Database isolation**: Each test can run in its own isolated container, ensuring that there is no conflict or data leakage between tests.

## How Testcontainers Works

Testcontainers allows you to define containers programmatically and configure them to suit your testing needs. It leverages Docker to manage the lifecycle of the containers. When running tests, Testcontainers will:

* Start the container before the test.

* Run the test with the database container as part of the test environment.

* Clean up the container after the test completes.

The key idea is that each test runs in a fresh container with a clean database, ensuring isolation between tests.

## Using Testcontainers with Spring Boot

When using Testcontainers with Spring Boot, you can integrate it seamlessly to create **real database integration tests**. You’ll often use Testcontainers with Spring’s **`@SpringBootTest`** annotation for full application context loading while having the database managed in a container.

### 1 Steps to Use Testcontainers in Spring Boot

1. **Add Testcontainers Dependency**

First, you need to include the Testcontainers dependency in your project. If you are using **Maven**, add the following dependency:

```
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>1.17.6</version> <!-- Use the latest version -->
    <scope>test</scope>
</dependency>
```

For **Gradle**, add this to your `build.gradle`:

```
testImplementation 'org.testcontainers:testcontainers:1.17.6' // Use the latest version
```

Additionally, you may need the specific container dependencies, like for PostgreSQL or MySQL:

```
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <version>1.17.6</version> <!-- Same version as testcontainers -->
    <scope>test</scope>
</dependency>
```

2.   
   **Create a Testcontainer for the Database**

Create a Testcontainer instance in your test class. You can configure it to run a database like PostgreSQL, MySQL, etc. Here’s an example of how to set up a PostgreSQL container using Testcontainers:

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

@SpringBootTest
@Testcontainers
public class UserRepositoryTest {

    // Create a PostgreSQL container instance
    @Container
    public PostgreSQLContainer<?> postgresContainer = new PostgreSQLContainer<>("postgres:13.2")
            .withDatabaseName("testdb")
            .withUsername("user")
            .withPassword("password");

    @Test
    public void testDatabaseConnection() {
        // Test the connection to the real PostgreSQL container
        String jdbcUrl = postgresContainer.getJdbcUrl();
        String username = postgresContainer.getUsername();
        String password = postgresContainer.getPassword();
        
        // Use Spring Data JPA or JDBC to connect to the container and perform tests
    }
}
```

In this example:

* **`PostgreSQLContainer`**: This is a predefined Testcontainer class for running a PostgreSQL instance.

* **`@Container`**: Marks the container as a field that should be managed by Testcontainers.

* **`withDatabaseName`, `withUsername`, `withPassword`**: These methods configure the database name, username, and password for the PostgreSQL container.

3. **Configure Spring to Use the Testcontainer Database**

To use the Testcontainer database, you need to configure Spring Boot to connect to it. Spring Boot’s auto-configuration will automatically use the container’s JDBC URL, username, and password if you configure them in the `application.properties` or `application.yml` file.

Here’s an example of how to configure the connection:

```
spring.datasource.url=jdbc:postgresql://${postgresContainer.host}:${postgresContainer.getMappedPort(5432)}/testdb
spring.datasource.username=user
spring.datasource.password=password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
```

Note that `postgresContainer.host` and `postgresContainer.getMappedPort(5432)` will automatically resolve the dynamic values provided by the Testcontainer instance.

4. **Running Tests with Testcontainers**

Now, when you run your test, Testcontainers will automatically:

* Start a PostgreSQL container.

* Provide Spring Boot with the connection details (JDBC URL, username, password).

* Allow Spring Boot to interact with the database container just like a real PostgreSQL instance.

* Shut down the container after the test is finished.

### 2 Integration with Spring Data JPA

When using Spring Data JPA, Testcontainers allows you to perform integration tests that interact with the actual database. Here’s an example of how you would run an integration test using Spring Data JPA with Testcontainers.

```java
@SpringBootTest
@Testcontainers
public class UserRepositoryTest {

    @Container
    public PostgreSQLContainer<?> postgresContainer = new PostgreSQLContainer<>("postgres:13.2")
            .withDatabaseName("testdb")
            .withUsername("user")
            .withPassword("password");

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testSaveUser() {
        User user = new User("john.doe@example.com", "John Doe");
        userRepository.save(user);

        User foundUser = userRepository.findByEmail("john.doe@example.com").orElseThrow();
        assertEquals("John Doe", foundUser.getName());
    }
}
```

## Advantages of Using Testcontainers

* **Realistic Tests**: By using actual database instances in containers, you test with the same database type that your production system uses (e.g., PostgreSQL, MySQL), avoiding discrepancies with in-memory databases like H2.

* **Clean Database State**: Each test runs in an isolated container with a clean database, ensuring that tests do not interfere with one another.

* **Automatic Cleanup**: Testcontainers automatically removes the containers after the tests finish, so there’s no need to worry about manually cleaning up resources.

* **Cross-Environment Consistency**: Testcontainers ensures that your tests run consistently in different environments (local development, CI/CD pipelines, etc.), as the Docker container configuration is identical everywhere.

* **Docker Flexibility**: You can use Testcontainers for more than just databases (e.g., message brokers, caching systems, etc.), giving you a consistent way to test integration with other services.

## Conclusion

Using **Testcontainers** for real database integration tests is an excellent way to test your application’s data layer in an environment that closely resembles production. It allows you to:

* Spin up a real database container for each test.

* Automatically manage the lifecycle of containers.

* Ensure database isolation and cleanup between tests.

* Test with actual database behavior, avoiding limitations of in-memory databases.

By integrating Testcontainers with Spring Boot, you can easily set up full-stack integration tests that interact with real database systems, providing high confidence in your application’s database interactions.

