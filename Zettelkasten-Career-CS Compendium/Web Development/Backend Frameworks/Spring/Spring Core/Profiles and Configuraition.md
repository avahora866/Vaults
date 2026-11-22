---
tags:
  - Spring
  - LEAF
---
# Spring Profiles

Spring Profiles allow you to define environment-specific configurations (e.g., `dev`, `test`, `prod`) and enable only relevant beans and configurations for a given environment.

## @Profile Annotation

* You can use `@Profile` on `@Component`, `@Configuration`, and `@Bean` definitions to load them conditionally.

```java
@Profile("dev")
@Service
public class DevDatabaseService implements DatabaseService {
    @Override
    public String getDatabaseConnection() {
        return "Connected to Dev Database";
    }
}
```

## Activating Profiles

* **Application Properties:** In `application.properties` or `application.yml`:

```
spring.profiles.active=dev
```

```
spring:
  profiles:
    active: dev
```

* **Programmatically:** Using `SpringApplication`:

```java
SpringApplication app = new SpringApplication(MyApplication.class);
app.setAdditionalProfiles("dev");
app.run(args);
```

* **Command Line:** When starting the application:

```
java -jar app.jar --spring.profiles.active=dev
```

---

# Property-Based Configurations

Property-based configurations allow you to externalize configuration values, making your application more flexible and easier to manage in different environments.

## @PropertySource

`@PropertySource` is used to load properties files into the Spring `Environment`.

```java
@Configuration
@PropertySource("classpath:application-dev.properties")
public class AppConfig {
}
```

## @Value Annotation

You can inject property values directly into your beans using `@Value`.

```java
@Value("${app.name}")
private String appName;
```

* **Default Values:** You can provide a default value using `:` syntax.

```java
@Value("${app.description:Default App Description}")
private String appDescription;
```

## @ConfigurationProperties

`@ConfigurationProperties` is useful for binding groups of related properties to POJOs.

```java
@ConfigurationProperties(prefix = "app")
public class AppConfigProperties {
    private String name;
    private String description;

    // Getters and Setters
}
```

* **Enable Binding:** You need to enable this feature using `@EnableConfigurationProperties` in a configuration class.

```java
@Configuration
@EnableConfigurationProperties(AppConfigProperties.class)
public class AppConfig {
}
```

* **Example Properties File (`application.properties`):**

```
app.name=MyApp
app.description=This is a sample application
```

---

# Configuration Classes

Instead of using XML configurations, you can use `@Configuration` classes to define beans programmatically.

```java
@Configuration
public class MyConfig {
    
    @Bean
    @Profile("dev")
    public DatabaseService devDatabaseService() {
        return new DevDatabaseService();
    }

    @Bean
    @Profile("prod")
    public DatabaseService prodDatabaseService() {
        return new ProdDatabaseService();
    }
}
```

* **@Bean:** Defines a method as a bean, which is managed by the Spring container.  
* **@Profile:** Combined with `@Bean`, this allows you to define environment-specific beans.

---

# Practical Use Case: Combining Profiles and External Properties

You might use `@Profile` to load specific beans for `dev` and `prod` environments while using `@ConfigurationProperties` to manage external configuration.

* **Development (`application-dev.properties`):**

```
app.name=MyApp-Dev
app.db.url=jdbc:h2:mem:devdb
```

* **Production (`application-prod.properties`):**

```
app.name=MyApp-Prod
app.db.url=jdbc:mysql://localhost/proddb
```

* **Configuration Class:**

```java
@Configuration
@PropertySource("classpath:application-${spring.profiles.active}.properties")
public class AppConfig {

    @Bean
    @ConfigurationProperties(prefix = "app.db")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }
}
```

---

# Summary

* **@Profile**: Enable beans and configurations for specific environments.  
* **@PropertySource**: Load external properties files.  
* **@Value**: Inject individual properties into fields.  
* **@ConfigurationProperties**: Map a set of related properties to a POJO.  
* **@Configuration**: Define configuration classes and beans programmatically.
