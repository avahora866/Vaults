---
tags:
  - FORMAT
  - Spring
---
# Using H2 or Embedded Databases for Integration Tests

When performing **integration tests** in Spring applications, especially for **persistence** layers, it's often useful to run tests against an actual database to verify that your data layer works as expected. However, it's not ideal to use a production database for testing because of performance, reliability, and data consistency concerns. This is where **H2** and other **embedded databases** come into play.

## What is an Embedded Database?

An **embedded database** is a database that is embedded within an application and runs in the same process as the application. These databases are lightweight and typically used for development, testing, or small-scale production environments. Some popular embedded databases are:

* **H2**: A fast, in-memory database often used for testing.

* **Derby**: A relational database engine that can be embedded in Java applications.

* **HSQLDB**: Another lightweight relational database that can be embedded in applications.

For integration tests, **H2** is the most commonly used embedded database in the Spring ecosystem because it supports SQL features like MySQL and PostgreSQL, but without the overhead of configuring an external database server.

## Why Use H2 for Integration Testing?

* **In-Memory Operation**: H2 can run completely in-memory, which means it doesn’t persist data between test runs, ensuring that each test runs in a clean, isolated environment.

* **Lightweight and Fast**: H2 is fast to start up, run, and shut down, making it ideal for fast and reliable testing.

* **SQL Compatibility**: H2 supports SQL syntax similar to that of other relational databases like PostgreSQL, MySQL, and Oracle, so it's an ideal candidate for testing SQL-based applications.

* **No Need for External Resources**: Unlike testing against a full-scale external database, H2 doesn't require database installation, making it very easy to set up and tear down.

## How to Configure H2 for Integration Tests

In Spring applications, you can easily configure **H2** for testing purposes by making use of Spring Boot’s built-in support for embedded databases.

### 1 Dependencies

To get started, you’ll need to include H2 and the Spring Boot Starter Data JPA dependencies in your `pom.xml` (for Maven) or `build.gradle` (for Gradle).

**Maven:**

```
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

**Gradle:**

```
testImplementation 'com.h2database:h2'
testImplementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```

### 2 Configure `application.properties` or `application.yml` for Tests

Next, configure your test-specific database settings in `application.properties` or `application.yml`. By using a separate configuration for tests, you can ensure that H2 is only used during tests and not in production.

**`src/test/resources/application.properties`:**

```
 Use H2 for testing
spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.datasource.hikari.maximum-pool-size=1

 Hibernate dialect for H2
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

 Show SQL logs for debugging
spring.jpa.show-sql=true

 Automatically create/drop tables in the test database
spring.jpa.hibernate.ddl-auto=create-drop
```

This configuration does the following:

* **`spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1`**: This specifies that H2 should run in-memory with the database named `testdb`. The `DB_CLOSE_DELAY=-1` ensures that the database does not close when the last connection is closed (so the database persists throughout the test run).

* **`spring.jpa.hibernate.ddl-auto=create-drop`**: This tells Hibernate to automatically create and drop the schema at the start and end of each test.

### 3 Running Integration Tests with H2

Once you've configured the database, you can now write integration tests using H2. Spring Boot will automatically configure the embedded H2 database for you when running tests.

**Example Test Class:**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testCreateUser() {
        // Create a new user and save to the database
        User user = new User("john.doe@example.com", "John Doe");
        userRepository.save(user);

        // Fetch the user from the database
        Optional<User> found = userRepository.findByEmail("john.doe@example.com");
        
        // Assert that the user exists
        assertTrue(found.isPresent());
        assertEquals("John Doe", found.get().getName());
    }
}
```

In this example:

* The `@SpringBootTest` annotation ensures that the Spring context is loaded and the database is configured as specified.

* The `@Transactional` annotation ensures that the changes are rolled back after the test completes, providing a clean slate for each test.

### 4 In-Memory Database for Isolation

Each test case will run against the **in-memory H2 database**, so you don’t need to worry about leftover data between tests. The **`ddl-auto=create-drop`** setting ensures the database schema is created and dropped automatically for each test method.

## Using H2 Profiles to Separate Test and Production Databases

It's important to ensure that the **H2 database** is used only for testing and not in production environments. You can achieve this by using **Spring Profiles**.

**`application.properties`:**

```
 Default properties (for production)
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.database-platform=org.hibernate.dialect.MySQL5Dialect

 Test-specific properties (activated with @Profile("test"))
---
spring.profiles: test
spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=create-drop
```

To activate the **test profile** in your tests, use the `@ActiveProfiles` annotation:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@ActiveProfiles("test")
public class UserRepositoryTest {
    // Test code here
}
```

## Advantages of Using H2 for Integration Tests

* **Speed**: H2 runs entirely in memory, making it much faster than testing with external databases.

* **Isolation**: Each test runs in an isolated environment, ensuring no shared state or data between tests.

* **Easy Setup**: You don’t need to set up and configure an external database server.

* **Automatic Schema Management**: With Hibernate's `ddl-auto=create-drop`, the schema is automatically created and dropped for each test, ensuring a clean state.

## Considerations When Using H2 for Integration Testing

While H2 is great for fast and isolated testing, there are some considerations:

* **Database Compatibility**: H2 is similar to other relational databases like MySQL and PostgreSQL but may have some differences in SQL dialects and behaviors. It's important to verify that your application works with the actual production database, especially when using specific database features.

* **In-Memory Limitations**: Since H2 is an in-memory database, it’s not suitable for persistent data storage or for applications that need large amounts of data. Ensure that your tests don't rely on features or configurations that are specific to persistent databases.

## Conclusion

Using an **H2 database** or other **embedded databases** for integration tests is a great way to test your persistence layer without the overhead of setting up and managing external databases. With Spring Boot’s automatic configuration, you can easily set up H2 for integration testing, ensuring fast, isolated, and reliable tests.

* **H2** is ideal for testing because of its lightweight, in-memory operation and fast startup.

* Ensure **test configurations** are separate from production by using **Spring Profiles**.

* Consider potential differences in SQL dialects when using H2, especially when your application is designed to run on a different database in production.