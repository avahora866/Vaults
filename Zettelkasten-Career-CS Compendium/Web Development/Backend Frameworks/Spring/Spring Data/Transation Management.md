---
tags:
  - FORMAT
  - Spring
---
# Transaction Management in Spring Data

Transaction management is crucial for ensuring data consistency and integrity in an application. Spring Data provides advanced transaction management using the `@Transactional` annotation and integrates with the underlying JPA (Java Persistence API) entity manager.

---

# Advanced Transaction Handling with Spring Data

## Using `@Transactional` Across Multiple Repositories and Methods

Spring allows managing transactions across multiple repositories and service methods using the `@Transactional` annotation.

### Key Concepts

* **Declarative Transactions**: Using `@Transactional`, Spring handles transactions automatically.  
* **Propagation Behavior**: Determines how transactions behave when calling multiple methods.  
* **Rollback Policies**: Specifies conditions under which transactions should be rolled back.

### Example: Transaction Across Multiple Repositories

```java
@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private PaymentRepository paymentRepository;

    @Transactional
    public void placeOrder(Order order, Payment payment) {
        orderRepository.save(order);
        paymentRepository.save(payment);
        // If any exception occurs, the transaction will roll back.
    }
}
```

* If an exception occurs in `paymentRepository.save(payment)`, the entire transaction is rolled back, including `orderRepository.save(order)`.

---

## Understanding `EntityManager` and Persistence Context

JPA’s `EntityManager` and the Persistence Context play a critical role in managing entity states and transactions.

### What is an EntityManager?

* The `EntityManager` is responsible for database interactions, such as persisting, finding, and removing entities.  
* It is used internally by Spring Data repositories.

### What is the Persistence Context?

* The **persistence context** is a cache that holds entity instances managed by the `EntityManager`.  
* It ensures that entities are automatically tracked and synchronized with the database.

### Example: How EntityManager Works

```java
@PersistenceContext
private EntityManager entityManager;

@Transactional
public void updateProduct(Long productId, String newName) {
    Product product = entityManager.find(Product.class, productId);
    product.setName(newName); // No need to call entityManager.merge(product)
}
```

* Since the `product` is managed in the persistence context, any changes made to it within the transaction will be automatically persisted when the transaction commits.

---

## How Persistence Context Works in Transactions

The persistence context ensures that:

* Entities retrieved within a transaction remain **managed**.  
* Changes made to managed entities are **automatically synchronized** when the transaction commits.  
* Queries within the same persistence context return the **same entity instance**.

### Example: Persistence Context in Action

```java
@Transactional
public void demoPersistenceContext(Long id) {
    Product product1 = entityManager.find(Product.class, id);
    Product product2 = entityManager.find(Product.class, id);
    
    System.out.println(product1 == product2); // true, both refer to the same entity instance
}
```

* The same instance is returned due to persistence context caching.

---

## Transaction Propagation Strategies

Spring provides various propagation options in `@Transactional`:

| Propagation Type | Description |
| ----- | ----- |
| `REQUIRED` (Default) | Uses the existing transaction or starts a new one if none exists. |
| `REQUIRES_NEW` | Suspends the existing transaction and starts a new one. |
| `NESTED` | Creates a nested transaction within an existing transaction. |
| `SUPPORTS` | Runs within a transaction if one exists but doesn’t create a new one. |
| `NOT_SUPPORTED` | Runs outside of any transaction. |
| `NEVER` | Ensures that no transaction is running. |
| `MANDATORY` | Requires an existing transaction, or it throws an exception. |

### Example: `REQUIRES_NEW` vs. `REQUIRED`

```java
@Service
public class TransactionService {

    @Autowired
    private OrderRepository orderRepository;

    @Transactional
    public void mainTransaction() {
        orderRepository.save(new Order("Main Order"));
        subTransaction();  // Called within the same transaction
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void subTransaction() {
        orderRepository.save(new Order("Sub Order"));
        throw new RuntimeException("Rollback sub-transaction"); 
    }
}
```

* With `REQUIRES_NEW`, the sub-transaction rolls back, but the main transaction remains unaffected.  
* With `REQUIRED`, both transactions would roll back.

---

## Common Issues and Best Practices

### Common Issues

* **LazyInitializationException**: When an entity is fetched lazily but accessed outside the transaction.  
* **Incorrect Propagation Usage**: Not using `REQUIRES_NEW` when required.  
* **Mixing Spring and JPA Transactions**: Spring transactions should be preferred over manual JPA ones.

### Best Practices

* Use `@Transactional` only at the **service layer**.  
* Choose the correct **propagation strategy**.  
* Keep transactions **short-lived** to avoid locking issues.  
* Avoid **manual `flush()` calls** unless necessary.

---

## Conclusion

Spring Data provides powerful transaction management with `@Transactional`. Understanding the **EntityManager**, **persistence context**, and **transaction propagation** is key to writing efficient and reliable transactional code. By following best practices, you can ensure consistency and avoid common pitfalls in transaction handling.

