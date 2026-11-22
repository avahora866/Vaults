---
tags:
  - "#TRUNK"
  - Spring
---
# Inversion of Control (IoC) and the Spring Container
To understand Spring, you must first unlearn the traditional method of managing Java objects.

In standard Java programming, if Class A needs Class B to function, Class A is typically responsible for creating Class B.

$$Class \ A \xrightarrow{creates} Class \ B$$

This creates a **tight coupling**. If you wish to change Class B to a different implementation (e.g., swapping a Database Repository for a Mock Repository for testing), you must modify the code inside Class A.

**Inversion of Control (IoC)** flips this relationship. Instead of the object controlling its dependencies, that control is handed over (inverted) to an external entity—the **Spring Container**.

## The Spring Container (`ApplicationContext`)
Related Note: [[Application Context]]
The Spring Container is the engine of the framework. In Spring, we typically refer to this container via the `ApplicationContext` interface. Its responsibilities are strict and cyclical:
1. **Instantiate** objects.
2. **Configure** objects.
3. **Assemble** dependencies between objects.
4. **Manage** their entire lifecycle from creation to destruction.

When an object is managed by the Spring Container, we stop calling it a "Java Object" and start calling it a **Spring Bean**.

# Configuration Meta-Data
The Container is powerful, but it is not psychic. It does not know which objects should be Beans and how they relate to one another unless we provide **Configuration Meta-Data**.

There are three primary ways to provide this data to the container:
1. **XML Configuration** (Legacy, mostly deprecated in modern development).
2. **Annotation-based Configuration** (Component Scanning).
3. **Java-based Configuration** (`JavaConfig`).

## JavaConfig: `@Configuration` and `@Bean`
In this approach, you create a dedicated Java class that serves as the blueprint for the Container.
- **`@Configuration`**: This class annotation tells Spring, "This class contains bean definitions."
- **`@Bean`**: This method annotation tells Spring, "When this method is called, the return value should be registered as a Bean in the Application Context."

### Example: The Car and The Engine
```java
// 1. A simple dependency
public class ElectricEngine implements Engine {
    public void start() { System.out.println("Hummmmm..."); }
}

// 2. The dependent class
public class Car {
    private final Engine engine;

    // The Car needs an engine to be created
    public Car(Engine engine) {
        this.engine = engine;
    }
}

// 3. The Configuration (The Blueprint)
@Configuration
public class AppConfig {

    @Bean
    public Engine engine() {
        // We explicitly create the Engine instance here
        return new ElectricEngine();
    }

    @Bean
    public Car car() {
        // We call the method above. Spring intercepts this call
        // to ensure the specific lifecycle management of the bean.
        return new Car(engine());
    }
}
```

**The Crucial Detail:** Notice inside `car()` we call `engine()`. In standard Java, calling `engine()` twice would create two new `ElectricEngine` objects. However, because this is inside a `@Configuration` class, Spring uses a library called **CGLIB** to intercept that method call. If the `engine` bean already exists in the container, Spring returns the _existing_ instance rather than creating a new one. This ensures the **Singleton** behavior (which we will cover in depth later).

# Dependency Injection Types
Now that we know _where_ to define beans, we must discuss _how_ Spring places the dependencies into them. This is the **Injection** in Dependency Injection.

There are two primary valid types, and one "anti-pattern" type you will often see in tutorials but should avoid.

## Constructor Injection ( The Gold Standard)
This is the modern preferred method for 90% of scenarios. You provide the dependencies through the class constructor.

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    // The @Autowired annotation is optional here if there is only one constructor
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Why it is superior:**
- **Mandatory Dependencies:** You cannot instantiate `UserService` without a `UserRepository`. It prevents `NullPointerException` later in the code.
- **Immutability:** Notice the `final` keyword. Once the dependency is injected, it cannot be changed. This is safer for concurrent (multi-threaded) execution.
- **Testing:** In a unit test, you do not need Spring. You can simply call `new UserService(new MockRepository())`.

## Setter Injection
Here, you provide a public setter method that Spring calls _after_ the object is instantiated.

```java
@Service
public class NotificationService {
    private EmailSender emailSender;

    @Autowired
    public void setEmailSender(EmailSender emailSender) {
        this.emailSender = emailSender;
    }
}
```

**When to use it:**
- **Optional Dependencies:** If the dependency is not strictly required for the object to function (e.g., a logging integration that might be null).
- **Circular Dependencies:** If Bean A needs Bean B, and Bean B needs Bean A, constructor injection will fail (Chicken and Egg problem). Setter injection solves this because objects are created _before_ dependencies are injected.
## Field Injection (The Anti-Pattern)
```java
@Autowired
private UserRepository userRepository; // AVOID THIS
```
This is **bad practice**. It hides dependencies, makes the class impossible to test without Spring (reflection is required to set the field), and promotes violating the Single Responsibility Principle by making it too easy to add too many dependencies.

# Component Scanning (The "Implicit" Way)
Writing a `@Bean` method for every single class in your application (Controller, Service, Repository) is tedious and verbose. Spring provides a shortcut called **Component Scanning**.

Instead of listing beans in a config class, you annotate the classes themselves, and Spring scans your classpath to find them.
## The Stereotype Annotations
The generic annotation is **`@Component`**. It marks a Java class as a Spring Bean. However, Spring provides specialized versions of this annotation (Stereotypes) to indicate the _role_ of the class.
1. **`@Repository`**:
    - **Role:** Used for the Data Access Layer (DAO).
    - **Special Power:** It automatically translates raw database exceptions (like `SQLException`) into Spring's unchecked `DataAccessException` hierarchy. This decouples your service layer from the specific database technology.
