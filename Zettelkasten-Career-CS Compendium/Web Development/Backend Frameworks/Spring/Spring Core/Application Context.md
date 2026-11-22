---
tags:
  - Spring
  - SECONDARY_BRANCH
---
# The Hierarchy: `BeanFactory` vs. `ApplicationContext`
The Spring Container has two layers.
1. **`BeanFactory`**: This is the root interface. It provides the basic configuration mechanism. It is **Lazy**. It only creates a bean when you ask for it (`getBean()`). It is rarely used in modern applications unless memory is extremely constrained (like IoT devices).
2. **`ApplicationContext`**: This is a sub-interface of `BeanFactory`. It is **Eager** (instantiates Singletons on startup). It adds enterprise-specific features.
Equation of Inheritance:

$$ApplicationContext = BeanFactory + Events + Resources + I18n + AOP$$
# How to Access the ApplicationContext
If you are inside a Spring Bean (like a Service or Controller), you can simply ask Spring to give you a reference to the container itself.

**Method A: Standard Injection (Recommended)** Since `ApplicationContext` is itself a bean, you can `@Autowired` it.
```java
@Service
public class DiagnosticService {

    private final ApplicationContext context;

    public DiagnosticService(ApplicationContext context) {
        this.context = context;
    }

    public void printStats() {
        int beanCount = context.getBeanDefinitionCount();
        System.out.println("There are " + beanCount + " beans in this container.");
    }
}
```

**Method B: The Aware Interface (Legacy/Infrastructure)** You can implement `ApplicationContextAware`. This is often used in framework code.

```java
@Component
public class MyUtil implements ApplicationContextAware {
    private static ApplicationContext ctx;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) {
        ctx = applicationContext;
    }
}
```

# How to USE It (The Capabilities)
Now that you have the `context` object, what can you actually do with it? Here are the three most powerful use cases.
## Use Case A: Dynamic Bean Retrieval ("The Service Locator")
Normally, you want Spring to inject dependencies automatically. But sometimes, you don't know _which_ bean you need until runtime.

**Scenario:** You have a `PaymentProcessor` interface with two implementations: `StripeProcessor` and `PaypalProcessor`. The user selects their payment method in a dropdown menu on the frontend.

You cannot `@Autowired` just one. You need to pick one dynamically.
```java
public void processPayment(String method, double amount) {
    // "method" string comes from the user (e.g., "stripeProcessor")
    
    // We ask the context specifically for the bean with that name
    PaymentProcessor processor = context.getBean(method, PaymentProcessor.class);
    
    processor.pay(amount);
}
```
_Note: This pattern effectively turns your class into a factory. Use it sparingly, as it couples your code to the Spring Framework.
## Use Case B: The Event System

This is one of the most elegant features of the `ApplicationContext`. It allows beans to communicate without knowing about each other (Decoupling).

**The Concept:** Instead of `RegistrationService` calling `EmailService.sendWelcomeEmail()`, the RegistrationService simply shouts "I just registered a user!" into the Context. The Context then notifies anyone who cares.

**Publisher:**
```java
public void registerUser(User user) {
    repo.save(user);
    // Publish an event to the ApplicationContext
    context.publishEvent(new UserRegisteredEvent(this, user));
}
```
**Listener:**
```java
@Component
public class WelcomeEmailSender {
    
    @EventListener
    public void handleRegistration(UserRegisteredEvent event) {
        // Automatically triggered when the event is published
        sendEmail(event.getUser().getEmail());
    }
}
```

## Use Case C: Accessing Resources
The `ApplicationContext` is also a `ResourceLoader`. It provides a standardized way to read files from the classpath, file system, or URLs without using standard Java `File` IO boilerplate.
```java
public void loadConfig() {
    // The "classpath:" prefix is understood by the Context
    Resource resource = context.getResource("classpath:config/settings.json");
    File file = resource.getFile();
    // Read the file...
}
```