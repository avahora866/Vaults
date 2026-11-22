---
tags:
  - Spring
  - LEAF
---
Spring Runners are components in the Spring Framework that allow you to execute code at application startup. They are primarily used for initialization tasks, data seeding, or background processes after the Spring Boot application context has loaded.

# Types of Spring Runners:

1. `CommandLineRunner`

Runs after the application starts.

Accepts command-line arguments as a String array.

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class MyRunner implements CommandLineRunner {
    @Override
    public void run(String... args) {
        System.out.println("Application started with arguments: " + String.join(", ", args));
    }
}
```

1. `ApplicationRunner`

Similar to `CommandLineRunner`, but provides `ApplicationArguments` for easier argument handling.


```java
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class MyAppRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) {
        System.out.println("Application started. Options: " + args.getOptionNames());
    }
}
```

# Key Differences:

`CommandLineRunner` uses a raw String array for arguments.

`ApplicationRunner` provides an `ApplicationArguments` object for structured argument handling.

Both runners are executed after the application context loads and can be used for logging, initializing resources, or preloading data into the system.
