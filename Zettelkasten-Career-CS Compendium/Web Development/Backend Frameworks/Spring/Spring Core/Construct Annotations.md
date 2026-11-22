---
tags:
  - Spring
  - LEAF
---
Construct annotations help define lifecycle methods that execute when an object is initialized or destroyed. These annotations are used for dependency injection, bean initialization, and cleanup.

# @PostConstruct (Jakarta & Java EE)

- Runs after the bean is created and dependencies are injected.
- Used for initialization logic (e.g., loading data, opening resources).
- Works in Spring-managed beans.

## Example:

```java
import jakarta.annotation.PostConstruct;
import org.springframework.stereotype.Component;

@Component
public class PostConstructExample {
    @PostConstruct
    public void init() {
        System.out.println("Executed after dependency injection");
    }
}
```

## When does it execute?

- After the bean is fully initialized (constructor + dependencies injected).
- Before the bean is ready for use by other components.


# @PreDestroy (Jakarta & Java EE)

- Runs before the bean is destroyed.
- Used for cleanup logic (e.g., closing database connections, stopping threads).
- Works in Spring-managed beans.


## Example:

```java
import jakarta.annotation.PreDestroy;
import org.springframework.stereotype.Component;

@Component
public class PreDestroyExample {
    @PreDestroy
    public void cleanup() {
        System.out.println("Executed before bean destruction");
    }
}
```

## When does it execute?

- Right before the bean is destroyed by the Spring container.
- Does not run for @Bean methods marked as prototype scope (only singleton beans are managed by Spring).


---

# @Bean(initMethod, destroyMethod) (Spring)

- Used in Java-based Spring configuration.
- Specifies custom initialization and destruction methods.

## Example:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    @Bean(initMethod = "init", destroyMethod = "cleanup")
    public MyBean myBean() {
        return new MyBean();
    }
}

class MyBean {
    public void init() {
        System.out.println("Custom init method executed");
    }

    public void cleanup() {
        System.out.println("Custom destroy method executed");
    }
}
```

## When does it execute?

- initMethod: Runs after bean creation and dependency injection.
- destroyMethod: Runs before bean destruction.

---

# InitializingBean and DisposableBean (Spring Interfaces)

- InitializingBean: Defines afterPropertiesSet() for initialization.
- DisposableBean: Defines destroy() for cleanup.
- Not recommended for general use** (tightly couples beans to Spring).


## Example:

```java
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.stereotype.Component;

@Component
public class BeanLifecycleExample implements InitializingBean, DisposableBean {
    @Override
    public void afterPropertiesSet() {
        System.out.println("Bean initialized using InitializingBean");
    }

    @Override
    public void destroy() {
        System.out.println("Bean destroyed using DisposableBean");
    }
}
```

## When does it execute?

- afterPropertiesSet(): After dependency injection.
- destroy(): Before the bean is destroyed.

---

# Best Practices

- Use @PostConstruct and @PreDestroy for lifecycle methods in Spring Beans.
- Use @Bean(initMethod, destroyMethod) for Java-based configuration.
- Avoid InitializingBean and DisposableBean due to tight coupling with Spring.