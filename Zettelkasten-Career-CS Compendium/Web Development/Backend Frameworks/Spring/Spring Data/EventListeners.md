---
tags:
  - FORMAT
  - Spring
---
Event listeners in Spring Data help execute logic before or after certain lifecycle events occur on entities, such as saving, updating, or deleting. These listeners can be broadly categorized into two types:

1. **JPA Entity Lifecycle Callbacks** (JPA Annotations)

   * Defined within the entity or a separate entity listener class.  
   * Triggered automatically by JPA when an entity undergoes specific operations.  
   * Common annotations include:  
     * `@PrePersist` – Before saving a new entity  
     * `@PostPersist` – After saving a new entity  
     * `@PreUpdate` – Before updating an entity  
     * `@PostUpdate` – After updating an entity  
     * `@PreRemove` – Before deleting an entity  
     * `@PostRemove` – After deleting an entity  
     * `@PostLoad` – After loading an entity from the database  
2. **Example Usage:**

```java
@Entity
@EntityListeners(MyEntityListener.class) // External listener
public class User {
    @Id @GeneratedValue
    private Long id;
    private String name;
}

public class MyEntityListener {
    @PostPersist
    public void afterSave(User user) {
        System.out.println("User saved: " + user.getName());
    }
}
```

3. **Spring Data-Specific Event Listeners**

   * More flexible and work outside of JPA.  
   * Based on the Spring event system (`ApplicationEventPublisher`).  
   * Events include:  
     * `BeforeConvertEvent` – Before converting an object to a database entity  
     * `BeforeSaveEvent` – Before persisting an entity  
     * `AfterSaveEvent` – After persisting an entity  
     * `BeforeDeleteEvent` – Before deleting an entity  
     * `AfterDeleteEvent` – After deleting an entity  
4. **Example Usage (Spring Data Listener):**

```java
@Component
public class UserEventListener {
    @EventListener
    public void handleBeforeSave(BeforeSaveEvent<User> event) {
        System.out.println("Before saving user: " + event.getEntity().getName());
    }
}
```

# When to Use Each Approach?

* **Use JPA Callbacks** when you need lifecycle hooks that are closely tied to entity state (e.g., auditing, validation).  
* **Use Spring Data Event Listeners** when you need a decoupled approach (e.g., integration with other Spring components, cross-cutting concerns like notifications).