2. **`@Service`**:
    - **Role:** Used for the Service Layer (Business Logic).
    - **Special Power:** Currently, it behaves exactly like `@Component`, but it adds semantic value for the reader and may gain special behavior in future Spring versions.
3. **`@Controller`** (and `@RestController`):
    - **Role:** Used for the Web Layer (handling HTTP requests).
    - **Special Power:** It tells Spring MVC to map URL requests to methods in this class.

### Comparison: Explicit vs. Implicit
|**Feature**|**JavaConfig (@Bean)**|**Component Scanning (@Component)**|
|---|---|---|
|**Control**|High. You control the creation logic.|Low. Spring manages creation.|
|**Use Case**|**Third-party code.** (You cannot add `@Component` to a library class like `java.sql.DataSource` or `ObjectMapper`).|**Your own code.** (Classes you write and own).|
|**Verbosity**|Verbose. Requires a method for every bean.|Concise. Just add an annotation.|
# Bean Scopes
We have established how to create beans and how to inject them. Now we must ask: **How long do they live, and how many of them are there?**
This is defined by the **Scope** of the bean.
## 1. Singleton Scope (The Default)
Unless you specify otherwise, every Spring Bean is a **Singleton**.
- **Definition:** The container creates **exactly one** instance of the bean per bean definition ID.
- **Behavior:** Every time you inject this bean into another class, or ask the container for it, you get the _exact same reference_ to the same object in memory.

**The Danger Zone:** Because Singletons are shared, they must be **Stateless**. If a Singleton bean has a mutable instance variable (like a public `counter` or a `List` that changes), and multiple threads (users) access that bean simultaneously, they will overwrite each other's data. This leads to elusive bugs known as **Race Conditions**.
## 2. Prototype Scope
- **Definition:** The container creates a **new** instance every time the bean is requested or injected.
- **Behavior:** If Bean A and Bean B both depend on Bean C (Prototype), they each get their _own_ private copy of Bean C.
```java
@Component
@Scope("prototype") // or @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public class Ticket { ... }
```

# The Bean Lifecycle
You now know how to define a bean, inject it, and scope it. But what actually happens inside the container when you start your application?

The life of a Bean is not just "New Object created." It is a strictly ordered ceremony. If you try to use a dependency before this ceremony is complete, you will encounter errors.

## The Three Phases of Birth
1. **Instantiation:**
    - Spring finds the `@Component` or `@Bean` definition.
    - It calls the **Constructor** of the class to create the raw Java object in memory.
    - _Note:_ At this exact moment, the dependencies (unless injected via constructor) are still `null`.
2. **Populate Properties (Dependency Injection):**
    - Spring scans for `@Autowired` on setters or fields.
    - It injects the required dependencies.
3. **Initialization:**
    - The object exists and has dependencies, but it might need to perform setup logic (e.g., "Connect to the database now that I have the URL," or "Load the cache").
    - Spring looks for a method annotated with **`@PostConstruct`**.
    - This method runs **once**, automatically, after everything is ready.

## The Phase of Death
- **Destruction:**
    - When the application shuts down (`context.close()`), Spring looks for a method annotated with **`@PreDestroy`**.
    - This is where you clean up resources (close open files, stop background threads).
```java
@Component
public class DatabaseLoader {

    private final UserRepository repo;

    // Phase 1: Instantiation (Constructor)
    public DatabaseLoader(UserRepository repo) {
        this.repo = repo;
        System.out.println("1. Constructor called. Repo is set.");
    }

    // Phase 3: Initialization
    @PostConstruct
    public void init() {
        // This runs AFTER the constructor and AFTER dependency injection
        System.out.println("2. PostConstruct called. Loading initial data...");
        repo.save(new User("Admin"));
    }

    // Phase 4: Destruction
    @PreDestroy
    public void cleanup() {
        System.out.println("3. PreDestroy called. Application shutting down.");
    }
}
```

**Why this matters:** A common mistake is trying to use a dependency inside the **Constructor** for logic that requires the full environment to be ready. While Constructor Injection makes the dependency available, other container features (like AOP proxies or specific property values) might not be fully settled until the Initialization phase. **Logic goes in `@PostConstruct`, not the constructor.**

# Advanced Wiring (Handling Ambiguity)
So far, our examples have been simple: One Interface (`Engine`), One Implementation (`ElectricEngine`). But in enterprise systems, you often have **multiple implementations** of the same interface.

**The Scenario:** Imagine you have an interface `NotificationService`. You have two `@Component` classes that implement it:
1. `EmailNotificationService`
2. `SMSNotificationService`

If you try to inject `NotificationService` into a Controller:
```java
@Autowired
private NotificationService service; // ERROR!
```

Spring will throw a **`NoUniqueBeanDefinitionException`**. It is saying: _"I found two matching beans. I do not know which one to give you."_

To solve this, we use **`@Primary`** or **`@Qualifier`**.
## 1. `@Primary` (The Default)
You place this annotation on _one_ of the implementation classes. It tells Spring: "If there is ambiguity, pick me unless specified otherwise."
```java
@Component
@Primary
public class EmailNotificationService implements NotificationService { ... }

@Component
public class SMSNotificationService implements NotificationService { ... }
```
Now, simply asking for `NotificationService` injects the **Email** version.

## 2. `@Qualifier` (The Specific)

Sometimes you need the non-primary bean, or you need to be explicit. You give the bean a name (by default, the class name in camelCase) and ask for it by that name.
```java
@Service
public class AlertSystem {
    
    private final NotificationService service;

    public AlertSystem(@Qualifier("SMSNotificationService") NotificationService service) {
        this.service = service;
    }
}
```
**Note:** `@Qualifier` is stronger than `@Primary`. If you use `@Qualifier`, Spring ignores `@Primary` and fetches the specific bean you asked for.